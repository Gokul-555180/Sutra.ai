# SUTRA.ai — Master Documentation Index

> **Project:** SUTRA.ai  
> **Tagline:** *"Where thoughts become instructions."*  
> **Repository:** [https://github.com/Gokul-555180/Sutra.ai](https://github.com/Gokul-555180/Sutra.ai)  
> **Documentation Version:** 1.0.0 (Foundational Baseline)  

Welcome to the centralized documentation portal for **SUTRA.ai**, the free, local-first Desktop Prompt Engineering Environment. This index organizes all technical, product, design, and governance specifications across the repository.

---

## 1. Documentation Map by Domain

```
docs/
├── INDEX.md (This Portal)
├── product/
│   ├── PRODUCT_REQUIREMENTS.md       # Product Vision, Personas, FRs, NFRs
│   ├── FEATURE_SPECIFICATIONS.md     # Engineering specs for 10 core modules
│   ├── USER_FLOWS.md                 # Interactive user journeys & Mermaid flows
│   └── ROADMAP.md                    # 16-week, 6-phase engineering timeline
├── technical/
│   ├── TECH_STACK.md                 # Detailed rationale for Tauri 2, Rust, React, SQLite
│   ├── AI_SYSTEM_DESIGN.md           # Prompt Intelligence Engine & Provider Abstraction
│   ├── DATABASE_DESIGN.md            # SQLite schema, ERD, FTS5 virtual tables
│   ├── API_DESIGN.md                 # IAIProvider interfaces & Tauri IPC command contracts
│   └── SECURITY_DESIGN.md            # STRIDE threat model & OS Keyring vault architecture
├── design/
│   ├── DESIGN_SYSTEM.md              # Proposed dark-first design tokens, typography, colors
│   ├── UI_UX_GUIDELINES.md           # Ergonomics, keyboard shortcuts, non-blocking UI
│   └── BRAND_GUIDELINES.md           # Etymology, voice, tone, visual identity
├── development/
│   ├── DEVELOPMENT_GUIDE.md          # Setup, prerequisites, planned scripts, debugging
│   ├── TESTING_STRATEGY.md           # Vitest, RTL, MockProvider, Playwright, Cargo tests
│   └── RELEASE_PROCESS.md            # SemVer, packaging, WiX/NSIS installers, rollback
├── decisions/
│   └── ADR-0001-TECHNOLOGY-SELECTION.md # Key trade-offs & runtime selection rationale
└── learning/
    ├── README.md                     # Guide to weekly reflective engineering journals
    └── WEEKLY_LEARNING_TEMPLATE.md   # Standard template for 16-week learning logs
```

---

## 2. Directory & Root Documents Directory

### Core Root Governance
* [README.md](../README.md) — Public landing page, project overview, highlights, and quickstart.
* [ARCHITECTURE.md](../ARCHITECTURE.md) — Comprehensive technical architecture specification.
* [AGENTS.md](../AGENTS.md) — Strict engineering invariants and rules for AI coding agents.
* [CONTRIBUTING.md](../CONTRIBUTING.md) — Contribution guidelines, conventional commits, PR workflows.
* [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) — Community pledge and standard of conduct.
* [SECURITY.md](../SECURITY.md) — Security policy, vulnerability reporting, and credential guarantees.
* [CHANGELOG.md](../CHANGELOG.md) — Keep a Changelog historical and unreleased records.

### Product Specifications (`docs/product/`)
* [PRODUCT_REQUIREMENTS.md](product/PRODUCT_REQUIREMENTS.md) — High-level product requirements document (PRD), user personas, functional and non-functional requirements.
* [FEATURE_SPECIFICATIONS.md](product/FEATURE_SPECIFICATIONS.md) — Exhaustive functional specifications for Modules 1 through 10.
* [USER_FLOWS.md](product/USER_FLOWS.md) — Step-by-step user journeys with Mermaid sequence and flow diagrams.
* [ROADMAP.md](product/ROADMAP.md) — Structured 16-week execution roadmap spanning 6 implementation phases.

### Technical Architecture & Design (`docs/technical/`)
* [TECH_STACK.md](technical/TECH_STACK.md) — Full evaluation of Tauri 2, Rust, React, SQLite, Zustand, and CodeMirror.
* [AI_SYSTEM_DESIGN.md](technical/AI_SYSTEM_DESIGN.md) — Pipeline architecture, hybrid deterministic/LLM reasoning, rate limiting, and provider abstraction.
* [DATABASE_DESIGN.md](technical/DATABASE_DESIGN.md) — Relational SQLite entity relationship model, migrations, and FTS5 search indexing.
* [API_DESIGN.md](technical/API_DESIGN.md) — Standardized TypeScript provider interfaces and Tauri IPC command contracts.
* [SECURITY_DESIGN.md](technical/SECURITY_DESIGN.md) — STRIDE threat modeling, OS Keyring integration, and sandboxed file ingestion.

### Design System & Ergonomics (`docs/design/`)
* [DESIGN_SYSTEM.md](design/DESIGN_SYSTEM.md) — Semantic color tokens, typography scales, spacing grids, and component primitives.
* [UI_UX_GUIDELINES.md](design/UI_UX_GUIDELINES.md) — Keyboard-first navigation, shortcut matrix, non-blocking streaming ergonomics.
* [BRAND_GUIDELINES.md](design/BRAND_GUIDELINES.md) — Philosophical roots of Sutra, tone of voice, visual direction, and conceptual logo.

### Development, Quality & Release (`docs/development/`)
* [DEVELOPMENT_GUIDE.md](development/DEVELOPMENT_GUIDE.md) — Local workstation setup, prerequisites, development commands, and debugging guides.
* [TESTING_STRATEGY.md](development/TESTING_STRATEGY.md) — Deterministic testing policy, Vitest unit testing, `MockProvider`, and Playwright E2E.
* [RELEASE_PROCESS.md](development/RELEASE_PROCESS.md) — Semantic versioning protocol, Tauri packaging, WiX installer validation, and hotfix handling.

### Architectural Decision Records (`docs/decisions/`)
* [ADR-0001-TECHNOLOGY-SELECTION.md](decisions/ADR-0001-TECHNOLOGY-SELECTION.md) — Architectural trade-off analysis: Tauri 2 vs Electron, React vs Svelte, SQLite vs Cloud.

### Learning & Reflection (`docs/learning/`)
* [docs/learning/README.md](learning/README.md) — Instructions for logging technical growth during the 16-week build.
* [WEEKLY_LEARNING_TEMPLATE.md](learning/WEEKLY_LEARNING_TEMPLATE.md) — Standardized template for tracking weekly objectives, debugging breakthroughs, and lessons learned.

### Community & Contribution Templates (`.github/`)
* [Bug Report Template](../.github/ISSUE_TEMPLATE/bug_report.md)
* [Feature Request Template](../.github/ISSUE_TEMPLATE/feature_request.md)
* [Documentation Improvement Template](../.github/ISSUE_TEMPLATE/documentation.md)
* [Pull Request Review Template](../.github/PULL_REQUEST_TEMPLATE.md)

---

## 3. Recommended Reading Orders

Depending on your role, read the documentation in the following sequence to build a clear, coherent mental model:

### 1. For New Software Developers
1. [README.md](../README.md) → High-level orientation and philosophy.
2. [docs/technical/TECH_STACK.md](technical/TECH_STACK.md) → Understanding the libraries and tools.
3. [docs/development/DEVELOPMENT_GUIDE.md](development/DEVELOPMENT_GUIDE.md) → Local environment configuration.
4. [ARCHITECTURE.md](../ARCHITECTURE.md) → System layers and module architecture.
5. [docs/development/TESTING_STRATEGY.md](development/TESTING_STRATEGY.md) → Test suites and mock providers.
6. [CONTRIBUTING.md](../CONTRIBUTING.md) → Git conventions and pull request etiquette.

### 2. For Autonomous AI Coding Agents
1. [AGENTS.md](../AGENTS.md) → **Mandatory Invariants:** TypeScript strictness, component sizing, security rules.
2. [ARCHITECTURE.md](../ARCHITECTURE.md) → 6-layer architecture, module responsibilities, folder structures.
3. [docs/technical/API_DESIGN.md](technical/API_DESIGN.md) → Data contracts, IPC commands, provider interfaces.
4. [docs/product/FEATURE_SPECIFICATIONS.md](product/FEATURE_SPECIFICATIONS.md) → Exact requirements for the targeted feature.
5. [docs/design/DESIGN_SYSTEM.md](design/DESIGN_SYSTEM.md) → Design tokens and styling guidelines.

### 3. For Product Managers & Functional Contributors
1. [README.md](../README.md) → Vision and problem statement.
2. [docs/product/PRODUCT_REQUIREMENTS.md](product/PRODUCT_REQUIREMENTS.md) → Functional and non-functional requirements.
3. [docs/product/FEATURE_SPECIFICATIONS.md](product/FEATURE_SPECIFICATIONS.md) → Module-by-module feature definitions.
4. [docs/product/USER_FLOWS.md](product/USER_FLOWS.md) → User journeys and interaction flows.
5. [docs/product/ROADMAP.md](product/ROADMAP.md) → 16-week delivery roadmap.

### 4. For UI / UX Designers & Frontend Stylists
1. [docs/design/BRAND_GUIDELINES.md](design/BRAND_GUIDELINES.md) → Brand personality and philosophy.
2. [docs/design/DESIGN_SYSTEM.md](design/DESIGN_SYSTEM.md) → Semantic color palette, typography, spacing, border tokens.
3. [docs/design/UI_UX_GUIDELINES.md](design/UI_UX_GUIDELINES.md) → Ergonomics, non-blocking token streaming, keyboard shortcuts.
4. [docs/product/USER_FLOWS.md](product/USER_FLOWS.md) → Interactive state transitions and modals.
