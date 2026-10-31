# SUTRA.ai — 16-Week Engineering Roadmap

> **Document Version:** 1.0.0  
> **Status:** Active Execution Plan (Pre-Implementation Baseline)  
> **Timeline:** 16 Weeks (4 Months)  
> **Target Release:** SUTRA.ai v1.0.0-GA Desktop Release  
> **Related Documents:** [PRODUCT_REQUIREMENTS.md](PRODUCT_REQUIREMENTS.md), [ARCHITECTURE.md](../../ARCHITECTURE.md), [TECH_STACK.md](../technical/TECH_STACK.md)

---

## 1. Roadmap Architecture & Overview

This roadmap defines the disciplined, week-by-week implementation plan for SUTRA.ai. In accordance with our engineering invariants ([AGENTS.md](../../AGENTS.md)), development executes in small, verifiable, and test-backed milestones.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       16-WEEK DEVELOPMENT TIMELINE                          │
├────────────────────┬────────────────────┬───────────────────────────────────┤
│ Weeks 1–3          │ Weeks 4–6          │ Weeks 7–9                         │
│ Phase 1: Foundation│ Phase 2: Architect │ Phase 3: AI Intelligence          │
│ App shell, Tauri 2,│ Facet compiler,    │ Provider abstraction, Gemini,     │
│ Rust IPC, Tailwind │ CodeMirror editor  │ Groq, Ollama, Optimizer modes     │
├────────────────────┼────────────────────┼───────────────────────────────────┤
│ Weeks 10–12        │ Weeks 13–14        │ Weeks 15–16                       │
│ Phase 4: Storage   │ Phase 5: Sandbox   │ Phase 6: Hardening & Release      │
│ SQLite, FTS5,      │ Playground, multi- │ Security audits, installers,      │
│ Context Studio     │ model benchmarks   │ code signing, v1.0.0 distribution │
└────────────────────┴────────────────────┴───────────────────────────────────┘
```

> [!IMPORTANT]
> **Implementation Status Notice:** As of foundational initialization, all roadmap milestones represent forward-looking planned engineering tasks. No item is marked as completed until proven via automated tests and working application code.

---

## 2. Phase 1: Desktop Foundation & Infrastructure (Weeks 1–3)

### Week 1: Tauri 2 Scaffolding & Native Rust IPC
* **Objectives:** Initialize the Tauri 2 desktop project, configure strict Rust toolchain, and implement foundational IPC command handlers.
* **Deliverables:**
  * Clean `src-tauri/` project configured with `tauri.conf.json`.
  * Safe window frame controls (minimize, maximize, close) with frameless custom titlebar.
  * Native logging bridge and panic hook handlers in Rust.
* **Dependencies:** Rust 1.77+, Node.js v20+, Cargo.
* **Learning Objectives:** Tauri 2 IPC protocol changes, Rust `serde` serialization patterns, memory-safe OS bindings.
* **Acceptance Criteria:** Desktop app launches natively on Windows, macOS, and Linux with < 60MB RAM footprint.

### Week 2: React Frontend Setup & Design System Tokens
* **Objectives:** Establish the React 18 + Vite frontend with TypeScript strict mode and Tailwind CSS / shadcn/ui design tokens.
* **Deliverables:**
  * Strict `tsconfig.json` with zero implicit `any` permitted.
  * Design tokens configured: deep charcoal substrates (`#0B0D11`, `#12151B`), low-contrast borders (`#222733`), and restrained violet accents (`#7C3AED`).
  * Primitive UI components: Button, Input, Dialog, Tooltip, Dropdown, Skeleton.
* **Dependencies:** Week 1 baseline.
* **Learning Objectives:** Radix UI accessibility primitives, Tailwind CSS token colocation, Framer Motion layout transitions.
* **Acceptance Criteria:** Components pass automated accessibility audit (WAI-ARIA) with 100% keyboard navigability.

### Week 3: Application Shell & Dashboard Layout
* **Objectives:** Assemble the core application chrome, navigation sidebar, and initial Home / Dashboard view.
* **Deliverables:**
  * `AppShell` with collapsible sidebar, navigation rail, and status bar.
  * `DashboardView` with empty-state onboarding, quick-action cards, and system status pills.
  * Zustand `useAppStore` for active view routing and window focus states.
* **Dependencies:** Week 2 components.
* **Acceptance Criteria:** Switching between placeholder views executes in < 16ms with zero layout shift.

---

## 3. Phase 2: Prompt Engineering Core (Weeks 4–6)

### Week 4: Structured Facet Model & Deterministic Parser
* **Objectives:** Implement the domain prompt engine that parses raw thoughts into modular components (Role, Objective, Instructions, Constraints, Format).
* **Deliverables:**
  * `PromptArchitectEngine` with Zod schemas for all 9 structured facets.
  * Deterministic keyword extraction and delimiter formatting rules.
  * Pure TypeScript unit test suite (Vitest) verifying facet parsing.
* **Dependencies:** Week 3 application shell.
* **Learning Objectives:** Formal language tokenization, regular expression optimization, abstract syntax tree (AST) modeling.
* **Acceptance Criteria:** 100% unit test coverage for facet extraction with zero network calls.

### Week 5: CodeMirror 6 Prompt Editor & Dynamic Syntax Highlighting
* **Objectives:** Integrate CodeMirror 6 customized for prompt engineering workflows.
* **Deliverables:**
  * Custom CodeMirror extensions for prompt delimiters (`###`, `"""`, `<tags>`).
  * Dynamic token highlighting for Mustache variables (`{{variable_name}}`).
  * Gutter line numbers, token counter, and character length status indicator.
* **Dependencies:** Week 4 domain models.
* **Learning Objectives:** CodeMirror 6 ViewPlugin, Decoration sets, StateField architecture.
* **Acceptance Criteria:** Editor renders seamlessly with sub-millisecond typing latency.

### Week 6: Interactive Clarification Question Engine
* **Objectives:** Build the clarification dialog system that detects missing parameters and queries the user.
* **Deliverables:**
  * Missing parameter detection heuristics (e.g., unspecified coding language or output format).
  * Interactive modal displaying targeted multiple-choice clarification options.
  * Facet synthesis loop merging user responses into the final prompt.
* **Dependencies:** Week 5 editor integration.
* **Acceptance Criteria:** Automatically prompts user when critical constraints are missing.

---

## 4. Phase 3: AI Intelligence & Optimization (Weeks 7–9)

### Week 7: Provider Abstraction Layer & In-Memory Mocking
* **Objectives:** Construct the vendor-agnostic `IAIProvider` interface and in-memory `MockProvider` for automated testing.
* **Deliverables:**
  * Core interfaces: `IAIProvider`, `NormalizedRequest`, `NormalizedResponse`, `StreamChunk`.
  * `ProviderRegistry` for dynamic provider resolution.
  * In-memory `MockProvider` with configurable streaming latency and error injection.
* **Dependencies:** Phase 2 completion.
* **Learning Objectives:** Abstract factory pattern, async iterable streams, backoff strategies.
* **Acceptance Criteria:** Full unit test suite passes using `MockProvider` with zero API costs.

### Week 8: Gemini, Groq & Local Ollama Adapters
* **Objectives:** Implement live adapters for Google Gemini, Groq, and local Ollama instances.
* **Deliverables:**
  * `GeminiAdapter` supporting Gemini 1.5/2.0 streaming.
  * `GroqAdapter` supporting Llama 3 / Mixtral ultra-low latency streaming.
  * `OllamaAdapter` communicating over `http://localhost:11434` for 100% offline local inference.
  * Stream throttle hook preventing DOM thrashing (16ms batching window).
* **Dependencies:** Week 7 provider abstraction.
* **Acceptance Criteria:** Real-time token streaming works reliably across all three adapters.

### Week 9: SUTRA Optimizer (10 Modes & Explainability Diff Engine)
* **Objectives:** Build the 10-mode optimization engine with side-by-side visual diffs and rationale logs.
* **Deliverables:**
  * 10 optimization modes implemented (Enhance, Precision, Expand, Simplify, Technical, Creative, Research, Academic, Image, Model Adaptation).
  * Side-by-side diff component highlighting character-level additions and deletions.
  * Structured explainability changelog explaining *why* modifications occurred.
* **Dependencies:** Week 8 adapters.
* **Acceptance Criteria:** Every optimization produces both a visual diff and an explainable rationale entry.

---

## 5. Phase 4: Local Storage & Personalization (Weeks 10–12)

### Week 10: SQLite Integration, Migrations & OS Keyring Vault
* **Objectives:** Implement local database persistence and OS-native hardware credential storage.
* **Deliverables:**
  * Rust native SQLite bridge with migration runner (`001_initial_schema.sql`).
  * OS Keyring integration (Windows Credential Manager, macOS Keychain, Linux Secret Service).
  * `PromptRepository` and `CollectionRepository` implementing transactional writes.
* **Dependencies:** Phase 3 completion.
* **Learning Objectives:** SQLite ACID transactions, SQLite FTS5 configuration, native OS keyring APIs.
* **Acceptance Criteria:** API keys are stored encrypted in OS vaults; database operations survive simulated crash recovery.

### Week 11: SUTRA Library & Full-Text Search (FTS5)
* **Objectives:** Build the local prompt asset manager with infinite folders, tags, and sub-millisecond search.
* **Deliverables:**
  * Full-Text Search (FTS5) virtual tables and automatic update triggers.
  * Infinite nested folder tree and color-coded tag taxonomy.
  * Immutable version history drawer with point-in-time diffing and rollback.
* **Dependencies:** Week 10 database foundation.
* **Acceptance Criteria:** FTS5 search queries over 10,000 mock prompts return in < 15ms.

### Week 12: SUTRA Context Studio & Knowledge Profiles
* **Objectives:** Implement reusable context profiles, instruction blocks, and safe file ingestion.
* **Deliverables:**
  * Context profile management (Project rules, coding conventions, brand guidelines).
  * Local file ingestion for `.txt`, `.md`, and `.pdf` files (capped at 10 MB).
  * Real-time token budget calculator showing context overhead before execution.
* **Dependencies:** Week 11 library.
* **Acceptance Criteria:** Context is never injected silently; requires explicit user toggle.

---

## 6. Phase 5: Advanced Experimentation & Analysis (Weeks 13–14)

### Week 13: SUTRA Analyzer & 9-Dimensional Rubric Engine
* **Objectives:** Implement the static and semantic prompt quality diagnostic engine.
* **Deliverables:**
  * 9-dimensional scoring algorithm (Clarity, Specificity, Structure, Constraints, etc.).
  * Heuristic contradiction detector (e.g., brief vs detailed conflicts).
  * Diagnostic report card with visual score breakdown and one-click auto-fix actions.
* **Dependencies:** Phase 4 completion.
* **Acceptance Criteria:** Clear disclaimer displayed that scores represent heuristic guidance.

### Week 14: SUTRA Playground (Multi-Model Comparative Benchmarking)
* **Objectives:** Build the parallel multi-pane comparative execution environment.
* **Deliverables:**
  * Dual- and quad-pane comparative execution sandbox.
  * Telemetry tracking: Latency to First Token (TTFT), total tokens, tokens/sec.
  * Tabular test case runner with variable replacement.
  * Experiment result export (CSV and Markdown).
* **Dependencies:** Week 13 analyzer and Phase 3 adapters.
* **Acceptance Criteria:** Streams up to 4 models in parallel without UI stutter.

---

## 7. Phase 6: Hardening, Packaging & Release (Weeks 15–16)

### Week 15: Security Audits, E2E Testing & Performance Tuning
* **Objectives:** Perform end-to-end user journey tests, dependency audits, and bundle optimization.
* **Deliverables:**
  * Playwright E2E automated test suite covering all primary user flows.
  * `cargo audit` and `pnpm audit` passing with 0 high/critical vulnerabilities.
  * SQLite database compaction and query optimization.
* **Dependencies:** All previous phases.
* **Acceptance Criteria:** 100% of integration test suites pass in continuous integration.

### Week 16: Packaging, Cross-Platform Bundling & v1.0.0 Release
* **Objectives:** Build and validate production desktop installers for Windows, macOS, and Linux.
* **Deliverables:**
  * Windows installer: `.msi` and `.exe` via Tauri Bundler.
  * macOS bundle: `.dmg` (Universal binary for Intel & Apple Silicon).
  * Linux package: `.AppImage` and `.deb`.
  * Verified Release Notes, user documentation, and v1.0.0 GitHub Release.
* **Dependencies:** Week 15 verification.
* **Acceptance Criteria:** Standalone installers install cleanly on fresh OS environments without developer dependencies.
