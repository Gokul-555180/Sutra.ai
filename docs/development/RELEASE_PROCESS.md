# SUTRA.ai — Release Process & Distribution Protocol

> **Document Version:** 1.0.0  
> **Status:** Active Release Policy  
> **Target Audience:** Release Engineers, Maintainers, Core Developers  
> **Related Documents:** [ROADMAP.md](../product/ROADMAP.md), [TESTING_STRATEGY.md](TESTING_STRATEGY.md), [CHANGELOG.md](../../CHANGELOG.md)

---

## 1. Versioning Protocol

SUTRA.ai adheres strictly to [Semantic Versioning 2.0.0](https://semver.org/):

$$\text{MAJOR}.\text{MINOR}.\text{PATCH}$$

* **MAJOR:** Incompatible architectural changes, database schema migrations requiring manual intervention, or major UI overhauls.
* **MINOR:** Backward-compatible new features (e.g., adding a new AI provider adapter, introducing a new optimization mode).
* **PATCH:** Backward-compatible bug fixes, performance optimizations, or documentation corrections.

---

## 2. Pre-Release Quality Checklist

A release candidate branch (`release/vX.Y.Z`) cannot be merged or tagged until every gate passes:

- [ ] **Type Verification:** `pnpm tsc --noEmit` passes with 0 errors.
- [ ] **Lint & Code Style:** `pnpm lint` passes with 0 warnings.
- [ ] **Unit & Domain Tests:** 100% of Vitest unit tests pass.
- [ ] **Native Rust Tests:** `cargo test` passes across all native modules.
- [ ] **E2E Desktop Flows:** Playwright smoke tests pass on Windows, macOS, and Linux runners.
- [ ] **Security Audits:** `pnpm audit` and `cargo audit` report 0 high or critical vulnerabilities.
- [ ] **Changelog Updated:** `CHANGELOG.md` reflects all merged PRs under the new version header.

---

## 3. Desktop Installer Packaging (Tauri Bundler)

Production desktop installers are compiled using Tauri Bundler via GitHub Actions:

| Target Platform | Package Format | Distribution Artifact |
| :--- | :--- | :--- |
| **Windows 10/11** | WiX Toolset / NSIS | `Sutra_x64_en-US.msi`, `Sutra_Setup.exe` |
| **macOS (Intel & Apple Silicon)** | DMG Bundle | `Sutra_Universal.dmg` |
| **Linux (x86_64)** | AppImage & Debian | `Sutra.AppImage`, `sutra_amd64.deb` |

### Validation on Clean Environments
Before public announcement:
1. Verify installer executes cleanly on a fresh Windows sandbox with zero developer tools installed.
2. Verify SQLite database initializes correctly on first launch.
3. Verify clean uninstallation leaves zero orphaned temporary files.

---

## 4. Hotfix & Rollback Procedures

* **Emergency Hotfix:** If a critical bug or security flaw is discovered in production:
  1. Branch directly from the tagged release: `hotfix/vX.Y.Z+1`.
  2. Implement minimal targeted fix with accompanying regression test.
  3. Validate pre-release gates and cut patch release.
* **Release Rollback:** If a release introduces catastrophic local data corruption:
  1. Mark the GitHub release as pre-release / broken.
  2. Issue advisory recommending users maintain their existing version.
  3. Provide a database recovery script to repair affected SQLite instances.
