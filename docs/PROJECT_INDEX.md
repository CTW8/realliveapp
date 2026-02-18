# RealLive Project Index (AI-Friendly)

## Purpose
This file is the single entry point for humans and AI agents to understand this repository fast and start implementation work with minimal ambiguity.

## 1. What This Repo Contains
- Static mobile prototype (Android-oriented)
- Static web console prototype
- Product and engineering planning docs (PRD + Jira breakdown + technical breakdown)

## 2. Source of Truth Order
When requirements conflict, use this priority:
1. `/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`
2. `/Users/lizhen/Documents/vsproj/realliveapp/prototypes/server-web/UI_DESIGN.md`
3. `/Users/lizhen/Documents/vsproj/realliveapp/prototypes/mobile-app/UI_DESIGN.md`
4. Prototypes (`index.html`) for concrete interaction details

## 3. Core Documents
### Product
- `/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`

### Planning / Backlog
- `/Users/lizhen/Documents/vsproj/realliveapp/JIRA_BREAKDOWN.md`

### Engineering
- `/Users/lizhen/Documents/vsproj/realliveapp/WEB_TECH_BREAKDOWN.md`
- `/Users/lizhen/Documents/vsproj/realliveapp/MOBILE_TECH_BREAKDOWN.md`
- `/Users/lizhen/Documents/vsproj/realliveapp/docs/INDEX_HTML_DESIGN_BREAKDOWN.md`
- `/Users/lizhen/Documents/vsproj/realliveapp/docs/FEATURE_TRACEABILITY.md`
- `/Users/lizhen/Documents/vsproj/realliveapp/docs/AI_DEVELOPMENT_GUIDE.md`

### Prototypes
- `/Users/lizhen/Documents/vsproj/realliveapp/prototypes/mobile-app/index.html`
- `/Users/lizhen/Documents/vsproj/realliveapp/prototypes/server-web/index.html`

## 4. Feature → Prototype Mapping (Quick)
- Auth: mobile A group + web login page
- Dashboard: mobile B1 + web dashboard page
- Live monitor: mobile C/D + web monitor page
- Playback: mobile E + web playback page
- Alert center + rule automation: mobile B3 + web alerts page
- Device management: mobile F + web devices page
- Storage: mobile G3 + web storage page
- Settings/Security/IAM/Audit: mobile G4/H8 + web settings page

## 5. Recommended Development Path
1. Build web app shell + routing + global feedback
2. Implement dashboard + live baseline
3. Implement alert + rules + playback
4. Implement devices + storage + settings
5. Add observability + tests + release hardening

Use the detailed plan in:
- `/Users/lizhen/Documents/vsproj/realliveapp/WEB_TECH_BREAKDOWN.md`

## 6. Local Preview Commands
```bash
python3 -m http.server 8080
```
- Mobile: `http://localhost:8080/prototypes/mobile-app/index.html`
- Web: `http://localhost:8080/prototypes/server-web/index.html`

## 7. AI Task Contract (Short)
For every implementation task, AI should produce:
1. Scope and assumptions
2. File-level change plan
3. Code change
4. Test steps and verification result
5. Follow-up risks/open questions

Detailed contract:
- `/Users/lizhen/Documents/vsproj/realliveapp/docs/AI_DEVELOPMENT_GUIDE.md`
