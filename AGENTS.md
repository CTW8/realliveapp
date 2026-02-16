# Repository Guidelines

## Project Structure & Module Organization
This repository contains two static prototypes: Android app UI and server web UI.

- `prototypes/mobile-app/index.html`: Main mobile prototype (single-file HTML/CSS/JS).
- `prototypes/mobile-app/UI_DESIGN.md`: Mobile implementation-focused design notes.
- `prototypes/mobile-app/UI_DESIGN_REVIEW.md`: Mobile review-oriented rationale.
- `prototypes/mobile-app/UI_ONE_PAGER.md`: Mobile one-page summary.
- `prototypes/server-web/web-console.html`: Server web console prototype.
- `prototypes/server-web/WEB_UI_DESIGN.md`: Web console flow and interaction notes.

Keep each prototype self-contained. If the project is productized later, split to `src/`, `assets/`, and `tests/`.

## Build, Test, and Development Commands
No build system is configured. Preview via static hosting:

- `python3 -m http.server 8080`
  - Serve this repository locally.
- `open http://localhost:8080/prototypes/mobile-app/index.html`
  - Open mobile prototype.
- `open http://localhost:8080/prototypes/server-web/web-console.html`
  - Open web console prototype.
- `git diff -- prototypes/mobile-app/index.html`
  - Review mobile UI changes.
- `git diff -- prototypes/server-web/web-console.html`
  - Review web UI changes.

## Coding Style & Naming Conventions
- Use 2-space indentation in HTML/CSS blocks.
- Prefer short, component-like class names and page-level section blocks.
- Keep CSS scoped by feature area; avoid one-off inline styles.
- Preserve ASCII unless non-ASCII text is required (e.g., localized UI labels).
- Keep route/action hooks explicit with attributes like `data-page`, `data-go`, `data-toast`.

## Testing Guidelines
There is no automated test framework yet. Validate changes manually:

- Check mobile layout overflow, timeline alignment, player height, and status-bar-safe spacing.
- Check web page routing (`hash`/button navigation), modal open/close, and toast feedback.
- For UI changes, include before/after screenshots in the PR.

If tests are later added, place them under `tests/` and use `feature-name.spec.*` naming.

## Commit & Pull Request Guidelines
Current history uses short commit messages (e.g., `优化细节 1`, `init project`). Keep commits focused and concise.

- Commit format: short imperative summary; one logical change per commit.
- PRs should include:
  - What changed and why
  - Affected pages/flows (mobile + web)
  - Visual evidence (screenshots or GIFs)
  - Manual verification steps performed
