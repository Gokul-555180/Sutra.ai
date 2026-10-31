# SUTRA.ai — Comprehensive User Flows

> **Document Version:** 1.0.0  
> **Status:** Foundational Specification  
> **Related Documents:** [PRODUCT_REQUIREMENTS.md](PRODUCT_REQUIREMENTS.md), [FEATURE_SPECIFICATIONS.md](FEATURE_SPECIFICATIONS.md), [ARCHITECTURE.md](../../ARCHITECTURE.md)

This document visualizes and specifies the end-to-end user journeys through SUTRA.ai, detailing step-by-step user interactions, decision branches, system responses, and state transitions.

---

## 1. First-Time Onboarding Flow

When a user launches SUTRA.ai for the first time, the application provides an immediate zero-friction onboarding sequence without requiring account creation or cloud authentication.

```mermaid
flowchart TD
    Start["Launch SUTRA.ai Desktop App"] --> CheckDB{"Local Database Exists?"}
    CheckDB -- No --> InitDB["Run Initial SQLite Migrations (001_initial_schema.sql)"]
    InitDB --> WelcomeModal["Display Welcome & Philosophy Modal"]
    CheckDB -- Yes --> CheckKeys{"Configured AI Providers Exist?"}
    
    WelcomeModal --> ProviderChoice{"Configure AI Provider Now?"}
    ProviderChoice -- "Configure Now" --> OpenSettings["Open Provider Settings Modal"]
    ProviderChoice -- "Skip for Now" --> EmptyDashboard["Load Clean Dashboard with Quick-Start Guide"]
    
    OpenSettings --> SelectProvider["Select Provider (Gemini / Groq / Ollama / OpenAI)"]
    SelectProvider --> EnterKey["Input API Key or Verify Localhost Ollama"]
    EnterKey --> TestConn["Click 'Test Connection' (Vault Storage)"]
    TestConn --> ConnSuccess{"Connection Valid?"}
    
    ConnSuccess -- Yes --> SaveVault["Store in OS Keyring / Credential Manager"]
    SaveVault --> EmptyDashboard
    ConnSuccess -- No --> ShowErr["Show Diagnostic Error & Retry Option"]
    ShowErr --> EnterKey
```

---

## 2. Structured Prompt Creation Flow (SUTRA Architect)

The primary prompt authoring workflow guides the user from raw intention to a structured, model-adapted prompt.

```mermaid
sequenceDiagram
    autonumber
    actor User as User
    participant UI as Architect View
    participant Engine as Prompt Intelligence Engine
    participant LLM as Provider / Local Model
    participant Repo as PromptRepository

    User->>UI: Enter raw thought (e.g. "Python invoice parser")
    User->>UI: Select Domain (Coding) & Target Model (Claude 3.5)
    UI->>Engine: Parse input for structural facets
    Engine->>Engine: Run deterministic keyword & token scan
    
    opt Missing Critical Invariants
        Engine-->>UI: Return Clarification Questions (e.g. Input file type?)
        UI-->>User: Render Interactive Clarification Dialog
        User->>UI: Select options (PDF, structured JSON output)
        UI->>Engine: Send clarification payload
    end

    Engine->>LLM: Synthesize prompt facets (Role, Objective, Constraints)
    LLM-->>Engine: Return structured schema
    Engine->>Engine: Adapt delimiters (<instructions>, XML tags)
    Engine-->>UI: Populate CodeMirror editor with structured prompt
    User->>UI: Refine instructions or inject {{variables}}
    User->>UI: Click "Save to Library"
    UI->>Repo: Create Prompt & Version Snapshot in SQLite
    Repo-->>UI: Confirm save; display success toast
```

---

## 3. Explainable Optimization Flow (SUTRA Optimizer)

Demonstrates how users refactor existing prompts across 10 specialized modes with instant visual diffs and explainability logs.

```mermaid
flowchart TD
    A["User Pastes Existing Prompt or Loads from Library"] --> B["Select Optimization Mode (e.g., 'Precision' or 'Technical')"]
    B --> C["Select Target Model Architecture"]
    C --> D["Click 'Optimize Prompt'"]
    D --> E["Dispatch Request via IAIProvider Abstraction"]
    E --> F["Receive Streamed Optimized Prompt"]
    F --> G["Compute Inline Character & Word Diffs (LCS Algorithm)"]
    F --> H["Extract Structured Rationale / Changelog Items"]
    
    G --> I["Render Side-by-Side Diff View (Red = Del, Green = Add)"]
    H --> J["Render 'Why SUTRA Modified This' Rationale Panel"]
    
    I & J --> K{"User Evaluates Modifications"}
    K -- "Accept / Save" --> L["Save as New Version Snapshot in Library"]
    K -- "Iterate Further" --> M["Undo / Redo or Switch Optimization Mode"]
    M --> B
    K -- "Test in Playground" --> N["Export Directly to Sutra Playground"]
```

---

## 4. Prompt Quality Analysis Flow (SUTRA Analyzer)

Scans a prompt against the 9-dimensional rubric, flagging ambiguities, missing constraints, and conflicting instructions.

```mermaid
flowchart TD
    A["User Inputs Prompt into Analyzer"] --> B["Click 'Analyze Prompt'"]
    B --> C["Step 1: Local Deterministic Rule Scanner (<5ms)"]
    C --> D{"Detects Missing Delimiters or Conflicting Lengths?"}
    D -- Yes --> FlagDeterministic["Flag Deterministic Violations"]
    D -- No --> E["Step 2: Semantic Evaluation via Provider Engine"]
    
    FlagDeterministic --> E
    E --> F["Compute Scores (0-100) across 9 Dimensions"]
    F --> G["Render Diagnostic Quality Card & Radar Chart"]
    G --> H["Render Identified Defect Badges & Auto-Fix Suggestions"]
    
    H --> I{"User Clicks 'Apply Auto-Fix'"}
    I -- Yes --> J["Apply Specific Patch to Prompt Body"]
    J --> B
    I -- No --> K["User Manually Edits Prompt or Exports Report"]
```

---

## 5. Comparative Playground Experimentation Flow

Enables developers to empirically benchmark prompt variants across multiple foundational models in parallel.

```mermaid
sequenceDiagram
    autonumber
    actor User as User
    participant PG as Playground View
    participant Store as PlaygroundStore
    participant Provider1 as Gemini Adapter
    participant Provider2 as Groq Adapter (Llama 3)
    participant Provider3 as Ollama Adapter (Local)
    participant Repo as ExperimentRepository

    User->>PG: Load Prompt Version A into Pane 1 & Version B into Pane 2
    User->>PG: Configure Models (Gemini 1.5 Pro vs Llama 3.3 vs Ollama Qwen)
    User->>PG: Adjust Parameters (Temperature: 0.2, Max Tokens: 2048)
    User->>PG: Click "Execute Parallel Run"
    
    par Stream Model 1
        Store->>Provider1: Dispatch Prompt A
        Provider1-->>PG: Stream tokens into Pane 1
    and Stream Model 2
        Store->>Provider2: Dispatch Prompt B
        Provider2-->>PG: Stream tokens into Pane 2
    and Stream Model 3
        Store->>Provider3: Dispatch Prompt B
        Provider3-->>PG: Stream tokens into Pane 3
    end

    PG->>PG: Record Latency to First Token (TTFT) & Total Execution Time
    PG->>PG: Calculate Total Token Usage & Output Speed (tokens/sec)
    User->>PG: Assign subjective quality score & enter evaluation notes
    User->>PG: Click "Save Experiment Run"
    PG->>Repo: Persist run metrics and outputs to SQLite
```

---

## 6. Library Management & Dynamic Variable Injection Flow

```mermaid
flowchart TD
    A["User Opens SUTRA Library"] --> B["Type Search Query into FTS5 Search Bar"]
    B --> C["Instant Sub-Millisecond SQLite FTS5 Query"]
    C --> D["Display Filtered Prompts Grouped by Collections / Tags"]
    
    D --> E["Select Prompt Record"]
    E --> F{"Contains Dynamic Mustache Variables (e.g., {{input}})? "}
    
    F -- Yes --> G["Render Dynamic Variable Input Form"]
    G --> H["User Inputs Variable Values at Runtime"]
    H --> I["Resolve Compiled Prompt with Substituted Values"]
    I --> J["Copy to Clipboard or Open in Playground"]
    
    F -- No --> K["Display Static Prompt Text & Version History Drawer"]
    K --> L{"User Selects Historical Version"}
    L -- Yes --> M["View Point-in-Time Diff & Click 'Revert to Version'"]
    M --> N["Create Incremental Rollback Snapshot"]
```

---

## 7. Context Studio Knowledge Attachment Flow

```mermaid
flowchart TD
    A["User Opens SUTRA Context Studio"] --> B["Create Context Profile (e.g., 'TypeScript Clean Code Rules')"]
    B --> C["Add Custom Instruction Blocks or Ingest Local Reference Files (<10MB)"]
    C --> D["Save Profile to Local SQLite"]
    
    D --> E["User Switches to Sutra Architect or Optimizer"]
    E --> F["Open 'Attach Context' Drawer"]
    F --> G["Select Context Profile(s) to Include"]
    G --> H["Context Studio Computes Real-Time Token Impact Preview"]
    H --> I{"User Approves Token Budget?"}
    
    I -- Yes --> J["Explicitly Delimit & Prepend Context to Target Prompt"]
    I -- No --> K["Deselect Profiles or Trim Context Blocks"]
    K --> H
```

---

## 8. Provider Configuration & Hardware Vault Security Flow

```mermaid
sequenceDiagram
    autonumber
    actor User as User
    participant Settings as Settings View
    participant IPC as Tauri IPC Command Bridge
    participant Rust as Rust Native Vault
    participant OS as OS Credential Manager (WinCred/Keychain/SecretService)

    User->>Settings: Open Provider Management Tab
    User->>Settings: Select Provider (e.g. OpenAI) & Paste API Key (`sk-...`)
    Settings->>IPC: invoke('save_provider_credential', { providerId, apiKey })
    IPC->>Rust: Pass validated payload to Rust backend
    Rust->>OS: Store credential securely under target 'sutra.ai.openai'
    OS-->>Rust: Confirmation Success
    Rust-->>IPC: OK
    IPC-->>Settings: Return success status (Clear plaintext key from React state)
    Settings-->>User: Display green "Key Encrypted & Stored in OS Vault" badge
```
