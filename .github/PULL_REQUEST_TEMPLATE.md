## Summary of Changes
Provide a clear, high-level summary of what this Pull Request introduces or resolves.

## Related Issue
Closes #[ISSUE_NUMBER]

## Category of Change
- [ ] `feat`: New feature or capability
- [ ] `fix`: Bug fix
- [ ] `docs`: Documentation addition or correction
- [ ] `refactor`: Structural code cleanup (zero behavioral change)
- [ ] `perf`: Performance enhancement
- [ ] `test`: Unit, integration, or E2E test suite additions
- [ ] `ci` / `chore`: Build scripts, dependencies, or GitHub actions

## Verification & Testing Performed
Describe the automated tests and manual verification performed:
* [ ] TypeScript check passed: `pnpm tsc --noEmit` (0 errors)
* [ ] Linting passed: `pnpm lint` (0 warnings)
* [ ] Unit tests passed: `pnpm test` (Vitest)
* [ ] Rust tests passed (if applicable): `cargo test`
* [ ] Offline test verification: verified with `MockProvider` (no paid API calls made)

## Screenshots or Terminal Output (if applicable)
Add before/after screenshots or terminal test outputs.

## Security & Privacy Considerations
- [ ] No API keys, secrets, or `.env` files are committed.
- [ ] External network requests (if any) route strictly through `IAIProvider` abstractions.
- [ ] File operations enforce size limits (<10MB) and path sandboxing.
- [ ] No tracking scripts or analytics beacons have been introduced.

## Contributor Checklist
- [ ] My code adheres to the engineering standards in [AGENTS.md](AGENTS.md).
- [ ] I have updated corresponding documentation in `docs/` if architectural or command changes were made.
- [ ] My commit messages follow the Conventional Commits specification.
