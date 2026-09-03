# RFC: AuraNotes System Architecture & Technical Design

**Document Status:** 🟢 APPROVED FOR IMPLEMENTATION
**Author:** Manus (Principal Software Architect)
**Architecture Pattern:** Clean Architecture + Unidirectional Data Flow (UDF)
**Target Constraints:** Strict 150MB Heap Limit (2GB RAM Devices)

---

## 1. High-Level System Architecture

To guarantee zero-lag rendering and prevent `OutOfMemoryError` (OOM) crashes, AuraNotes strictly enforces **Clean Architecture** with isolated layers. The UI never communicates directly with the file system or database.

### Layer Interaction Diagram

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                          PRESENTATION LAYER                             │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────┐  │
│  │ Jetpack Compose │    │  SurfaceView    │    │    ViewModel        │  │
│  │ (Toolbars, UI)  │◄───┤ (Jetpack Ink)   │◄───┤ (StateFlow / UDF)   │  │
│  └────────┬────────┘    └────────┬────────┘    └──────────┬──────────┘  │
└───────────┼──────────────────────┼────────────────────────┼─────────────┘
            │ Events               │ Touch Input            │ State
┌───────────▼──────────────────────▼────────────────────────▼─────────────┐
│                             DOMAIN LAYER                                │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────┐  │
│  │ InkProcessor    │    │ AudioSyncEngine │    │ MLBeautification    │  │
│  │ (Viewport Cull) │    │ (Timestamping)  │    │ (Digital Ink API)   │  │
│  └────────┬────────┘    └────────┬────────┘    └──────────┬──────────┘  │
└───────────┼──────────────────────┼────────────────────────┼─────────────┘
            │                      │                        │
┌───────────▼──────────────────────▼────────────────────────▼─────────────┐
│                              DATA LAYER                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────┐  │
│  │ Room Database   │    │ FileRepository  │    │ AudioRepository     │  │
│  │ (Metadata/Tags) │    │ (.auranote I/O) │    │ (MediaRecorder)     │  │
│  └─────────────────┘    └─────────────────┘    └─────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Data Layer Contracts (Kotlin Definitions)

The Data Layer abstracts the complexity of the `.auranote` zip format and the SQLite metadata index.

### 2.1 Local Room Entities (Metadata Index)
```kotlin
@Entity(tableName = "notes")
data class NoteEntity(
    @PrimaryKey val id: String = UUID.randomUUID().toString(),
    val title: String,
    val folderId: String?,
    val createdAt: Long,
    val updatedAt: Long,
    val canvasType: CanvasType, // INFINITE or FIXED
    val filePath: String // URI to the .auranote file in Downloads
)
```

### 2.2 Data Transfer Objects (DTOs for `.auranote` serialization)
```kotlin
@Serializable
data class StrokeDto(
    val id: String,
    val toolType: ToolType, // PEN, HIGHLIGHTER, ERASER
    val colorHex: String,
    val thickness: Float,
    val points: List<PointDto>, // X, Y, Pressure
    val audioTimestampMs: Long? = null // For audio syncing
)

@Serializable
data class PointDto(val x: Float, val y: Float, val pressure: Float)
```

### 2.3 Repository Interfaces
```kotlin
interface NoteRepository {
    fun observeAllNotes(): Flow<List<NoteEntity>>
    suspend fun createNote(note: NoteEntity): Result<Unit>
    suspend fun saveStrokesToFile(noteId: String, strokes: List<StrokeDto>): Result<Unit>
    suspend fun loadStrokesFromFile(noteId: String, viewportBounds: RectF): Result<List<StrokeDto>>
}

interface AudioRepository {
    fun startRecording(noteId: String): Flow<Long> // Emits elapsed MS
    suspend fun stopRecording(): Result<String> // Returns audio file URI
    fun playAudio(uri: String): Flow<Long> // Emits playback MS for UI syncing
}
```

---

## 3. State Flow & UDF Mapping

We utilize the **MVI (Model-View-Intent)** pattern via Kotlin `StateFlow`. The UI is a pure function of the state.

### 3.1 UI State Definition
```kotlin
data class CanvasUiState(
    val isLoading: Boolean = true,
    val activeTool: ToolType = ToolType.PEN,
    val visibleStrokes: List<Stroke> = emptyList(), // Jetpack Ink Stroke objects
    val audioState: AudioState = AudioState.IDLE,
    val currentAudioTimeMs: Long = 0L,
    val error: CanvasError? = null
)
```

### 3.2 Intent (Events)
```kotlin
sealed class CanvasIntent {
    data class OnStrokeDrawn(val stroke: Stroke) : CanvasIntent()
    data class OnViewportChanged(val newBounds: RectF) : CanvasIntent()
    object ToggleAudioRecording : CanvasIntent()
    data class ApplyBeautification(val targetStrokeId: String) : CanvasIntent()
}
```

### 3.3 ViewModel Flow
1. **Event:** User pans the screen -> `CanvasIntent.OnViewportChanged(bounds)`.
2. **UseCase:** `ViewModel` calls `GetVisibleStrokesUseCase(bounds)`.
3. **Data:** Repository queries the spatial hash index and loads only strokes within `bounds`.
4. **State:** `ViewModel` updates `_uiState.update { it.copy(visibleStrokes = newStrokes) }`.
5. **UI:** `SurfaceView` re-renders instantly.

---

## 4. Concurrency & Memory Management

Given the strict **150MB heap limit**, concurrency and memory allocation must be surgically controlled.

### 4.1 Threading Strategy (Coroutines)
*   **`Dispatchers.Main`**: Strictly reserved for Jetpack Compose UI updates and `SurfaceView` invalidation.
*   **`Dispatchers.IO`**: Used for reading/writing the `.auranote` zip files and Room DB queries.
*   **`Dispatchers.Default`**: Used for heavy CPU tasks: ML Kit handwriting recognition, Bezier curve smoothing, and QuadTree spatial hashing calculations.

### 4.2 The "Infinite Canvas" Memory Strategy
We cannot keep an infinite canvas in RAM. We implement **Spatial Hashing (QuadTree)**:
1. The canvas is divided into a grid of "Chunks" (e.g., 1024x1024 pixels).
2. As the user draws, strokes are assigned to their respective Chunk IDs.
3. When the user pans, the `InkProcessor` calculates which Chunks intersect the screen.
4. Off-screen Chunks are serialized to the local `.auranote` file and **dropped from memory**.
5. On-screen Chunks are deserialized and rendered.

---

## 5. Failure Modes & Mitigation Techniques

| Failure Mode | Root Cause | Mitigation Strategy |
| :--- | :--- | :--- |
| **OOM Crash during PDF Import** | Loading a 100-page PDF into Bitmaps exceeds 150MB heap. | Use `PdfRenderer`. Render only the currently visible page + 1 adjacent page. Recycle Bitmaps using an `inBitmap` object pool. |
| **Audio Timestamp Drift** | `System.currentTimeMillis()` drifts during long recordings. | Use `SystemClock.elapsedRealtime()` tied to the `AudioRecord` buffer frame count to guarantee frame-perfect sync. |
| **Concurrent File Write Corruption** | Auto-save triggers while the user is actively drawing. | Implement a `Mutex` lock in the `FileRepository`. Write to a `.tmp` file first, then perform an atomic file rename to `.auranote`. |
| **UI Stutter (Jank)** | Garbage Collection (GC) pauses caused by object allocation in `onDraw`. | **Zero Allocation Rule:** Pre-allocate `Paint`, `Path`, and `RectF` objects in the `SurfaceView` constructor. Never use `val x = ...` inside the draw loop. |

---
*End of RFC.*