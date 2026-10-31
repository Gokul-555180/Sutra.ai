# ADR-0001: Core Desktop & Frontend Technology Stack Selection

> **Status:** Approved (Baseline Architecture)  
> **Date:** October 2026  
> **Decision Makers:** Principal Software Architect, Desktop Engineering Lead  
> **Related Documents:** [ARCHITECTURE.md](../../ARCHITECTURE.md), [TECH_STACK.md](../technical/TECH_STACK.md)

---

## 1. Context & Problem Statement

SUTRA.ai is designed as a free, local-first, privacy-conscious desktop Prompt Engineering Environment. The tool must provide an industrial-grade workstation experience characterized by instant startup, minimal memory consumption, native hardware security, and long-term architectural stability across Windows, macOS, and Linux.

This Architectural Decision Record documents the evaluation and selection of the foundational runtime, languages, state management, and persistence technologies.

---

## 2. Decision Matrix

### Decision 1: Tauri 2 (Rust) over Electron
* **Status:** Approved.
* **Context:** A desktop runtime is required to bridge web frontend technologies with native operating system capabilities.
* **Options Considered:**
  1. *Electron:* Mature, proven, broad web ecosystem, but notorious for high RAM overhead (>300MB idle) and bloated binary sizes (>120MB).
  2. *Tauri 2 (Rust):* Memory-safe compiled native core, lightweight system webview bindings (<60MB idle), sub-15MB installer sizes, and hardened security model.
  3. *Native C++ / Qt:* Blazing performance, but poor velocity for rapid UI iteration, specialized rich text editing, and modern web styling.
* **Decision:** Selected **Tauri 2 with Rust**.
* **Reasoning:** Tauri aligns perfectly with our local-first, lightweight desktop ethos. Its security architecture allows strict capability isolation, blocking unauthorized shell execution and network access.
* **Consequences:** Team must maintain compiled Rust native modules for database and OS keyring access. Webview differences (WebView2 vs WebKit) must be accounted for in CSS.
* **Risks:** Dependency on system-installed webviews on older Linux environments.
* **Revisit Conditions:** If major webview rendering discrepancies significantly impair cross-platform consistency.

---

### Decision 2: React 18+ & TypeScript 5+ over Alternatives
* **Status:** Approved.
* **Context:** A frontend framework is needed for reactive UI components, complex multi-pane layouts, and prompt diffing.
* **Options Considered:**
  1. *React 18+ (TypeScript):* Unmatched component ecosystem, standard industry tooling, excellent headless UI libraries (Radix UI).
  2. *Svelte / Solid:* Excellent reactivity and small runtime, but smaller ecosystem for specialized code editor wrappers and complex diff visualizers.
  3. *Vue 3:* Strong reactivity, but less mature headless accessibility ecosystem compared to Radix / shadcn.
* **Decision:** Selected **React 18+ with TypeScript in strict mode**.
* **Reasoning:** React provides the richest ecosystem for our specialized requirements (CodeMirror 6 integrations, Tiptap headless editors, Radix UI primitives). TypeScript strict mode guarantees compile-time contract enforcement.
* **Consequences:** Developers must prevent unnecessary re-renders during high-speed token streaming by using selector-based state subscriptions and custom throttles.
* **Risks:** Virtual DOM reconciliation stalls if token streams are not batched.
* **Revisit Conditions:** If concurrent token rendering causes unresolvable frame drops under heavy load.

---

### Decision 3: SQLite over Remote-First Databases
* **Status:** Approved.
* **Context:** Prompt templates, version snapshots, and evaluation benchmarks must be stored locally with high reliability.
* **Options Considered:**
  1. *SQLite (via Tauri SQL Bridge):* Zero configuration, ACID transactions, embedded file, native FTS5 full-text indexing, universal portability.
  2. *IndexedDB (Browser Webview):* Sandboxed within webview, prone to unexpected browser cache eviction, poor relational query support.
  3. *Cloud Database (PostgreSQL / Supabase):* Violates our local-first, zero-mandatory-subscription principle.
* **Decision:** Selected **embedded SQLite**.
* **Reasoning:** SQLite provides rock-solid persistence, instant local search indexing via FTS5, atomic multi-table transactions, and user data ownership.
* **Consequences:** Data access must follow the repository pattern, communicating asynchronously across Tauri IPC.
* **Risks:** Database migrations must be tested rigorously to prevent corrupting existing user data.
* **Revisit Conditions:** None; SQLite is the universal standard for local desktop persistence.

---

### Decision 4: Zustand over Redux Toolkit or React Context
* **Status:** Approved.
* **Context:** Global state management is needed for active views, unsaved draft buffers, and playground configurations.
* **Options Considered:**
  1. *Zustand:* Minimal boilerplate (<1KB), outside-React selector access, fine-grained subscriber notifications.
  2. *Redux Toolkit:* Excessive boilerplate and complexity for a single-user desktop workstation.
  3. *React Context:* Causes widespread component re-renders during rapid streaming state updates.
* **Decision:** Selected **Zustand**.
* **Reasoning:** Zustand delivers optimal performance during high-frequency token updates by allowing components to subscribe to isolated slice selectors.

---

### Decision 5: Unified Provider Abstraction over Direct Vendor Coupling
* **Status:** Approved.
* **Context:** SUTRA.ai connects to multiple foundation models (Gemini, Groq, OpenAI, Anthropic, Ollama).
* **Options Considered:**
  1. *Direct Vendor SDKs:* Importing `@google/genai`, `openai`, etc., directly across UI components.
  2. *Unified `IAIProvider` Abstraction:* Standardized interface and normalized request/response contracts mediating all AI calls.
* **Decision:** Selected **Unified Provider Abstraction Layer**.
* **Reasoning:** Decouples UI components from vendor quirks, eliminates vendor lock-in, enables seamless local Ollama model execution, and allows 100% offline testing via `MockProvider`.
