# AuraNotes Design System & Prototyping Guide

**Document Status:** 🟢 APPROVED FOR FIGMA PROTOTYPING
**Lead Designer:** Manus (Principal UI/UX Architect)
**Target Platform:** Android Tablets (Material Design 3 / Adaptive Layouts)

This document serves as the foundational blueprint for the AuraNotes Figma prototype. It strictly adheres to Material Design 3 (MD3) principles, ensures WCAG 2.1 AA accessibility, and enforces Android's 48x48dp minimum touch target constraints.

---

## 1. Color System (Light & Dark Mode)

AuraNotes uses a sleek, professional palette. The primary brand color is a vibrant "Aura Indigo" that provides excellent contrast against both light paper and dark canvas backgrounds.

| Token | Light Mode (HEX) | Dark Mode (HEX) | Usage |
| :--- | :--- | :--- | :--- |
| **Primary** | `#4F46E5` (Indigo 600) | `#818CF8` (Indigo 400) | Active tools, FABs, Primary Buttons |
| **On Primary** | `#FFFFFF` | `#1E1B4B` | Text/Icons placed on Primary color |
| **Secondary** | `#0EA5E9` (Sky 500) | `#38BDF8` (Sky 400) | Accents, Lasso selections, Audio waves |
| **Background** | `#F8FAFC` (Slate 50) | `#0F172A` (Slate 900) | Dashboard background |
| **Surface** | `#FFFFFF` | `#1E293B` (Slate 800) | Cards, Modals, Toolbars |
| **Surface Variant**| `#F1F5F9` (Slate 100) | `#334155` (Slate 700) | Canvas paper background, Hover states |
| **On Surface** | `#0F172A` (Slate 900) | `#F8FAFC` (Slate 50) | Primary text, Default icons |
| **Error** | `#DC2626` (Red 600) | `#F87171` (Red 400) | Destructive actions, Error states |

*Accessibility Note: All Primary and Error colors against Surface/Backgrounds have been verified to exceed the 4.5:1 WCAG 2.1 AA contrast ratio.*

---

## 2. Typography Hierarchy

**Typeface:** `Inter` (Sans-serif, highly legible at small sizes, excellent for UI density).

| Role | Font Size / Line Height | Weight | Usage |
| :--- | :--- | :--- | :--- |
| **Display Large** | 32sp / 40sp | Bold (700) | Empty state hero text, Onboarding |
| **Headline (H1)** | 24sp / 32sp | SemiBold (600) | Dashboard headers, Open Note Title |
| **Title (H2)** | 18sp / 24sp | Medium (500) | Folder names, Modal titles |
| **Body Large** | 16sp / 24sp | Regular (400) | Standard paragraph text, Settings descriptions |
| **Body Medium** | 14sp / 20sp | Regular (400) | Secondary text, Note metadata (date/size) |
| **Label Large** | 14sp / 20sp | Medium (500) | Button text, Tabs, Toolbar tooltips |
| **Label Small** | 12sp / 16sp | Medium (500) | Badges, Tiny metadata |

---

## 3. Key Screen Layouts

### 3.1 The Dashboard (File Management)
*   **Top App Bar:** Search bar (pill-shaped), View Toggle (Grid/List), Settings Icon.
*   **Sidebar (Left - Tablet):** Navigation rail containing "All Notes", "Recent", "Favorites", "Trash".
*   **Main Content Area:** Responsive grid of Folders and Note Cards.
*   **Floating Action Button (FAB):** Bottom-right corner. Large (56x56dp) with a "+" icon for "New Note".

### 3.2 The Canvas (Active Workspace)
*   **Background:** Edge-to-edge Surface Variant (the paper).
*   **Toolbar 1 (Utility):** Floating pill-shaped container. Default snapped to the Left edge (Vertical orientation). Contains Layers, Search, Record, Thumbnails, Undo/Redo, More.
*   **Toolbar 2 (Drawing):** Floating pill-shaped container. Default snapped to the Top edge (Horizontal orientation). Contains Pen, Highlighter, Eraser, Lasso, Text, Colors.
*   **Audio Bar (Conditional):** Appears at the top center when recording/playing audio. Shows waveform and timestamp.

---

## 4. Component Specifications

*Constraint Checklist: All interactive elements MUST have a minimum bounding box of 48x48dp.*

*   **Icon Buttons (Toolbars):**
    *   Visual Size: 24x24dp icon.
    *   Touch Target: 48x48dp (12dp padding on all sides).
    *   Active State: Primary color tint with a 10% opacity Primary background pill.
*   **Note Cards (Dashboard):**
    *   Corner Radius: 16dp (MD3 standard).
    *   Elevation: 1dp (Light mode shadow), 0dp with 1dp border (Dark mode).
    *   Thumbnail Area: 16:9 aspect ratio at the top of the card.
*   **Toolbars (Floating):**
    *   Corner Radius: 24dp (Pill shape).
    *   Elevation: 4dp (Casts a distinct shadow over the canvas).
    *   Padding: 8dp internal padding, 16dp margin from screen edges.
*   **Modals / Bottom Sheets:**
    *   Corner Radius: 28dp (Top-left and Top-right only).
    *   Scrim: 40% opacity black overlay behind the modal.

---

## 5. The 5 Essential UI States (IDEAL Framework)

### 5.1 Screen: The Dashboard
1.  **Initial / Blank State:**
    *   *Visual:* Illustration of a notebook.
    *   *Text:* "Your workspace is empty." (Display Large).
    *   *Action:* Pulsing FAB pointing to "Create your first note".
2.  **Loading / Skeleton State:**
    *   *Visual:* Shimmering gray rectangles in a grid formation mimicking Note Cards. No text.
3.  **Ideal / Populated State:**
    *   *Visual:* Grid of beautifully rendered Note Cards with custom covers and visible titles/dates.
4.  **Error / Failure State:**
    *   *Visual:* Red warning icon.
    *   *Text:* "Storage permission denied. AuraNotes cannot save or load files."
    *   *Action:* Primary button: "Grant Permission".
5.  **Edge / Partial Case:**
    *   *Scenario:* Note title is 100 characters long.
    *   *Visual:* Title text truncates with an ellipsis (`...`) after 2 lines. Card height remains fixed.

### 5.2 Screen: The Canvas
1.  **Initial / Blank State:**
    *   *Visual:* Clean paper background (grid/dots based on template). Toolbars visible.
    *   *Text:* Subtle watermark in the center: "Tap a pen to start writing."
2.  **Loading / Skeleton State:**
    *   *Visual:* Paper background is visible, but strokes are hidden. A subtle, non-blocking linear progress bar at the top edge indicates vector chunks are loading into memory.
3.  **Ideal / Populated State:**
    *   *Visual:* User is actively drawing. Ink is smooth. Toolbars are snapped to edges. Audio waveform is pulsing if recording.
4.  **Error / Failure State:**
    *   *Scenario:* ML Kit fails to recognize messy handwriting for beautification.
    *   *Visual:* Original handwriting remains. A small, non-intrusive toast appears at the bottom: "Could not recognize text. Try writing clearer." with a "Retry" action.
5.  **Edge / Partial Case:**
    *   *Scenario:* User zooms out to 1% on an infinite canvas with 10,000 strokes.
    *   *Visual:* Strokes gracefully degrade into simplified LOD (Level of Detail) lines to maintain 60fps. Toolbars remain fixed at 100% scale relative to the screen.

---
* **Shall we move to Figma?** I can provide a structured prompt to feed into a Figma AI generator (like Wireframe Designer) to instantly build these screens based on this exact spec.
* **Would you like to refine the Toolbars?** We can map out the exact pixel padding and icon order for Toolbar 1 and Toolbar 2.
* **Should we start coding the UI?** I can translate this design system directly into a Jetpack Compose `Theme.kt` file with all the colors and typography defined.