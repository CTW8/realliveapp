# RealLive UI Prototypes & Engineering Docs

RealLive 是远程监控产品的设计与研发准备仓库，当前包含：
- Android 手机端 App 原型（静态 HTML）
- Web Console 原型（静态 HTML）
- 从设计稿拆解出的 PRD、研发拆解和技术拆解文档

## Quick Start

项目无需安装依赖，使用静态服务即可预览：

```bash
python3 -m http.server 8080
```

打开：
- Mobile App: `http://localhost:8080/prototypes/mobile-app/index.html`
- Web Console: `http://localhost:8080/prototypes/server-web/index.html`

## Recommended Entry (For Human & AI)

先阅读：
- `/Users/lizhen/Documents/vsproj/realliveapp/docs/PROJECT_INDEX.md`

核心文档：
- 产品需求：`/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`
- 研发拆解：`/Users/lizhen/Documents/vsproj/realliveapp/JIRA_BREAKDOWN.md`
- Web 技术拆解：`/Users/lizhen/Documents/vsproj/realliveapp/WEB_TECH_BREAKDOWN.md`
- Mobile 技术拆解：`/Users/lizhen/Documents/vsproj/realliveapp/MOBILE_TECH_BREAKDOWN.md`
- AI 开发手册：`/Users/lizhen/Documents/vsproj/realliveapp/docs/AI_DEVELOPMENT_GUIDE.md`
- 需求追踪矩阵：`/Users/lizhen/Documents/vsproj/realliveapp/docs/FEATURE_TRACEABILITY.md`
- 机器可读上下文：`/Users/lizhen/Documents/vsproj/realliveapp/AI_CONTEXT.yaml`

## Repository Structure

- `prototypes/mobile-app/index.html`: 移动端主原型（单文件 HTML/CSS/JS）
- `prototypes/mobile-app/UI_DESIGN.md`: 移动端设计说明
- `prototypes/mobile-app/UI_DESIGN_REVIEW.md`: 移动端评审说明
- `prototypes/mobile-app/UI_ONE_PAGER.md`: 移动端一页式说明
- `prototypes/server-web/index.html`: Web 控制台主原型（单文件 HTML/CSS/JS）
- `prototypes/server-web/UI_DESIGN.md`: Web 设计说明
- `AGENTS.md`: 仓库协作约束
- `docs/`: AI 与研发统一索引文档

## Notes

- 当前页面为静态高保真原型，不包含真实后端联调。
- 文档已按“从设计到研发”的方式整合，可直接进入工程化开发。
