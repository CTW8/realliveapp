# Feature Traceability Matrix

## Purpose
Map requirement IDs to design sources and implementation targets so AI and engineers can trace decisions quickly.

| Requirement Area | PRD Reference | Prototype Reference | Primary Tech Doc | Target Module |
|---|---|---|---|---|
| Auth/Login | AUTH-* | mobile A1-A3, web login | WEB/MOBILE tech docs | `features/auth` |
| Dashboard | DASH-* | mobile B1, web dashboard | WEB tech | `features/dashboard` |
| Live Monitor | LIVE-* | mobile C/D, web monitor | WEB tech | `features/live-grid` |
| Alert Center | ALERT-* | mobile B3, web alerts | WEB tech | `features/alert-center` |
| Rule Engine | RULE-* | web alerts rule board | WEB tech | `features/rule-engine` |
| Playback | PLAY-* | mobile E, web playback | WEB tech | `features/playback-query` |
| Device Mgmt | DEV-* | mobile F, web devices | WEB/MOBILE tech docs | `features/device-management` |
| Storage | STO-* | mobile G3, web storage | WEB/MOBILE tech docs | `features/storage-center` |
| Settings/Security | SET-* | mobile G4/H8, web settings | WEB tech | `features/settings-center` |
| Global UX | GLB-* | both prototypes | WEB/MOBILE tech docs | `components/feedback`, `components/modal` |

## Rule-Specific Constraints (Must Enforce)
- Name length: 4-48
- Name uniqueness: case-insensitive per tenant
- Condition min length: 12
- Actions min length: 8
- High priority rules:
  - quiet hours must be Disabled
  - enabled must be true
  - escalation cannot be After 180 seconds

## Verification References
- Acceptance and UAT: `/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`
- Sprint and story breakdown: `/Users/lizhen/Documents/vsproj/realliveapp/JIRA_BREAKDOWN.md`
