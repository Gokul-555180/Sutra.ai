# SUTRA.ai — Technology Stack Specification & Architectural Rationale

> **Document Version:** 1.0.0  
> **Status:** Intended Architecture (Pre-Implementation Baseline)  
> **Related Documents:** [ARCHITECTURE.md](../../ARCHITECTURE.md), [TECH_STACK.md](TECH_STACK.md), [ADR-0001](../decisions/ADR-0001-TECHNOLOGY-SELECTION.md)

This document provides a comprehensive evaluation of the intended technology stack for SUTRA.ai. For each component, it details the engineering rationale, core responsibilities, evaluated alternatives, architectural trade-offs, and team learning curves.

---

## 1. Stack Overview Matrix

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          SUTRA.AI TECHNOLOGY STACK                          │
├─────────────────────┬─────────────────────┬─────────────────────────────────┤
│ Desktop Foundation  │ Frontend Layer      │ Design & Micro-Interactions     │
│ • Tauri 2           │ • React 18+         │ • Tailwind CSS                  │
│ • Rust 2021 Edition │ • TypeScript 5.0+   │ • shadcn/ui (Radix Primitives)  │
│                     │ • Vite Build Tool   │ • Framer Motion & Lucide Icons  │
├─────────────────────┼─────────────────────┼─────────────────────────────────┤
│ State & Persistence │ Intelligence Layer  │ Verification & Testing          │
│ • Zustand Stores    │ • Unified Abstraction│ • Vitest (Fast Unit Tests)      │
│ • SQLite (via SQL)  │ • Gemini, Groq,     │ • React Testing Library         │
│ • Zod Schemas       │   OpenAI, Anthropic │ • Playwright (Cross-Webview E2E)│
│ • CodeMirror 6      │ • Ollama (Local LLM)│ • Cargo Test (Rust Engine)      │
└─────────────────────┴─────────────────────┴─────────────────────────────────┘
```

> [!IMPORTANT]
> **Implementation Status:** The technologies cataloged herein represent our approved architecture for development. None of these dependencies are assumed to be pre-installed until initialized via planned roadmap milestones.

---

## 2. Desktop Framework & Native Runtime

### Tauri 2 (Rust)
* **Purpose:** Cross-platform desktop application container providing secure native OS integration, window management, and hardware communication.
* **Why Selected:** Memory efficiency (< 60MB RAM vs 300MB+ in Electron), sub-15MB installer sizes, hardened IPC security model, and memory-safe Rust native code.
* **Responsibilities:** Native OS window chrome, secure local file system access, hardware credential vault bridging, native SQLite database management.
* **Alternatives Evaluated:**
  * *Electron:* Familiar web ecosystem, but produces excessive bundle sizes (>120MB) and high memory consumption, conflicting with our lightweight desktop goal.
  * *Flutter / Qt:* High performance, but lacks the rich web ecosystem for specialized code editors (CodeMirror, Monaco) and markdown tooling.
* **Trade-offs:** Relies on operating system webview runtimes (WebView2 on Windows, WebKit on macOS, WebKitGTK on Linux), which requires thorough cross-platform styling validation.
* **Learning Requirements:** Tauri 2 IPC protocol, capabilities permission syntax, cross-compilation toolchains.

### Rust (2021 Edition)
* **Purpose:** Systems programming language for high-performance native desktop logic.
* **Why Selected:** Memory safety without garbage collection pauses, blazing performance for token calculation, native encryption, and concurrent database operations.
* **Responsibilities:** SQLite transaction bridge, OS Keyring API bindings, cryptographic utilities, local file ingestion sandboxing.
* **Trade-offs:** Steep learning curve, strict borrow checker constraints, and longer compilation times compared to Go or Node.js.

---

## 3. Frontend Architecture & User Interface

### React 18+ & TypeScript 5.0+
* **Purpose:** Declarative component model and compile-time type safety for complex, dynamic desktop interfaces.
* **Why Selected:** Mature component ecosystem, robust support for concurrent rendering, excellent headless UI integration, and widespread industry adoption.
* **Responsibilities:** View layout composition, reactive state rendering, editor integration, interactive prompt diff visualization.
* **TypeScript Invariants:** Configured in `strict: true` mode. Zero tolerance for un-typed `any` or loose type assertions. All domain entities and IPC boundaries are inferred from Zod schemas.
* **Trade-offs:** Virtual DOM overhead during ultra-fast streaming (mitigated by custom token throttling hooks).

### Vite
* **Purpose:** Modern frontend build tool and local development server.
* **Why Selected:** Instant Hot Module Replacement (HMR) powered by ES modules, lightning-fast development startup, and optimized Rollup production bundling.
* **Responsibilities:** Asset compilation, TypeScript transpilation, CSS minification, and tree-shaking dead code elimination.

### Tailwind CSS & shadcn/ui
* **Purpose:** Utility-first styling framework paired with accessible, unstyled headless UI components (Radix UI).
* **Why Selected:** Zero runtime CSS overhead, full control over component source code (components live inside `src/components/ui`), consistent dark-first design tokens, and built-in WAI-ARIA accessibility.
* **Responsibilities:** Desktop layout grid, deep charcoal substrate styling (`#0B0D11`, `#12151B`), low-contrast border styling, and accessible focus rings.
* **Alternatives Evaluated:**
  * *Chakra UI / Material UI:* Heavy runtime styling overhead, opinionated default aesthetics that look like generic web SaaS dashboards.

### Framer Motion & Lucide React
* **Purpose:** Physics-based micro-interactions and lightweight iconography.
* **Why Selected:** Framer Motion enables smooth, declarative panel transitions without blocking user inputs. Lucide React provides a crisp, 16px/18px stroke icon system with a small bundle footprint.
* **Responsibilities:** Collapsible navigation panels, drawer animations, modal transitions, and iconography.

---

## 4. State Management, Persistence & Validation

### Zustand
* **Purpose:** Lightweight, unopinionated client-side state management.
* **Why Selected:** Bare minimum boilerplate (< 1KB bundle size), outside-React selector access, transient state updates without unnecessary component re-renders, and trivial persistence middleware.
* **Responsibilities:** Managing active view tabs, unsaved prompt draft buffers, Playground comparative configurations, and UI notification queues.
* **Alternatives Evaluated:**
  * *Redux Toolkit:* Excessive boilerplate and complexity for a single-user desktop application.
  * *React Context:* Causes widespread component re-rendering during high-frequency token streaming updates.

### SQLite (Embedded via Tauri SQL)
* **Purpose:** Embedded, ACID-compliant local relational database.
* **Why Selected:** Serverless, zero-configuration local database, sub-millisecond query execution, native Full-Text Search (FTS5), and universal backup portability.
* **Responsibilities:** Persisting prompts, version snapshots, collections, tags, context profiles, and experiment run telemetry.

### Zod
* **Purpose:** TypeScript-first schema declaration and runtime data validation.
* **Why Selected:** Single source of truth for both runtime validation and TypeScript compile-time types (`z.infer<typeof Schema>`).
* **Responsibilities:** Validating external AI provider API payloads, parsing user-imported JSON prompt bundles, and sanitizing Tauri IPC command parameters.

---

## 5. Specialized Prompt Editors

### CodeMirror 6 & Tiptap
* **Purpose:** Dual-engine editing tailored for technical prompt engineers and prose authors.
* **Why Selected:**
  * **CodeMirror 6:** Modular, extensible architecture for code-like editing. Provides custom syntax highlighting for prompt delimiters (`###`, `"""`, `<tags>`), token count gutters, dynamic variable pills (`{{var}}`), and inline diff rendering.
  * **Tiptap:** Headless rich-text editor based on ProseMirror, ideal for non-technical users authoring multi-paragraph role instructions.
* **Trade-offs:** CodeMirror 6 requires custom extension development for proprietary prompt delimiter syntaxes.

---

## 6. AI Providers & Intelligence Engine

| Provider | Integration Type | Architectural Role & Capabilities |
| :--- | :--- | :--- |
| **Google Gemini** | Cloud Streaming API | Gemini 1.5 Pro / Flash & 2.0; massive context window handling, multimodal instruction parsing. |
| **Groq** | Cloud Low-Latency API | Llama 3.3 70B, Mixtral; ultra-fast inference (500+ tokens/sec) for real-time prompt optimization and instant rubric analysis. |
| **OpenAI** | Cloud Standard API | GPT-4o, GPT-4o-mini; structured JSON schema enforcement and industry-standard benchmark comparison. |
| **Anthropic** | Cloud Standard API | Claude 3.5 Sonnet / Haiku; exceptional reasoning, nuanced constraint following, and XML delimiter support. |
| **Ollama** | Localhost HTTP (`:11434`) | 100% offline, zero-cost, private model execution (Llama 3, DeepSeek-R1, Mistral, Qwen). |

---

## 7. Automated Testing & Verification

* **Vitest:** Lightning-fast unit test runner with native Vite module resolution. Executes all domain engine, rubric calculation, and variable parser tests in < 5 seconds.
* **React Testing Library:** User-centric component testing verifying accessibility, keyboard navigation, and UI state presentation.
* **Playwright:** End-to-end automation testing desktop webview user journeys, window management, and cross-platform interactions.
* **Cargo Test:** Native Rust test suite verifying SQLite migration integrity, encryption wrappers, and IPC security boundary handlers.
