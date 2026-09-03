i want to create notes apps called superNotes I want the app to have this things

1- dashboard with folder and file management dark mode and white mode and settings and navegation and importing pdf/word/pptx creating new notes with infenite canvas or with pages landscape/ portrait with templates paper color, line color , size (A4, A3  letter) , title,  and covers folder & note rename delete and move

2-  in canvas page i want taps to navigate between notebooks and i want for each notebook its pages and the ability to move vertical and horizontal and 2 moveable toolbars and i want them to be attatchable to any edge of the screen and changeable in style and changeable in tools

3- firs toolbar is splited and has this tools (layers "where i can add layers to draw on and make it unseen or move a layer on top of another or lock a layer or dublicate a layer or cleare a layer "/ search "to find any text from my notebook " record "you can record your voice while the session and listen to your voice and see how you wrote the session again"/ browser "where i can get any material and paste it to my notes like images and clear image background" thumbnails "shows sidebar that contains thumbnails for all pages and bookmarks and notes and outlines where you can do add page before or after or duplicate or copy page or add page outline or export to (pdf/ image )"/ add page "where i can add page with custom template or import a file from the storage or adding an image as a  page "/ timer "where i can make a timer while i am working " undo / redo / more button "to open a popup menu that has 4 taps (common/ canvas/ writing /more) /common has : export, add page outline , import pdf, turning pages vertical and horizontal ,sidebar position right or left , pin toolbar to top , custom toolbar/ canvas has add page setting , copy page , expand page (none- left -right - both sides , expansion settings ) / writing has : use pencil, shape recognation , stroke thickness flows canvas zoom , real time handwriting beautification , highlighter blending (automatic-normal-multiply-screen)  ")

4- the secound toolbar has (pen " has stability, tip, seesitivity, thickness, concentration ,handwriting prediction, Real-time handwriting beautification , color palette "/ ballpoint pen "has stability ,thickness concentration, normal line , dotted line , dashed line ,handwriting prediction, Real-time handwriting beautification, color palette" /technical pen " has thickness , concentration linestyle (normal- dash- wavy- zigzag- double lines), horizontal drawing toggle, color palette"/pencil "has stability ,thickness concentration, normal line , dotted line , dashed line ,handwriting prediction, Real-time handwriting beautification, color palette"/highlighter "stability, thickness, concentration , draw straight line , line type (normal- dotted- dashed), color palette "/ eraser" has stroke , precision , lasso, thickness , pressure sensitivity , automatic deselect after use, erase highlighter only , erase tapes , sensitivity , clear handwriting in current layer "/ lasso" has free mode , boxed mode , screanshoot mode , handwrite, highlighter, text , image , tape " / laser "has not tail , tail , thickness , duration , color palette "/ ruler /3 thickness buttons/ line type/ color palette/ reading mode /text "has alignment , font size , H1,2,3,4 , B , I , U, T , ol , li " / image and video adding/ stickers/ tape "has draw, straight line , rectangle , thickness , pattern , color palette , all hidden , all display "/ handwriting beautification "has writing font, snapping intensity (weak-standard-strong), dynamic bold ,writing language (auto- arabic- english ... etc ), unify font Size & line spacing toggle that has (font size - font spacing )") every tool you can select is by taping on it once then you can adjust it by tap twice to make a popup menu appear on top of the tool to edit its details

5- i want to use the fastest language to handle this things or use hyprid option to handle this things i want the ability to use the app on any android device with 2gb ram and i want the storage of the app to be in the internal storage of the device download directory

6- i want writing to be like real i want to fell like i draw using real pen like in notability and noteshelf3 and goodnotes i dont want to feel like its not real writing

i want you to know that i have android studio and this is my environment
PS D:\Projects\Trae> # Check Java
>> Write-Host "--- Java ---" -ForegroundColor Cyan; java -version
>>
>> # Check Kotlin
>> Write-Host "`n--- Kotlin ---" -ForegroundColor Cyan; kotlinc -version
>> # Check Android Studio
>> Write-Host "`n--- Android Studio ---" -ForegroundColor Cyan
>> if (Test-Path "C:\Program Files\Android\Android Studio") {
>>     Write-Host "Android Studio is installed." -ForegroundColor Green
>> } else {
>>     Write-Host "Android Studio not found in default path." -ForegroundColor Yellow
>> }
>>
>> # Check ADB
>> Write-Host "`n--- ADB ---" -ForegroundColor Cyan; adb --version
>>
>> # Check Gradle (Will likely throw an error, which is fine)
>> Write-Host "`n--- Gradle ---" -ForegroundColor Cyan; try { gradle -v } catch { Write-Host "Gradle not in PATH (Use ./gradlew in your project instead)." -ForegroundColor Yellow }
rite-Host "Gradle not in PATH (Use ./gradlew in your project instead)." -ForegroundColor Yellow };bbf4ed32-235c-422a-81de-364c29d03b1c--- Java ---
java version "20.0.1" 2023-04-18
Java(TM) SE Runtime Environment (build 20.0.1+9-29)
Java HotSpot(TM) 64-Bit Server VM (build 20.0.1+9-29, mixed mode, sharing)

--- Kotlin ---
info: kotlinc-jvm 2.4.10 (JRE 21.0.6+-13368085-b895.109)

--- Android Studio ---
Android Studio is installed.

--- ADB ---
Android Debug Bridge version 1.0.41
Version 37.0.1-15733141
Installed as C:\Users\Moaz\AppData\Local\Android\Sdk\platform-tools\adb.exe
Running on Windows 10.0.19045

--- Gradle ---
Gradle not in PATH (Use ./gradlew in your project instead).

PS D:\Projects\Trae> $gradlePath = "$env:USERPROFILE\.gradle\wrapper\dists"; if (Test-Path $gradlePath) { Get-ChildItem $gradlePath -Directory | Select-Object -ExpandProperty Name } else { echo "Not found" }
bin
gradle
gradle-6.8.3-bin
gradle-7.4.2-all
gradle-8.11.1-bin
gradle-8.7-bin
gradle-8.9-bin
gradle-9.1.0-all
gradle-9.3.1-all
gradle-9.3.1-bin
gradle-9.6.0-bin

and i want to use githup to save my work and prevent any code loss
and i have githup cli

and this is the tablet we will test on
PS D:\Projects\Trae> adb devices
* daemon not running; starting now at tcp:5037
* daemon started successfully
  List of devices attached
  R52MC0B6KDT     device

and i want fast development

Act as a Senior Android Developer and Technical Architect to lead the development of "superNotes," a high-performance handwriting and note-taking application. Your goal is to provide optimized code, architectural guidance, and implementation strategies that prioritize low-latency drawing, efficient memory management (specifically for 2GB RAM devices), and a rich feature set.

# Objectives
- Develop a robust Android application using the user's existing environment (Java 20, Kotlin 2.4.10, Android Studio).
- Implement a realistic, low-latency writing engine comparable to Notability or Goodnotes.
- Create a modular UI with a dashboard and a complex, customizable canvas with dual toolbars.
- Ensure efficient file management and storage in the device's internal Download directory.
- Maintain version control using GitHub CLI.

# Project Requirements

### 1. Dashboard & File Management
- Folder and file management system.
- Dark and Light mode support.
- PDF, Word, and PPTX import capabilities.
- Note creation with:
    - Infinite canvas or fixed pages (A4, A3, Letter).
    - Landscape and Portrait orientations.
    - Custom templates, paper colors, and line colors.
    - Titles and customizable folder/note covers.
    - Rename, delete, and move functionality.

### 2. Canvas & Navigation
- Tabbed interface for switching between notebooks.
- Vertical and horizontal page navigation.
- Two moveable, attachable toolbars (snap to any edge).
- Customizable toolbar styles and tool layouts.

### 3. Toolbar 1 (Utility & Management)
- **Layers:** Add, hide, move, lock, duplicate, and clear.
- **Search:** Global text search within notebooks.
- **Recording:** Audio recording synced to handwriting timestamps for playback.
- **Browser:** Integrated browser for material clipping with background removal for images.
- **Thumbnails:** Sidebar for bookmarks, outlines, and page management (add/duplicate/export to PDF/Image).
- **Add Page:** Custom templates, file imports, or image-to-page.
- **Timer:** Integrated productivity timer.
- **System:** Undo/Redo and a "More" popup menu with four tabs:
    - **Common:** Export, outlines, import, orientation, sidebar position, toolbar pinning.
    - **Canvas:** Page settings, copy, expansion (none, left, right, both).
    - **Writing:** Pencil mode, shape recognition, stroke thickness, zoom, real-time beautification, highlighter blending (Automatic, Normal, Multiply, Screen).

### 4. Toolbar 2 (Drawing Tools)
- **Tool Logic:** Single tap to select, double tap for a detail popup menu.
- **Pen Types:**
    - Pen (Stability, sensitivity, thickness, concentration).
    - Ballpoint (Dotted/dashed options).
    - Technical Pen (Wavy, zigzag, double lines, horizontal toggle).
    - Pencil (Texture and pressure simulation).
    - Highlighter (Straight line mode, blending).
- **Eraser:** Stroke, precision, lasso, thickness, and "erase highlighter only" or "erase tapes" modes.
- **Lasso:** Free-form, box, and screenshot modes; filter by handwriting, text, image, etc.
- **Laser:** Tail/no-tail options, duration, and color.
- **Text:** Rich text editing (H1-H4, Bold, Italic, Underline, Lists).
- **Handwriting Beautification:** Writing font conversion, snapping intensity, dynamic bold, multi-language support, and font/spacing unification.
- **Additional:** Ruler, stickers, tape (patterns, hiding/displaying), and media adding (image/video).

# Steps

1. **Reasoning Phase**: Analyze the performance requirements for a 2GB RAM device. Evaluate the best rendering approach (e.g., Custom Views with Canvas API vs. OpenGL/Vulkan for low-latency drawing). Determine the most efficient way to handle infinite canvas memory (tiling/chunking).
2. **Architecture Definition**: Design a modular MVVM or MVI architecture. Define the data layer for local storage and the repository pattern for file management.
3. **Core Engine Implementation**: Develop the low-latency drawing engine first, focusing on the "real writing feel" and pressure sensitivity.
4. **UI/UX Development**: Build the dashboard and the dual-toolbar canvas system using Jetpack Compose or XML (based on performance reasoning).
5. **Feature Integration**: Incrementally add the layers, recording, and beautification features.
6. **Git Workflow**: Provide commands for GitHub CLI to commit and push progress regularly.
