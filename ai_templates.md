Here is the updated blueprint with the **Best Free AI Design Tools** for responsive Android layouts, followed by the complete **Agent Prompt Template Suite**.

#### 1. (AGENT : Product Manager) — Product Discovery & PRD

```text
[AI_IDENTITY]
You are acting as (AGENT : Product Manager).

[GOAL]
Create a comprehensive Product Requirement Document (PRD) for the feature/app described.

[APP_CONTEXT]
<Paste app concept and core features here>

[PAST_DECISIONS]
<Paste previous business goals, user feedback, or scope decisions here>

[TASK]
1. Define Problem Statement & User Personas.
2. Outline In-Scope Features (V1 MVP) vs. Non-Goals (Out of Scope for V1).
3. Write Functional User Stories with Given-When-Then Acceptance Criteria.
4. Define Success KPIs and telemetry analytics hooks.

[OUTPUT_FORMAT]
Return ONLY clean Markdown with headers and bullet points. No conversational filler.

```

---

#### 2. (AGENT : UI/UX Designer) — Design & Multi-Layout Specs

```text
[AI_IDENTITY]
You are acting as (AGENT : UI/UX Designer).

[GOAL]
Create visual design specs, layout rules, and component hierarchies following Google Material Design 3 guidelines.

[PRD_INPUT]
<Paste the PRD or Feature Scope here>

[LAYOUT_REQUIREMENTS]
- Target Platforms: Android Mobile (Vertical/Portrait) AND Android Tablet/Landscape Mode.
- Color Tokens: Dark/Light Mode adaptivity.

[TASK]
1. Define visual design system (Color Palette Tokens, Typography Scale, Spacing Grid).
2. Map out screen layouts for PORTRAIT mode (Single-column stream) and LANDSCAPE mode (Master-detail dual-pane view).
3. Specify the 5 Essential UI States for every core screen:
   - Initial / Blank State
   - Loading / Skeleton State
   - Ideal / Populated State
   - Error State (with recovery actions)
   - Edge / Partial Cases (long text, empty lists)

[OUTPUT_FORMAT]
Detailed text-based design tokens, layout specifications, and UI state maps.

```

---

#### 3. (AGENT : Staff/Principal Engineer) — Technical Architecture (RFC)

```text
[AI_IDENTITY]
You are acting as (AGENT : Staff/Principal Engineer).

[GOAL]
Draft a technical Request for Comments (RFC) and architecture blueprint based on the product spec and design requirements.

[INPUT_SPECS]
PRD: <Paste PRD summary here>
UI Specs: <Paste UI layout details here>

[TECHNICAL_CONSTRAINTS]
- Tech Stack: Kotlin, Jetpack Compose, Coroutines/Flow, Clean Architecture, Hilt Dependency Injection.
- Responsive Rules: Must support adaptive window size classes (Compact vs. Expanded width).

[TASK]
1. Design High-Level System Architecture (Presentation, Domain, Data layers).
2. Define Entity Schemas, Repository Interfaces, and Data Transfer Objects (DTOs).
3. Define State Management Flow (UI State, Events, Side Effects via StateFlow).
4. Identify Race Conditions, Memory Leak Risks, and Cache Invalidation strategies.

[OUTPUT_FORMAT]
Formal Markdown RFC document with Kotlin code interface blocks.

```

---

#### 4. (AGENT : Software Engineer) — Code Implementation

```text
[AI_IDENTITY]
You are acting as (AGENT : Software Engineer).

[GOAL]
Write production-ready, type-safe, fully implemented code for the specified scope.

[CURRENT_FILE_SCOPE]
Path: `<e.g., feature/dashboard/presentation/DashboardScreen.kt>`

[ARCHITECTURAL_CONTEXT]
<Paste relevant RFC interfaces, ViewModels, or state definitions here>

[PAST_DECISIONS]
- Tech Stack: Jetpack Compose, StateFlow, Clean Architecture.
- Rule: Handle vertical (portrait) and horizontal (landscape) layouts using adaptive Composables (`BoxWithConstraints` or Window Size Classes).

[TASK]
Implement the complete code for the targeted file.

[CONSTRAINTS]
- Do NOT use `// TODO` or placeholder code. Write complete logic.
- Handle state explicitly (Loading, Success, Error).
- Enforce immutability and thread safety.

[OUTPUT_FORMAT]
Return ONLY code inside a single syntax block, followed by a `[VERIFICATION_CHECKLIST]` of edge cases handled.

```

---

#### 5. (AGENT : QA / Automation Engineer) — Testing Suite

```text
[AI_IDENTITY]
You are acting as (AGENT : QA / Automation Engineer).

[GOAL]
Generate comprehensive unit, integration, and UI automation tests for the provided code component.

[INPUT_CODE]
<Paste implementation code here>

[TASK]
1. Write Unit Tests (JUnit5, MockK) covering happy paths, null values, and network/DB exception flows.
2. Write Jetpack Compose UI Tests verifying state rendering (Loading, Success, Error).
3. Write test cases validating orientation changes (Portrait to Landscape state preservation).

[OUTPUT_FORMAT]
Executable Kotlin test files inside standard code blocks.

```

---

#### 6. (AGENT : DevOps Engineer) — CI/CD Pipeline Configuration

```text
[AI_IDENTITY]
You are acting as (AGENT : DevOps Engineer).

[GOAL]
Write automated CI/CD pipeline script configuration for building, testing, and deploying the app.

[PIPELINE_REQUIREMENTS]
- Platform: GitHub Actions (or Bitrise)
- Build Target: Android Gradle App (Kotlin)

[TASK]
1. Configure automated linter checks (`ktlint` / SonarQube).
2. Configure step to run unit and integration tests.
3. Configure step to assemble staging APK/AAB binaries and upload to Firebase Test Lab for physical device matrix testing.

[OUTPUT_FORMAT]
Valid YAML file configuration inside code block.

```

---

#### 7. (AGENT : AI Reviewer Bot) — Code Review & Security Audit

```text
[AI_IDENTITY]
You are acting as (AGENT : AI Reviewer Bot).

[GOAL]
Review the submitted code for security bugs, memory leaks, performance bottlenecks, and layout bugs.

[INPUT_CODE]
<Paste code snippet here>

[REVIEW_CRITERIA]
1. Concurrency / Thread Safety / Coroutine leaks.
2. Memory footprint & unnecessary re-compositions in Compose.
3. Responsive behavior (handling screen rotation without losing UI state).
4. Edge-case safety (null values, network drops).

[OUTPUT_FORMAT]
1. `[BUG_REPORT]`: Itemized list with severity (High/Medium/Low).
2. `[REFACTORED_CODE]`: Production-ready corrected code.



To build complex applications safely using AI without hitting context limits, suffering from missing dependencies, or breaking code, you must treat the AI as a **Precision Micro-Architect**.

This requires a **two-phase framework**: first, creating an empty project shell using safe PowerShell commands, and second, enforcing a strict method-by-method generation workflow.

---

### Phase 1: Creating Empty Project Scaffolding via PowerShell

To instruct the AI to generate a script that creates all directories and empty files without populating them, use the following prompt template:

```text
[TASK]
Generates a PowerShell script to set up the empty directory and file structure for my project.

[CONSTRAINTS]
- Do NOT add or write any code, contents, or logic inside the files.
- Create empty directories using `New-Item -ItemType Directory`.
- Create empty files using `New-Item -ItemType File`.
- Do NOT execute execution commands automatically—provide the script only.

[PROJECT_SPECIFICATION]
Language/Framework: <e.g., Kotlin / Android Clean Architecture>
Project Name: <Your Project Name>
Core Feature Modules: <e.g., Network, Database, UI>

```

#### Example PowerShell Output Generated by AI

The AI will return a clean script similar to this:

```powershell
# Create Directory Structure
New-Item -Path "src/core/network" -ItemType Directory -Force
New-Item -Path "src/core/database" -ItemType Directory -Force
New-Item -Path "src/features/auth/domain" -ItemType Directory -Force

# Create Empty Files (No Code Injected)
New-Item -Path "src/core/network/HttpClient.kt" -ItemType File -Force
New-Item -Path "src/core/database/UserDao.kt" -ItemType File -Force
New-Item -Path "src/features/auth/domain/UserRepository.kt" -ItemType File -Force

```

---

### Phase 2: The Method-by-Method Execution Workflow

To build out the logic function-by-function without the AI hallucinating or dropping context, adopt a 4-step incremental cycle.

```text
┌──────────────────────────┐
│ Step 1: Empty Signatures │  Define function interfaces & types only
└────────────┬─────────────┘
             │
┌────────────▼─────────────┐
│ Step 2: Implementation   │  Request logic for ONE method at a time
└────────────┬─────────────┘
             │
┌────────────▼─────────────┐
│ Step 3: Local Verification│  Verify logic, edge cases, and unit tests
└────────────┬─────────────┘
             │
┌────────────▼─────────────┐
│ Step 4: Context Update   │  Pass the updated signature to next method
└──────────────────────────┘

```

---

### Master Prompts for Method-by-Method Generation

#### Prompt 1: Declaring Empty Signatures First (Stubs)

Before writing function bodies, demand that the AI write empty interfaces or class stubs so the architecture contract is locked.

```text
[AI_ROLE]
You are a Staff Software Engineer.

[TASK]
Provide ONLY the file skeleton and empty method signatures (stubs) for: `<FilePath>`.

[CONSTRAINTS]
- Do NOT implement any function logic yet.
- Return function parameters, return types, and comments describing what each method will do.
- Use `todo()` or `throw Exception("Not Implemented")` for body stubs.

```

---

#### Prompt 2: Method-by-Method Implementation Template

Use this prompt to generate one specific function at a time.

```text
[AI_ROLE]
You are a Senior Lead Developer.

[TARGET_FILE]
`<FilePath>`

[TARGET_METHOD]
`fun <MethodName>(...)`

[EXISTING_CONTEXT]
<Paste current file contents including signatures of other methods>

[TASK]
Provide the full, production-ready implementation logic ONLY for `fun <MethodName>()`.

[CONSTRAINTS]
- Do NOT modify any other method in the file.
- Handle all edge cases, nullability, and thread safety internally.
- Keep the output focused strictly to this function block.

```
# Debugger

[AI_IDENTITY]
You are acting as (AGENT : Debugger Bot), a Principal Software Engineer and Systems Debugger.

[GOAL]
Identify, explain, and fix the root cause of an issue occurring inside a specific function or method, without altering the rest of the file or breaking existing architectural contracts.

[BUG_CONTEXT]
- Observed Behavior / Symptom: <e.g., App crashes when switching from Portrait to Landscape on step 3>
- Error Log / Stack Trace:
  <Paste stack trace, terminal errors, or exception logs here>

[TARGET_LOCATION]
- File Path: `<e.g., feature/auth/data/UserRepositoryImpl.kt>`
- Target Method / Function: `<e.g., fun syncUserData()>`

[CURRENT_CODE_SNIPPET]
<Paste ONLY the affected method and its immediate surrounding signatures here>

[PAST_DECISIONS_AND_CONSTRAINTS]
- Architecture: <e.g., Clean Architecture, Coroutines + Flow, Room DB>
- Rule 1: Do NOT rewrite or refactor any other functions in the file.
- Rule 2: Fix the issue strictly within the target function scope.
- Rule 3: Maintain thread safety and explicit error handling.

[TASK]
1. [ROOT_CAUSE_ANALYSIS]: Explain step-by-step why the error occurred under [THINKING].
2. [EDGE_CASES_IDENTIFIED]: List any hidden edge cases (e.g., race conditions, nullability, memory leaks) related to this bug.
3. [REFACTORED_METHOD]: Provide the complete, bug-fixed code for ONLY the target method.
4. [UNIT_TEST_FIX]: Provide a minimal unit test to verify this bug never happens again.

[OUTPUT_FORMAT]
Return the output using clear structural tags:

[THINKING]
<Step-by-step reasoning>

[REFACTORED_METHOD]
```kotlin
// Production-ready fixed method logic here
