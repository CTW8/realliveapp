# Repository Guidelines

## Project Structure & Module Organization
This repository is currently a single-page UI prototype.

- `index.html`: Main source file containing HTML, CSS, and static UI flows.
- `.claude/`: Local assistant/tooling metadata (do not depend on this in runtime code).
- No separate `src/`, `tests/`, or asset pipeline exists yet.

When adding new files, keep structure simple and predictable. If the project grows, prefer:
- `src/` for app code
- `assets/` for images/icons/fonts
- `tests/` for automated tests

## Build, Test, and Development Commands
No build system is configured. Use static preview commands:

- `python3 -m http.server 8080`
  - Serves the repository locally for browser testing.
- `open http://localhost:8080` (macOS)
  - Opens the preview URL.
- `git diff -- index.html`
  - Review UI/CSS changes before commit.

## Coding Style & Naming Conventions
- Use 2-space indentation in HTML/CSS blocks.
- Follow existing class naming patterns (short, component-like classes such as `.lv-video`, `.tl-item`).
- Keep CSS edits scoped; prefer updating existing selectors over adding one-off inline styles.
- Preserve ASCII unless non-ASCII text is required (e.g., localized UI labels).
- Group related style rules by feature section comments (e.g., `/* -- History / Playback -- */`).

## Testing Guidelines
There is no automated test framework yet. Validate changes manually:

- Verify layouts at common widths, especially within the phone frame.
- Check text overflow, icon alignment, and scroll behavior after CSS changes.
- For UI changes, include before/after screenshots in the PR.

If tests are later added, place them under `tests/` and use `feature-name.spec.*` naming.

## Commit & Pull Request Guidelines
Current history uses short commit messages (e.g., `优化细节 1`, `init project`). Keep commits focused and concise.

- Commit format: short imperative summary; one logical change per commit.
- PRs should include:
  - What changed and why
  - Affected screens/sections (e.g., Live View, History Playback)
  - Visual evidence (screenshots or GIFs)
  - Any manual verification steps performed

