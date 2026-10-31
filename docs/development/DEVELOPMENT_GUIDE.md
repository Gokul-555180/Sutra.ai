# SUTRA.ai — Comprehensive Developer Guide

> **Document Version:** 1.0.0  
> **Status:** Active Developer Instructions  
> **Target Audience:** Core Contributors, Software Engineers, AI Agents  
> **Related Documents:** [ARCHITECTURE.md](../../ARCHITECTURE.md), [TESTING_STRATEGY.md](TESTING_STRATEGY.md), [AGENTS.md](../../AGENTS.md)

---

## 1. Prerequisites & System Configuration

### Core Toolchains
1. **Rust (2021 Edition):** Stable version 1.77+ with `cargo` and `rustup`.
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   rustc --version
   ```
2. **Node.js:** v18.0.0+ LTS or v20.0.0+ LTS.
3. **pnpm:** v8.0.0+ (recommended) or `npm`.
   ```bash
   npm install -g pnpm
   ```

### Platform-Specific Desktop Build Dependencies

#### Windows Setup
* Visual Studio Community with **C++ build tools** workload.
* Microsoft Edge **WebView2 Runtime** (installed by default on Windows 10/11).
* Rust target: `x86_64-pc-windows-msvc`.

#### macOS Setup
* Xcode Command Line Tools: `xcode-select --install`

#### Linux Setup (Debian / Ubuntu)
```bash
sudo apt update
sudo apt install -y build-essential curl wget file libssl-dev libgtk-3-dev \
    libwebkit2gtk-4.1-dev libayatana-appindicator3-dev librsvg2-dev
```

---

## 2. Planned Development Commands

> [!NOTE]
> SUTRA.ai is in active foundational setup. The following scripts represent the standardized project scripts planned for Phase 1 initialization:

```bash
# 1. Install frontend dependencies
pnpm install

# 2. Launch Tauri desktop app in dev mode (with Vite Hot Reload)
pnpm tauri dev

# 3. Run TypeScript type check with strict compiler
pnpm tsc --noEmit

# 4. Run ESLint code quality scan
pnpm lint

# 5. Run Vitest unit & integration test suite
pnpm test

# 6. Run Rust backend unit & migration tests
cargo test --manifest-path src-tauri/Cargo.toml

# 7. Build production packaged desktop installer
pnpm tauri build
```

---

## 3. Environment & Credential Configuration

* **Zero Plaintext Secrets:** SUTRA.ai never reads API keys from `.env` files in production.
* **Local Development Overrides:** For local integration tests, mock providers are enabled by default. If live provider testing is needed during local debugging, keys can be temporarily provided via environment variables (`GEMINI_API_KEY`, `GROQ_API_KEY`) strictly during isolated test runs. Never commit `.env` files to git.

---

## 4. Debugging & Common Troubleshooting

### Webview Developer Tools
In development mode (`pnpm tauri dev`), right-click anywhere within the window and select **Inspect Element** to open the browser developer console and React component inspector.

### Rust Backend Logs
Native Rust logs are streamed directly to your terminal. Set log verbosity:
```powershell
$env:RUST_LOG="sutra=debug,tauri=info"
pnpm tauri dev
```

### Common Issues
1. **Windows C++ Linker Error:** Ensure Microsoft C++ Build Tools are installed and `vcvars64.bat` is in your environment PATH.
2. **Linux WebKit Missing:** Ensure `libwebkit2gtk-4.1-dev` (not 4.0) is installed on modern distributions (Ubuntu 24.04+).
