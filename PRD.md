As a Lead Product Manager at a Tier-1 tech company, I accept this challenge. To elevate this to **Google’s elite PRD standards**, we must integrate frameworks that Google uses internally to guarantee product success at scale.

This means adding **OKRs (Objectives and Key Results)**, **CUJs (Critical User Journeys)**, the **HEART Metrics Framework**, **Cross-Functional Requirements (a11y, i18n, Privacy)**, and a strict **Launch & Rollback Strategy** (Dogfooding -> GA).

Because this document will be extremely comprehensive and detailed, I will provide it in chunks to ensure maximum quality without hitting output limits.

Here is **Part 1** of the Master Google-Standard PRD for AuraNotes.

***

# 📄 Master Product Requirement Document (PRD): AuraNotes V1
**Document Status:** 🟡 IN REVIEW
**Target Launch:** Q4 2026
**Lead PM:** Manus (Tier-1 PM)
**Engineering Lead:** [Your Name]
**Target Platform:** Android (Min SDK 24, Target SDK 34)

---

## 1. Executive Summary & Strategic Context

### 1.1 The Problem (The "Why")
The Android tablet ecosystem is heavily fragmented. While flagship devices ($800+) enjoy premium note-taking apps, the massive mid-to-low-tier market (devices with ≤2GB RAM, like the `R52MC0B6KDT` test device) is severely underserved. Existing apps on these devices suffer from critical failures:
1. **Unusable Latency:** Standard Android `Canvas` APIs cause >50ms ink lag.
2. **OOM Crashes:** Infinite canvases and heavy PDFs cause `OutOfMemoryError` crashes on 2GB RAM limits.
3. **Feature Poverty:** Advanced features like audio-syncing and ML-driven beautification are gated behind flagship hardware.

### 1.2 The Solution (The "What")
**AuraNotes** is a hyper-optimized, vector-based note-taking application engineered specifically for memory-constrained Android devices. By bypassing standard Android UI rendering in favor of **Jetpack Ink (C++ backed)** and utilizing aggressive spatial hashing (viewport culling), AuraNotes delivers a zero-lag, "real pen" experience alongside premium features like audio-synced strokes and ML Kit handwriting beautification.

### 1.3 Strategic Alignment (The "Why Now")
With the recent stabilization of Google's `androidx.ink` library (1.0.0) and on-device ML Kit, it is now technically feasible to deliver flagship-level rendering on budget hardware without cloud compute costs. AuraNotes will capture the budget-student and emerging-market demographics before competitors adapt their legacy codebases.

---

## 2. Objectives & Key Results (OKRs)

To ensure alignment between Product and Engineering, V1 success is defined by the following OKRs:

**Objective 1: Deliver a flawless, zero-lag writing experience on 2GB RAM devices.**
*   **KR 1.1:** Achieve a p95 touch-to-display ink latency of **< 16ms** (maintaining 60fps).
*   **KR 1.2:** Maintain a strict heap memory footprint of **< 150MB** during infinite canvas panning.
*   **KR 1.3:** Achieve an OOM (Out of Memory) crash rate of **< 0.05%** in production.

**Objective 2: Drive high user retention through premium, differentiated features.**
*   **KR 2.1:** Achieve a D1 retention rate of **> 45%** and D7 of **> 20%**.
*   **KR 2.2:** See **> 30%** of active users utilize the Audio-Sync or ML Beautification features within their first 3 sessions.
*   **KR 2.3:** Keep average `.auranote` file sizes under **50MB** for a 1-hour lecture (audio + strokes).

---

## 3. Target Audience & Critical User Journeys (CUJs)

At Google, we design for CUJs—the absolute most important tasks a user must accomplish flawlessly.

### 3.1 Target Personas
*   **Primary (The Budget Student):** Uses a 2GB RAM tablet. Relies on capacitive touch (fingers or cheap rubber styluses). Needs to record 2-hour lectures while taking notes and importing heavy PDF syllabuses.
*   **Secondary (The Ideating Professional):** Uses the app for mind-mapping. Requires an infinite canvas, UI customizability (movable toolbars), and instant background removal for clipped web images.

### 3.2 Critical User Journeys (CUJs)
*   **CUJ 1: The Synchronized Lecture (P0)**
    *   *Journey:* User opens app → Creates new note → Snaps Toolbar 1 to the left edge → Taps "Record Audio" → Writes notes for 60 minutes → Stops recording → Plays back audio and watches strokes animate in sync.
*   **CUJ 2: The Heavy PDF Annotation (P0)**
    *   *Journey:* User imports a 100-page PDF syllabus → Engine streams pages into memory without crashing → User highlights text (using Multiply blend mode) → Exports annotated PDF.
*   **CUJ 3: The Messy-to-Clean Conversion (P1)**
    *   *Journey:* User selects Pen → Toggles "AI Beautification" → Writes a messy paragraph quickly → Pauses → ML Kit processes vector data in background → Text snaps into a unified, clean font seamlessly.

---

## 4. Scope Boundaries & Phasing

Explicitly defining what we will *not* build is critical to hitting our launch date.

### 4.1 In-Scope (V1 MVP - Launch)
*   **Core Engine:** Jetpack Ink vector engine (capacitive touch optimized).
*   **Canvas Types:** Infinite canvas (via viewport culling) and fixed A4/A3/Letter pages.
*   **UI Architecture:** Dual movable toolbars (snap to 4 edges) with single-tap select / double-tap settings.
*   **Tool Arsenal:** Pen, Ballpoint, Technical Pen, Pencil, Highlighter (with blending), Eraser (stroke/precision/lasso), Laser, Text, Tape.
*   **Hero Features:** Audio-synced stroke playback (ghosting/animating), Hybrid Beautification (Math smoothing + ML Kit AI font replacement), Image background removal (ML Kit).
*   **Storage:** Local `.auranote` zip-based file format stored in the device's internal `Download` directory.
*   **I/O:** PDF import and export.

### 4.2 Out of Scope (Explicitly Deferred)
*   **Deferred to V1.1:** Word (`.docx`) and PPTX import. *(Reason: Apache POI library causes massive memory spikes; requires a dedicated background worker architecture to be safe on 2GB RAM).*
*   **Deferred to V2.0:** Hardware-specific active stylus APIs (e.g., Samsung S-Pen pressure/tilt SDKs). *(Reason: V1 focuses on perfecting capacitive touch velocity-based pressure).*
*   **Deferred to V2.0:** Cloud synchronization (Google Drive/Dropbox) and real-time multi-user collaboration. *(Reason: Introduces massive scope creep regarding conflict resolution and backend infrastructure).*


## 5. Functional Requirements & Acceptance Criteria (Given-When-Then)

To ensure zero ambiguity for Engineering and QA, all core features are defined using strict BDD (Behavior-Driven Development) criteria.

### Epic 1: Infinite Canvas & Memory Management (P0)
**User Story:** As a user, I want to scroll infinitely without the app crashing so I can build massive mind maps on my 2GB RAM device.
```text
GIVEN the user is drawing on an infinite canvas,
WHEN the user pans the canvas so that existing strokes move outside the visible viewport,
THEN the rendering engine must cull (hide) the off-screen strokes from the active draw loop within 16ms,
AND the app's total heap memory usage must remain strictly under 150MB.

GIVEN the user zooms out to < 10% scale,
WHEN the canvas contains > 5,000 strokes,
THEN the engine must aggregate strokes into simplified LOD (Level of Detail) paths to prevent GPU overload.
```

### Epic 2: Audio-Synced Strokes (P0)
**User Story:** As a student, I want my notes to animate in sync with the lecture audio so I can recall exactly what was said when I wrote a specific word.
```text
GIVEN the audio recording is active,
WHEN the user completes a stroke (ACTION_UP),
THEN the stroke data object must be appended with the exact elapsed millisecond timestamp of the audio track (using SystemClock.elapsedRealtime).

GIVEN the user is playing back a recorded audio session,
WHEN the audio playback reaches timestamp [X],
THEN any strokes tagged with timestamp [X] must animate onto the canvas in real-time (ghosting effect),
AND the user can tap any existing stroke to jump the audio playback to that stroke's timestamp.
```

### Epic 3: Hybrid Handwriting Beautification (P1)
**User Story:** As a user with messy handwriting, I want the app to instantly convert my writing into a neat, unified font.
```text
GIVEN the AI Beautification toggle is active (Language set to Auto, English, or Arabic),
WHEN the user writes a word and pauses for >500ms,
THEN the ML Kit Digital Ink Recognizer must process the vector data on a background Coroutine,
AND replace the handwritten strokes with the unified font text on the UI thread without dropping frame rates.
```

### Epic 4: Toolbar Edge Snapping & Customization (P1)
**User Story:** As a left-handed user, I want to move my toolbars to the right side of the screen so my hand doesn't accidentally trigger tools.
```text
GIVEN the user is on the canvas screen,
WHEN the user long-presses and drags Toolbar 1 or Toolbar 2 to any of the 4 screen edges,
THEN the toolbar must snap to that edge,
AND re-orient its layout (vertical for left/right edges, horizontal for top/bottom edges) automatically.

GIVEN a tool is selected (e.g., Pen),
WHEN the user double-taps the tool icon,
THEN a popup menu must appear above the tool displaying its specific settings (Stability, Thickness, Concentration).
```

### Epic 5: Smart Eraser Filtering (P1)
**User Story:** As a user annotating a PDF, I want to erase my highlighter marks without accidentally erasing my handwritten notes underneath.
```text
GIVEN the Eraser tool is selected with the "Erase Highlighter Only" filter active,
WHEN the user drags the eraser over a bounding box containing both Pen strokes and Highlighter strokes,
THEN only the Highlighter strokes are deleted from the geometry layer, leaving the Pen strokes intact.
```

---

## 6. Cross-Functional Requirements (CFRs)

At Google scale, features are not enough. The app must adhere to strict CFRs.

### 6.1 Security & Privacy
*   **Local-First Architecture:** AuraNotes requires **zero** cloud connectivity. All `.auranote` files are stored locally in the device's `Download` directory.
*   **On-Device ML:** Google ML Kit (Digital Ink & Subject Segmentation) must download models locally. No user handwriting or clipped images are ever sent to external servers.
*   **Permissions:** The app will only request `RECORD_AUDIO` (when the user taps record) and `MANAGE_EXTERNAL_STORAGE` (or Scoped Storage equivalents) for file management.

### 6.2 Accessibility (a11y)
*   **Screen Readers:** All UI elements in Toolbar 1 and Toolbar 2 must have descriptive `contentDescription` tags for TalkBack support.
*   **Contrast:** Dark Mode and Light Mode palettes must meet WCAG 2.1 AA contrast ratios (4.5:1 for normal text/icons).

### 6.3 Internationalization (i18n)
*   **RTL Support:** The UI must fully support Right-to-Left layouts for Arabic users.
*   **ML Kit Languages:** The handwriting beautification engine must support Arabic, English, and Auto-detect out of the box.

---

## 7. Technical Architecture & Data Schema

### 7.1 The `.auranote` File Structure
To keep the user's `Download` folder clean, an `.auranote` file is a renamed `.zip` archive containing:
*   `meta.json`: Document title, creation date, paper color, template type, canvas type (infinite vs fixed).
*   `strokes.json`: Serialized Jetpack Ink geometry data (X, Y, pressure, timestamp, tool type).
*   `/assets/`: Directory containing synced `.m4a` audio files and inserted images/backgrounds.

### 7.2 Core Tech Stack
*   **Language:** Kotlin 2.4.10 / Java 20.
*   **Rendering Engine:** `androidx.ink:ink-nativeloader:1.0.0` (Jetpack Ink C++ backend) on a `SurfaceView`.
*   **Local Database:** `androidx.room` (for indexing files and folder structures).
*   **AI/ML:** `com.google.mlkit:digital-ink-recognition` and `com.google.mlkit:subject-segmentation`.
*   **Concurrency:** Kotlin Coroutines & Flows (`Dispatchers.IO` for saving, `Dispatchers.Default` for ML processing).

---

## 8. Launch & Rollback Strategy

### 8.1 Release Phases
1.  **Dogfooding (Internal Alpha):** Testing strictly on the `R52MC0B6KDT` 2GB RAM device. Focus: OOM crash validation and ink latency profiling.
2.  **Beta (Closed Track):** Rollout to 1,000 users. Focus: Telemetry gathering on different screen sizes and hardware profiles.
3.  **General Availability (GA):** Full Play Store release.

### 8.2 Rollback Triggers
The release will be halted and rolled back if telemetry detects:
*   OOM Crash rate exceeds **0.1%**.
*   Average Canvas Load Time exceeds **3.0 seconds**.
*   Fatal exceptions in the `ZipOutputStream` causing `.auranote` file corruption.

---

## 9. HEART Metrics Framework (Google Standard)

To measure true user success post-launch, we will track:
*   **Happiness:** App Store rating (Target: 4.5+).
*   **Engagement:** Number of strokes drawn per session (Target: >500).
*   **Adoption:** Percentage of users who create more than 1 folder (Target: >40%).
*   **Retention:** D7 Retention (Target: >20%).
*   **Task Success:** Percentage of successful audio recordings saved without interruption (Target: >99%).

