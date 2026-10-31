# SUTRA.ai — Feature Specifications Document

> **Document Version:** 1.0.0  
> **Status:** Foundational Specification  
> **Related Documents:** [PRODUCT_REQUIREMENTS.md](PRODUCT_REQUIREMENTS.md), [ARCHITECTURE.md](../../ARCHITECTURE.md), [USER_FLOWS.md](USER_FLOWS.md)

This document provides exhaustive, engineering-grade feature specifications for all 10 core modules in SUTRA.ai. Each specification defines inputs, algorithmic processing, user interactions, outputs, error conditions, and acceptance criteria.

---

## Module Catalog Summary

| Module ID | Module Name | Scope & Core Responsibility | Priority |
| :--- | :--- | :--- | :---: |
| **MOD-01** | **Home / Dashboard** | Workspace health, telemetry, recent prompts, quick launches | **MVP (P0)** |
| **MOD-02** | **SUTRA Architect** | Structured natural language prompt compilation engine | **MVP (P0)** |
| **MOD-03** | **SUTRA Optimizer** | Multi-mode explainable prompt enhancement & diff engine | **MVP (P0)** |
| **MOD-04** | **SUTRA Analyzer** | 9-dimensional rubric scoring, contradiction scanner | **MVP (P0)** |
| **MOD-05** | **SUTRA Playground** | Multi-model comparative testbed with telemetry | **Phase 7 (P1)** |
| **MOD-06** | **SUTRA Library** | FTS5 full-text indexed local prompt repository | **MVP (P0)** |
| **MOD-07** | **SUTRA Context Studio** | Reusable project knowledge & instruction profiles | **Phase 6 (P1)** |
| **MOD-08** | **SUTRA Workflow Studio**| Node-based visual prompt chaining canvas | **Phase 8 (P2)** |
| **MOD-09** | **SUTRA Academy** | Interactive prompt engineering labs & curriculum | **Phase 8 (P2)** |
| **MOD-10** | **Settings & Providers** | OS Keyring management, Ollama discovery, backup tools | **MVP (P0)** |

---

## Detailed Specifications

### MOD-01: Home / Dashboard

#### Feature Spec: `FEAT-0101` — Workspace Overview & Health Center
* **Description:** Unified entry dashboard presenting recent prompt activity, system storage metrics, and AI provider connectivity.
* **Purpose:** Provide immediate workspace situational awareness upon app launch.
* **Target Users:** All user personas.
* **User Interaction:**
  1. User launches application; Dashboard displays by default.
  2. Pinned prompts and recent prompts appear in clean horizontal cards.
  3. AI Provider status pills display connection state (Connected, Degraded, Disconnected).
  4. Quick Action buttons ("Create Prompt", "Optimize Prompt", "Analyze Prompt") instantly route user to respective feature views.
* **Inputs:** Local SQLite database queries for recent records; asynchronous ping to configured AI endpoints.
* **Processing:**
  * Query last 10 edited prompts from `prompts` table joined with `prompt_versions`.
  * Measure round-trip ping time to enabled provider endpoints (or Ollama localhost port).
  * Compute local database file size and total row counts.
* **Outputs:** Reactive UI cards, latency graphs, and database status indicators.
* **Edge Cases & Error States:**
  * *Database empty:* Display helpful onboarding empty-state card with quick-start templates.
  * *No internet / AI endpoint offline:* Provider pill turns amber/red with specific error tooltip ("Connection refused: localhost:11434" or "Invalid API Key").
* **Dependencies:** `PromptRepository`, `ProviderRegistry`, `SettingsStore`.
* **Acceptance Criteria:**
  * Renders in < 50ms from local SQLite cache.
  * Correctly identifies offline providers without stalling the UI thread.
* **Release Priority:** MVP (P0).

---

### MOD-02: SUTRA Architect

#### Feature Spec: `FEAT-0201` — Natural Language to Structured Prompt Synthesizer
* **Description:** Parses conversational, ambiguous user thoughts and compiles them into a modular, multi-facet prompt model.
* **Purpose:** Overcome user under-specification by enforcing structured prompt architecture.
* **Structured Facet Model:**
  1. `Role & Persona`: Authority, tone, and cognitive perspective.
  2. `Primary Objective`: Clear single-sentence goal.
  3. `Context & Assumptions`: Background information and runtime assumptions.
  4. `Procedural Instructions`: Step-by-step reasoning steps.
  5. `Requirements & Invariants`: Necessary constraints, formats, or stylistic rules.
  6. `Negative Constraints`: Explicit prohibitions ("Never use X", "Do NOT hallucinate Y").
  7. `Input Data Schema`: Delimited slots or variables for dynamic inputs.
  8. `Output Specification`: Exact format schema (JSON, Markdown, YAML, Plain Text).
  9. `Evaluation Criteria`: Self-refinement checks for the target LLM.
* **User Interaction:**
  1. User enters raw thought in the input area (e.g., *"Write a python script that parses invoices and outputs json"*).
  2. Selects target domain (Code, Analysis, Writing, Image) and target LLM family.
  3. System parses input, generates structured facets, and flags missing details.
  4. User reviews, toggles, or modifies individual facets in the CodeMirror editor.
* **Interactive Clarification Engine:**
  * If the input lacks critical parameters (e.g., target invoice format or JSON schema), the engine generates 1–3 multiple-choice clarification questions.
  * Clarification answers are merged directly into the facet compilation pipeline.
* **Inputs:** Raw user text string, selected domain, complexity toggle (Simple, Standard, Exhaustive).
* **Processing:** Deterministic regex pattern matching for keywords + semantic LLM extraction call via `IAIProvider`.
* **Outputs:** Structured prompt schema rendered with custom delimiter syntax (`###`, `"""`, `<instructions>`) and dynamic Mustache variable tokens (`{{var}}`).
* **Edge Cases & Error States:**
  * *Extremely short input (e.g., "code")*: System prompts user with a guided prompt wizard rather than making random assumptions.
  * *API failure during generation*: Falls back gracefully to deterministic rule-based template generation.
* **Acceptance Criteria:**
  * Produces valid structured prompt adhering to the target model's syntax conventions.
  * Allows editing of any individual facet without breaking the rest of the prompt.
* **Release Priority:** MVP (P0).

---

### MOD-03: SUTRA Optimizer

#### Feature Spec: `FEAT-0301` — 10-Mode Explainable Prompt Enhancement
* **Description:** Refactors prompts across 10 specialized optimization modes, generating side-by-side diffs and human-readable rationale changelogs.
* **Optimization Modes:**
  1. `Enhance`: General elevation of clarity, structure, and delimiter boundaries.
  2. `Precision`: Strips fluff, narrows scope, and enforces zero-hallucination guardrails.
  3. `Expand`: Unpacks condensed directives, adding edge-case handling and reasoning instructions.
  4. `Simplify`: Compresses prompts to minimize token consumption and reduce latency.
  5. `Technical`: Embeds types, invariants, error handling, and language-specific conventions.
  6. `Creative`: Injects stylistic tone, sensory depth, and narrative constraints.
  7. `Research`: Adds academic methodology, counter-argument mandates, and citation frameworks.
  8. `Academic`: Emphasizes formal tone, theoretical rigor, and peer-review evaluation standards.
  9. `Image Generation`: Translates prose into artistic medium, lighting, camera lens, and negative keywords.
  10. `Model-Specific Adaptation`: Tailors delimiters (e.g., Claude XML tags vs OpenAI Markdown headers vs Gemini turns).
* **User Interaction:**
  1. User pastes or loads an existing prompt.
  2. Selects an optimization mode from the toolbar.
  3. Clicks "Optimize".
  4. View switches to a side-by-side diff view with green (additions) and red (deletions).
  5. An "Explainability Panel" explains each modification with direct references to prompt engineering principles.
* **Inputs:** Raw input prompt, selected optimization mode, target model family.
* **Outputs:** Optimized prompt string, token difference metrics, character diff array, and structured rationale items:
  ```json
  {
    "category": "Constraint Enforcement",
    "change": "Added explicit negative constraint against markdown formatting",
    "rationale": "Prevents downstream JSON parser failure when parsing raw LLM responses"
  }
  ```
* **Acceptance Criteria:**
  * Every optimization MUST include at least one clear explanation entry.
  * Undo and redo stacks preserve full history across multiple iterative optimizations.
* **Release Priority:** MVP (P0).

---

### MOD-04: SUTRA Analyzer

#### Feature Spec: `FEAT-0401` — 9-Dimensional Diagnostic Rubric & Contradiction Scanner
* **Description:** Comprehensive static and semantic analyzer that scores prompts across 9 dimensions, identifying structural flaws, unclosed delimiters, and conflicting instructions.
* **Rubric Dimensions (0–100):**
  1. `Clarity`: Syntactic precision and readability.
  2. `Context Grounding`: Inclusion of necessary domain assumptions and environment framing.
  3. `Specificity`: Presence of concrete goals, boundaries, and measurable outcomes.
  4. `Structural Integrity`: Logical sequencing, clean delimiters, and hierarchical sections.
  5. `Completeness`: Coverage of expected inputs, operations, and edge cases.
  6. `Constraint Enforcement`: Explicit negative boundaries and hallucination guardrails.
  7. `Output Definition`: Exact specification of output format, types, and schemas.
  8. `Internal Consistency`: Absence of contradictory or mutually exclusive instructions.
  9. `Token Readability & Economy`: Efficient token utilization without redundant filler words.
* **Deterministic vs Semantic Analysis:**
  * *Deterministic Scanner:* Instant checks (< 5ms) for missing output specifications, unclosed quote delimiters, unclosed tags, and length-constraint contradictions (e.g., "be brief" + "explain in great detail").
  * *Semantic Evaluator:* LLM-assisted deep scan for contextual ambiguity and subtle domain assumptions.
* **Outputs:** Dimension score radar/bar chart, aggregate quality index, flagged defect badges, and one-click "Auto-Fix" buttons.
* **Disclaimer Requirement:** The UI must display an explicit disclaimer that rubric scores are heuristic evaluations rather than scientifically absolute measurements.
* **Release Priority:** MVP (P0).

---

### MOD-05: SUTRA Playground

#### Feature Spec: `FEAT-0501` — Multi-Model Comparative Sandbox
* **Description:** Parallel execution environment comparing prompt versions across multiple foundational and local LLMs.
* **User Interaction:**
  1. User configures dual or quad execution panes (e.g., Pane 1: Gemini 1.5 Pro, Pane 2: Groq Llama 3.3 70B, Pane 3: Local Ollama Qwen 2.5).
  2. Sets prompt variations (Prompt A vs Prompt B) and adjusts parameters (temperature, max tokens).
  3. Clicks "Execute All".
  4. Responses stream into each pane simultaneously.
  5. Bottom status bar displays comparative metrics: Latency to First Token (TTFT), Total Latency (ms), Generation Tokens/Sec, and Total Token Count.
* **Batch Test Cases:**
  * Users can upload CSV/JSON tabular test cases containing variable values (e.g., `user_question`, `language`).
  * Playground executes prompt across all test cases sequentially, compiling a comparative results table.
* **Acceptance Criteria:**
  * Non-blocking UI during simultaneous multi-stream execution.
  * Export experiment results to CSV or Markdown.
* **Release Priority:** Phase 7 (P1).

---

### MOD-06: SUTRA Library

#### Feature Spec: `FEAT-0601` — FTS5-Indexed Local Prompt Repository
* **Description:** Local-first prompt asset manager with infinite folder hierarchies, tag taxonomies, version history, and full-text search.
* **Key Capabilities:**
  * **FTS5 Search:** Full-text indexing of prompt title, description, body text, and tags. Returns search results in < 10ms for 10,000+ prompts.
  * **Immutable Version History:** Every save action creates an incremental `prompt_version` record with author notes. Users can view diffs between any two historical versions and revert with one click.
  * **Variable Detection:** Prompts containing `{{variable_name}}` are recognized as dynamic templates. Library provides an interactive runtime form to fill variables and copy the resolved prompt.
  * **Import & Export:** Export single prompts or entire collections to JSON bundles or Markdown files with standard YAML frontmatter.
* **Acceptance Criteria:**
  * All data stored in local SQLite database (`sutra.db`).
  * Rollback restores historical prompt text without deleting newer version records.
* **Release Priority:** MVP (P0).

---

### MOD-07: SUTRA Context Studio

#### Feature Spec: `FEAT-0701` — Reusable Context & Knowledge Profiles
* **Description:** Manages domain context snippets, coding conventions, and brand rules that can be explicitly attached to prompts during compilation.
* **Key Principles:**
  * **Zero Silent Injection:** Context profiles are never injected automatically without user selection.
  * **Token Budget Preview:** Real-time token counter displays how much context is being added to the prompt.
  * **Document Ingestion:** Parses local text from `.txt`, `.md`, and `.pdf` files (capped at 10 MB per file) into clean, delimited context blocks.
* **Release Priority:** Phase 6 (P1).

---

### MOD-08: SUTRA Workflow Studio

#### Feature Spec: `FEAT-0801` — Node-Based Visual Prompt Chaining Canvas
* **Description:** Visual workflow canvas connecting prompt nodes in multi-step AI reasoning pipelines.
* **Node Types:**
  * `Input Node`: Captures raw user input or uploaded file.
  * `Prompt Node`: Executes a prompt template via a configured model.
  * `Transformation Node`: Applies regex, JSON parsing, or text slicing.
  * `Conditional Node`: Branches execution based on classification outputs.
* **Release Priority:** Phase 8 (P2).

---

### MOD-09: SUTRA Academy

#### Feature Spec: `FEAT-0901` — Interactive Prompt Engineering Curriculum
* **Description:** Built-in interactive learning environment covering foundational to advanced prompting science.
* **Curriculum Modules:**
  * Zero-shot vs Few-shot calibration.
  * Delimiter engineering (XML, Triple Quotes, Markdown).
  * Role conditioning & System turn precedence.
  * Chain-of-Thought (CoT) and ReAct architectures.
  * Structured output extraction with JSON schemas.
* **Interactive Labs:** Live prompt exercises where users write prompts to solve specific tasks and receive instant rubric feedback.
* **Release Priority:** Phase 8 (P2).

---

### MOD-10: Settings & Provider Management

#### Feature Spec: `FEAT-1001` — Hardware Vault Security & Provider Setup
* **Description:** Centralized control for API credentials, local model discovery, theme customization, and database maintenance.
* **Key Capabilities:**
  * **OS Credential Manager:** Securely commits API keys directly to Windows Credential Manager, macOS Keychain, or Linux Secret Service via Tauri native plugins. Zero plaintext key storage.
  * **Ollama Local Discovery:** Pings `http://localhost:11434/api/tags` to discover running local models (Llama, Mistral, Qwen, DeepSeek) without requiring manual URL setup.
  * **Database Backup & Integrity:** Provides one-click SQLite database backup to a user-chosen directory, database VACUUM compaction, and integrity verification.
* **Release Priority:** MVP (P0).
