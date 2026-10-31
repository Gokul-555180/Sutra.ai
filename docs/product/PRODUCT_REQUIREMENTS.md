# SUTRA.ai — Product Requirements Document (PRD)

> **Document Version:** 1.0.0  
> **Status:** Approved for Architecture & Implementation  
> **Target Audience:** Product Managers, Software Engineers, UI/UX Designers, QA Engineers  
> **Related Documents:** [ARCHITECTURE.md](../../ARCHITECTURE.md), [FEATURE_SPECIFICATIONS.md](FEATURE_SPECIFICATIONS.md), [USER_FLOWS.md](USER_FLOWS.md), [ROADMAP.md](ROADMAP.md)

---

## 1. Executive Summary & Vision

### 1.1 The Meaning & Genesis of SUTRA
The word **Sutra** (*sūtra*, सूत्र) originates from ancient Sanskrit, meaning a "thread", "aphorism", "rule", or "concise guiding principle". In classical literature, a sutra distilled complex philosophical wisdom into minimal, unambiguous, and potent instructional units.

In modern generative artificial intelligence, a prompt serves the identical purpose: it is the structural thread that guides complex, probabilistic foundation models toward reliable, deterministic, and high-performance outcomes.

### 1.2 Product Vision Statement
**SUTRA.ai** is a free, local-first, privacy-conscious desktop Prompt Engineering Environment (PEE) that empowers software engineers, researchers, content creators, and everyday AI users to construct, optimize, evaluate, benchmark, organize, and master artificial intelligence prompts.

SUTRA.ai operates under the core belief that prompt engineering should not be an opaque art of trial-and-error, but a transparent, disciplined, explainable software discipline.

---

## 2. Problem Statement & Market Opportunity

Modern generative AI users face four critical challenges:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       THE CORE PROMPT ENGINEERING CRISIS                    │
├──────────────────────────┬──────────────────────────┬───────────────────────┤
│ 1. Under-Specification   │ 2. Opaque "Magic" Tools  │ 3. Vendor Lock-In &   │
│    Unstructured thoughts │    Generic AI wrappers   │    Privacy Risk       │
│    yield hallucinations  │    provide no rationale  │    Cloud SaaS leaks   │
│    and diluted attention.│    for modifications.    │    proprietary context│
└──────────────────────────┴──────────────────────────┴───────────────────────┘
```

1. **Cognitive Mismatch & Under-Specification:** Humans communicate in loose, contextual fragments. Large Language Models require explicit roles, delimited scopes, negative constraints, and output format schemas. Without structured engineering, prompts produce hallucinations, generic platitudes, or ignored instructions.
2. **Black-Box "Prompt Enhancers":** Existing web-based prompt tools provide opaque rewrites without explaining *what* changed or *why*. Users learn nothing and cannot adapt their prompts systematically.
3. **Privacy & Data Security Vulnerabilities:** Software developers and enterprise professionals frequently paste confidential source code, internal schemas, and brand secrets into third-party cloud prompt tools, introducing severe intellectual property exposure.
4. **Vendor Lock-In & Fragmented Testing:** Comparing how a prompt executes across Claude 3.5, GPT-4o, Gemini 1.5/2.0, Groq Llama, and local Ollama models currently requires manually juggling multiple browser tabs, subscription accounts, and mismatched copy-pasting.

---

## 3. Target Audience & User Personas

| Persona | Primary Role & Context | Core Pain Points | Key Value Realized from SUTRA |
| :--- | :--- | :--- | :--- |
| **Persona A: Alex (Senior Full-Stack Engineer)** | Builds production AI features, writes complex JSON schema extraction prompts and code generation prompts. | Inconsistent model outputs, context dilution, leaking proprietary code to third-party prompt SaaS tools. | Local-first offline execution, CodeMirror dynamic variable interpolation, precision optimization mode, zero telemetry. |
| **Persona B: Dr. Elena (Academic Researcher)** | Synthesizes academic literature, drafts peer-review critiques, and analyzes experimental datasets. | Vague model summaries, missing counter-arguments, lack of methodological rigor. | Research and Academic optimization modes, 9-dimensional rubric clarity scoring, repeatable test fixtures. |
| **Persona C: Marcus (Prompt Engineer & Evaluator)** | Tunes prompts across different foundational LLMs to minimize inference token costs and latency. | Manually benchmarking prompts across multiple web interfaces with zero unified latency tracking. | Multi-pane comparative Playground, side-by-side token/latency analytics, automated prompt diffing. |
| **Persona D: Maya (Product Designer / Everyday User)** | Generates user journey narratives, UI copy variations, and midjourney image prompts. | Uncertainty about how to structure prompts, intimidated by technical jargon. | Natural language to structured prompt conversion (Sutra Architect), guided interactive clarification, Sutra Academy. |

---

## 4. Product Objectives & Success Metrics

### 4.1 Product Objectives
* **Objective 1:** Provide a zero-cost, local-first prompt authoring workstation running completely offline without mandatory accounts or subscriptions.
* **Objective 2:** Deliver an explainable optimization engine that produces transparent before-and-after rationales for every prompt modification.
* **Objective 3:** Enable unified, vendor-neutral execution across cloud APIs (Gemini, Groq, OpenAI, Anthropic) and local offline instances (Ollama).
* **Objective 4:** Provide an objective, 9-dimensional evaluation rubric that diagnoses prompt vulnerabilities before token execution.

### 4.2 Key Performance Indicators (KPIs)
* **Performance:** Sub-100ms application startup time; sub-10ms SQLite FTS5 search query latency across 10,000+ stored prompts.
* **Memory Footprint:** Desktop idle RAM consumption below 70 MB (achieved via Tauri 2 Rust runtime).
* **Reliability:** 100% deterministic local test suite with 0 network dependencies required during continuous integration.
* **Security:** Zero plaintext API key storage; 100% compliance with OS-native credential vault standards.

---

## 5. Functional Requirements (FR)

### Module 1: Home / Dashboard
* **FR-001:** The system shall display a personalized workspace dashboard summarizing recent prompts, pinned favorites, and recent experiments.
* **FR-002:** The system shall provide quick-action entry points to immediately launch prompt creation, prompt optimization, or prompt analysis.
* **FR-003:** The dashboard shall display live health and latency metrics for all configured AI providers.
* **FR-004:** The dashboard shall display local storage statistics including total prompt count, collection count, and database file size.

### Module 2: SUTRA Architect (Structured Prompt Authoring)
* **FR-005:** The system shall parse unstructured natural language user input into structured prompt components: *Role, Primary Objective, Context, Instructions, Negative Constraints, Input Data Schema, Output Specification, and Self-Refinement Criteria*.
* **FR-006:** The system shall adapt prompt structure dynamically based on task intent (e.g., omitting coding constraints for creative writing tasks).
* **FR-007:** The system shall detect ambiguities or missing parameters and prompt the user with targeted interactive clarification questions before generating the final prompt.
* **FR-008:** The system shall allow users to toggle prompt complexity levels (Simple, Standard, Exhaustive).
* **FR-009:** The system shall generate multiple distinct prompt variations based on different structural frameworks.
* **FR-010:** The system shall provide a dual-mode editor: CodeMirror 6 with custom syntax highlighting for prompt delimiters and Mustache variables (`{{var}}`), and Tiptap for rich prose authoring.

### Module 3: SUTRA Optimizer (Explainable Prompt Enhancement)
* **FR-011:** The system shall support 10 specialized optimization modes: *Enhance, Precision, Expand, Simplify, Technical, Creative, Research, Academic, Image Generation, and Model-Specific Adaptation*.
* **FR-012:** The system shall provide a side-by-side visual diff (inline additions and deletions) comparing the original and optimized prompts.
* **FR-013:** The system shall generate an explicit, human-readable rationale log explaining *why* specific keywords, delimiters, or constraints were introduced or removed.
* **FR-014:** The system shall support full non-destructive undo and redo capabilities for all optimization cycles.
* **FR-015:** The system shall allow direct saving of any optimized variant as an incremental version snapshot.

### Module 4: SUTRA Analyzer (Quality Diagnostic Rubric)
* **FR-016:** The system shall evaluate prompts against a 9-dimensional analytical rubric: *Clarity, Context Grounding, Specificity, Structural Integrity, Completeness, Constraint Enforcement, Output Definition, Internal Consistency, and Token Readability*.
* **FR-017:** The system shall compute individual dimension scores (0–100) and an aggregate weighted quality index accompanied by clear disclaimers that scores are heuristic guides rather than scientific absolutes.
* **FR-018:** The system shall identify contradictory instructions (e.g., demanding extreme brevity while requesting exhaustive detail).
* **FR-019:** The system shall generate one-click actionable remediation recommendations for each flagged defect.

### Module 5: SUTRA Playground (Multi-Model Experimentation)
* **FR-020:** The system shall allow side-by-side prompt execution across multiple AI models simultaneously (e.g., Claude 3.5 Sonnet vs GPT-4o vs Groq Llama-3 vs Local Ollama).
* **FR-021:** The system shall expose model hyperparameters where supported: temperature, top_p, top_k, max_output_tokens, and stop sequences.
* **FR-022:** The system shall record execution telemetry for every run: latency (ms), prompt tokens, completion tokens, and estimated cost.
* **FR-023:** The system shall support tabular test cases with variable substitution across batch prompt runs.
* **FR-024:** The system shall allow saving entire experiment configurations and results for regression benchmarking.

### Module 6: SUTRA Library (Local-First Management)
* **FR-025:** The system shall store unlimited prompt records locally in SQLite, constrained only by available physical disk space.
* **FR-026:** The system shall support infinite nested hierarchical folders, custom collections, and color-coded tags.
* **FR-027:** The system shall provide sub-millisecond full-text search indexing (SQLite FTS5) across titles, descriptions, prompt bodies, tags, and variables.
* **FR-028:** The system shall automatically maintain an immutable version history for every prompt edit, allowing point-in-time diffing and rollback.
* **FR-029:** The system shall support seamless JSON and Markdown (with YAML frontmatter) import and export.

### Module 7: SUTRA Context Studio (Knowledge Management)
* **FR-030:** The system shall allow creation of reusable context profiles containing project architecture rules, personal preferences, coding conventions, or brand guidelines.
* **FR-031:** The system shall support safe, explicit context profile attachment to prompts with real-time token count previews.
* **FR-032:** The system shall never silently inject unselected context profiles into outbound AI requests.
* **FR-033:** The system shall extract text content from imported `.txt`, `.md`, and `.pdf` reference files (up to 10 MB per file).

### Module 8: SUTRA Workflow Studio (Prompt Chaining)
* **FR-034:** The system shall provide a visual node-based canvas for chaining multi-step prompts (Research → Outline → Draft → Critique → Finalize).
* **FR-035:** The system shall support sequential variable mapping where the output of Node N serves as the input variable for Node N+1.
* **FR-036:** The system shall record execution histories and step-by-step intermediate outputs across workflow runs. *(Deferred to Phase 8)*.

### Module 9: SUTRA Academy (Interactive Learning)
* **FR-037:** The system shall provide structured interactive tutorials covering key prompting paradigms: Zero-Shot, Few-Shot, Role Prompting, Delimiter Framing, ReAct, Chain-of-Thought, and Structured Output.
* **FR-038:** The system shall provide hands-on interactive labs where user-authored prompts are evaluated in real time against golden standard rubrics.

### Module 10: Settings & Provider Management
* **FR-039:** The system shall securely store API keys for Google Gemini, Groq, OpenAI, and Anthropic in OS-native credential vaults (Windows Credential Manager, macOS Keychain, Linux Secret Service).
* **FR-040:** The system shall auto-discover local Ollama instances running on `http://localhost:11434` and enumerate available local models.
* **FR-041:** The system shall support theme switching between dark, light, and system-adaptive modes.
* **FR-042:** The system shall provide one-click database compaction, integrity verification, and full database backup/restore.

---

## 6. Non-Functional Requirements (NFR)

### 6.1 Performance & Resource Efficiency
* **NFR-001:** Cold application startup time shall be less than 800 milliseconds on standard desktop hardware (Intel Core i5 / Apple M1 or higher).
* **NFR-002:** Baseline memory usage when idling shall not exceed 70 MB of RAM.
* **NFR-003:** UI rendering must maintain a consistent 60+ FPS during token streaming by throttling DOM updates to a 16ms window.
* **NFR-004:** Full-text search queries over 10,000 prompts shall complete in under 15 milliseconds.

### 6.2 Security & Data Privacy
* **NFR-005:** API keys must never be logged to stdout, persisted to plaintext disk files, or embedded in webview storage.
* **NFR-006:** The application shall emit zero external telemetry, tracking beacons, or analytics packets.
* **NFR-007:** The Tauri native layer shall explicitly whitelist IPC commands and forbid arbitrary child process shell executions.
* **NFR-008:** All user-imported files shall undergo strict size checks (<= 10MB) and encoding validation (UTF-8).

### 6.3 Accessibility (a11y) & Ergonomics
* **NFR-009:** The application shall be 100% navigable via keyboard alone (global hotkeys, focus traps, list arrow navigation).
* **NFR-010:** All interactive elements shall maintain a minimum color contrast ratio of 4.5:1 against their adjacent background surfaces in both dark and light modes.
* **NFR-011:** All icons and custom controls shall feature explicit ARIA labels and roles for screen reader compatibility.

### 6.4 Reliability & Maintainability
* **NFR-012:** The frontend codebase must compile under TypeScript `strict: true` with zero compiler warnings and zero implicit `any` types.
* **NFR-013:** Multi-step database operations must execute within ACID transactions (`BEGIN TRANSACTION ... COMMIT`) to prevent database corruption during unexpected crashes or power losses.
* **NFR-014:** Automated unit test suites must be 100% deterministic and execute completely offline using mock providers.

---

## 7. Out-of-Scope Capabilities (Explicit Exclusions)

To protect focus, architectural clarity, and desktop performance, the following items are explicitly **out of scope** for the core release:
1. **Mandatory SaaS Cloud Hosting:** No central proprietary cloud server hosting user prompts. SUTRA is strictly local-first.
2. **Built-in Paid LLM Reselling:** SUTRA does not act as a credit-reseller or token middleman. Users bring their own free/paid API keys or run local models.
3. **Heavy Web Scraping / Crawling:** SUTRA is an instruction environment, not an autonomous web search bot.
4. **Mobile Native Apps:** SUTRA is optimized for high-density desktop engineering workflows (Windows, macOS, Linux). Mobile native ports are not planned for initial milestones.
