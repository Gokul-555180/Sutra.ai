# Contributing to SUTRA.ai

Thank you for your interest in contributing to **SUTRA.ai**! As an open-source, local-first Prompt Engineering Environment, our goal is to build an industrial-grade, disciplined, and privacy-respecting tool for software engineers, prompt researchers, and everyday AI users.

This document outlines our contribution philosophy, repository setup, branching and commit workflows, coding standards, and review processes.

---

## 1. Core Contribution Philosophy

Before writing code or documentation, familiarize yourself with our guiding tenets:
1. **Local-First & Privacy Conscious:** SUTRA.ai never ships telemetry or tracking scripts. Any feature requiring external network access must be explicitly authorized and transparent to the user.
2. **Provider Agnostic:** Core prompt intelligence must never lock into a specific LLM vendor. Features must interface through our normalized provider abstraction layer.
3. **Explainability Over Magic:** When building optimization, analysis, or structuring algorithms, prioritize human-readable rationale. Users must understand *why* prompts change.
4. **Desktop Craftsmanship:** We hold SUTRA.ai to the standard of high-density, restrained tools like Linear, Raycast, and Cursor. No visual clutter, generic AI gradient gimmicks, or content-shifting layouts.
5. **Quality Over Speed:** A clean, well-tested 50-line contribution that adheres to our architecture is vastly preferred over a 1,000-line monolithic pull request that introduces technical debt.

---

## 2. Reading Order for New Contributors

Before opening issues or pull requests, please read the authoritative documentation:
1. [README.md](README.md) — High-level overview, philosophy, and feature catalog.
2. [ARCHITECTURE.md](ARCHITECTURE.md) — System boundaries, 6-layer architecture, and data pipelines.
3. [AGENTS.md](AGENTS.md) — Rigorous engineering invariants, TypeScript rules, design constraints, and testing protocols.
4. [docs/INDEX.md](docs/INDEX.md) — Master documentation index and subsystem specifications.

---

## 3. Development Prerequisites & Setup

### Prerequisites
* **Git:** 2.30+
* **Node.js:** v18.0.0+ LTS or v20.0.0+ LTS
* **pnpm:** v8.0.0+ (preferred package manager) or `npm`
* **Rust:** Stable 1.77+ with `rustup` and `cargo`
* **Platform Dependencies (Desktop Webview & C++ toolchain):**
  * **Windows:** Visual Studio C++ Build Tools and WebView2 Runtime.
  * **macOS:** Xcode Command Line Tools (`xcode-select --install`).
  * **Linux:** `build-essential`, `libwebkit2gtk-4.1-dev`, `libgtk-3-dev`, `libssl-dev`, `libayatana-appindicator3-dev`, `librsvg2-dev`.

### Cloning & Workspace Initialization
```bash
# Clone the repository
git clone https://github.com/Gokul-555180/Sutra.ai.git
cd Sutra.ai

# Install frontend dependencies (when frontend scaffold is initialized)
pnpm install

# Run the Tauri desktop application in development mode
pnpm tauri dev
```

*Note: SUTRA.ai is currently in its foundational specification phase. Development commands will be fully operational as Phase 1 (Foundation) milestones are committed.*

---

## 4. Branching & Git Workflow

We adhere to a structured Git flow:
* **`main`:** Production-ready and stable development integration branch. Direct commits to `main` are reserved only for foundational repository initialization and emergency release patches.
* **Feature Branches:** Create descriptive branches from `main`:
  * `feat/<feature-name>`: New capabilities (e.g., `feat/rubric-clarity-analyzer`)
  * `fix/<bug-name>`: Bug fixes (e.g., `fix/codemirror-delimiter-highlight`)
  * `docs/<topic>`: Documentation updates (e.g., `docs/update-user-flows`)
  * `refactor/<module>`: Structural code improvements without behavioral changes
  * `test/<suite>`: New or expanded unit/integration tests

---

## 5. Commit Message Conventions

We enforce the **Conventional Commits** specification. Commit messages must be lowercase, imperative, and structured as:

```
<type>(<scope>): <short description>

[optional body explaining context and rationale]

[optional footer referencing issues, e.g., Closes #42]
```

### Allowed Types
* `feat`: A new user-facing feature or domain capability.
* `fix`: A bug fix.
* `docs`: Documentation-only additions or revisions.
* `refactor`: Code refactoring that neither fixes a bug nor adds a feature.
* `perf`: Performance optimizations (e.g., SQLite query indexes, streaming throttles).
* `test`: Adding missing tests or correcting existing tests.
* `build`: Changes that affect the build system or external dependencies.
* `ci`: Changes to CI/CD workflows and configuration files.
* `chore`: Routine maintenance, linting config, or repo housekeeping.

### Examples
* `feat(architect): add interactive clarification question generator`
* `fix(optimizer): correct XML delimiter tag nesting in precision mode`
* `docs(api): document provider streaming backoff specifications`
* `test(analyzer): add unit tests for contradictory instruction detection`

---

## 6. Coding Standards & Review Criteria

All contributions must strictly satisfy the guidelines in [AGENTS.md](AGENTS.md):
1. **TypeScript Strictness:** Zero untyped variables, zero implicit `any`, explicit function return signatures, and Zod schema-first data models.
2. **Modularity & Component Sizing:** Single-responsibility components capped at < 250 lines. Large components must be decomposed into feature sub-components or custom hooks.
3. **Desktop Ergonomics:** Adhere to our dark-first, low-contrast border design system (`#0B0D11` / `#12151B`). Avoid loud gradients, heavy glassmorphism, or non-functional animations.
4. **State Lifecycle:** Every UI view must gracefully handle `Idle`, `Loading/Streaming`, `Empty`, and `Error` states with actionable recovery paths.
5. **Security Invariants:** Never commit secrets, `.env` files, or API credentials. All external AI calls must be initiated explicitly by user intent.
6. **Testing Verification:** New domain algorithms (rubrics, optimizers, parsers) must include deterministic Vitest unit tests. External AI calls must be verified using our in-memory `MockProvider`.

---

## 7. Submitting a Pull Request

1. **Self-Review:** Inspect your git diff before opening a PR (`git diff main...HEAD`). Ensure no temporary debug logs (`console.log`) or untyped assertions remain.
2. **Run Verification Commands:**
   ```bash
   pnpm tsc --noEmit     # TypeScript type-check
   pnpm lint             # ESLint & Prettier validation
   pnpm test             # Vitest test suite
   ```
3. **Open PR:** Use the provided [PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md). Provide a concise summary, describe what was verified, and link any related issues.
4. **Review & Iterate:** Be responsive to feedback from reviewers. Keep discussions professional and focused on architectural rigor and user experience.

---

## 8. Reporting Bugs & Proposing Features

* **Bug Reports:** Search existing issues to prevent duplicates. If not found, use our [Bug Report Template](.github/ISSUE_TEMPLATE/bug_report.md) with exact reproduction steps, OS version, and diagnostic logs.
* **Feature Proposals:** Start with our [Feature Request Template](.github/ISSUE_TEMPLATE/feature_request.md). Clearly describe the problem, proposed solution, and user impact before writing code.
* **Documentation Issues:** Use the [Documentation Template](.github/ISSUE_TEMPLATE/documentation.md) for correcting inaccuracies, broken links, or architectural discrepancies.

Thank you for helping make SUTRA.ai the standard for local-first prompt engineering!
