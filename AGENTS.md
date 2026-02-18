# Repository Guidelines

## AI Quick Project Brief (Read First)
RealLive is a remote monitoring product workspace with two high-fidelity static prototypes:
- Android-style mobile app prototype
- Web console prototype

Current repository state:
- The code in `prototypes/*/index.html` is design prototype code, not production architecture.
- Product and engineering specs are already prepared for implementation.

Primary source-of-truth order for AI development:
1. `PRD.md`
2. `docs/INDEX_HTML_DESIGN_BREAKDOWN.md` (index.html-driven breakdown)
3. `WEB_TECH_BREAKDOWN.md` and `MOBILE_TECH_BREAKDOWN.md`
4. Prototype files for exact UI/interaction details:
   - `prototypes/mobile-app/index.html`
   - `prototypes/server-web/index.html`

Non-negotiable business rules (must keep consistent):
- Status semantics: online=green, recording=orange, offline=red.
- Mobile live capability is single-stream only (no multi-grid simultaneous playback on mobile).
- Dangerous actions require confirmation (delete, batch destructive actions, session revoke).
- Rule constraints:
  - Name length 4-48
  - Name unique (case-insensitive in tenant scope)
  - Condition min length 12
  - Actions min length 8
  - High-priority rules cannot be quiet-hours enabled, cannot be disabled, and cannot use `After 180 seconds` escalation.

Recommended AI execution flow:
1. Read `docs/PROJECT_INDEX.md`.
2. Map task -> requirement -> prototype section.
3. Implement smallest complete increment with clear file boundaries.
4. Validate interaction behavior and edge states (loading/empty/error).
5. Report changed files + verification steps + remaining risks.

## Project Structure & Module Organization
This repository contains two static prototypes: Android app UI and server web UI.

- `prototypes/mobile-app/index.html`: Main mobile prototype (single-file HTML/CSS/JS).
- `prototypes/mobile-app/UI_DESIGN.md`: Mobile implementation-focused design notes.
- `prototypes/mobile-app/UI_DESIGN_REVIEW.md`: Mobile review-oriented rationale.
- `prototypes/mobile-app/UI_ONE_PAGER.md`: Mobile one-page summary.
- `prototypes/server-web/index.html`: Server web console prototype.
- `prototypes/server-web/UI_DESIGN.md`: Web console flow and interaction notes.
- `docs/PROJECT_INDEX.md`: Unified project index for AI/human onboarding.
- `AI_CONTEXT.yaml`: Machine-readable project context for AI development.

Keep each prototype self-contained. If the project is productized later, split to `src/`, `assets/`, and `tests/`.

## Build, Test, and Development Commands
No build system is configured. Preview via static hosting:

- `python3 -m http.server 8080`
  - Serve this repository locally.
- `open http://localhost:8080/prototypes/mobile-app/index.html`
  - Open mobile prototype.
- `open http://localhost:8080/prototypes/server-web/index.html`
  - Open web console prototype.
- `git diff -- prototypes/mobile-app/index.html`
  - Review mobile UI changes.
- `git diff -- prototypes/server-web/index.html`
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
