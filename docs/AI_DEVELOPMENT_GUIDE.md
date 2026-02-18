# AI Development Guide

## 1. Goal
Enable AI agents to implement features consistently from this repository without re-discovering project context every time.

## 2. Read Order Before Coding
1. `/Users/lizhen/Documents/vsproj/realliveapp/docs/PROJECT_INDEX.md`
2. `/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`
3. Tech breakdown by platform:
   - Web: `/Users/lizhen/Documents/vsproj/realliveapp/WEB_TECH_BREAKDOWN.md`
   - Mobile: `/Users/lizhen/Documents/vsproj/realliveapp/MOBILE_TECH_BREAKDOWN.md`
4. Prototypes for exact interaction behavior

## 3. Working Rules
1. Do not invent business rules not present in PRD/tech docs.
2. If PRD and prototype disagree, follow PRD and note the mismatch.
3. Keep status semantics consistent:
   - online = green
   - recording = orange
   - offline = red
4. Dangerous actions must require confirmation.
5. Rules domain must enforce high-priority constraints.

## 4. Implementation Checklist (Per Task)
1. Identify requirement ID from PRD.
2. Locate corresponding prototype section.
3. Define file-level implementation scope.
4. Implement feature with error/empty/loading states.
5. Add/update tests.
6. Verify acceptance criteria.
7. Update docs if behavior changed.

## 5. Output Format for AI Task Completion
AI should always provide:
1. What changed (brief)
2. File list
3. Validation executed
4. Known risks/limitations
5. Next actionable options

## 6. Testing Baseline
- Unit tests: business functions and validators
- Component tests: modal/drawer/table interactions
- E2E: key journeys
  - login -> dashboard -> live
  - alert -> detail -> bulk operation
  - rule create/edit/validate/undo
  - playback date + camera selection
  - settings tab switching

## 7. Architecture Guardrails
- Separate page orchestration from feature logic.
- Keep API calls in service layer.
- Use typed DTOs for request/response.
- Centralize modal/toast management.
- Route-driven state where possible for list filters.

## 8. Change Risk Flags
Mark tasks as high-risk if they touch:
- rule validation
- batch delete / undo flows
- auth/session/2FA
- permissions/RBAC
- system settings and audit logs

## 9. Definition of Done
A task is done only if:
1. Acceptance criteria are met.
2. Error/empty/loading states are present.
3. Required tests pass or are explicitly documented as pending.
4. No regression in related critical flows.
5. Docs updated when behavior or contracts changed.
