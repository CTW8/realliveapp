# RealLive UI Prototypes

RealLive 是远程监控产品的原型仓库，当前包含两套静态页面：
- Android 手机端 App UI（巡检、实时监看、回放、告警、设备配置）
- 服务器端 Web Console UI（运营与运维管理后台）

## Quick Start

项目无需安装依赖，使用静态服务即可预览：

```bash
python3 -m http.server 8080
```

打开浏览器访问：

- Mobile App: `http://localhost:8080/prototypes/mobile-app/index.html`
- Server Web: `http://localhost:8080/prototypes/server-web/web-console.html`

## Repository Structure

- `prototypes/mobile-app/index.html`: 移动端主原型（单文件 HTML+CSS+JS）
- `prototypes/mobile-app/UI_DESIGN.md`: 移动端详细设计说明
- `prototypes/mobile-app/UI_DESIGN_REVIEW.md`: 移动端评审说明
- `prototypes/mobile-app/UI_ONE_PAGER.md`: 移动端一页式说明
- `prototypes/server-web/web-console.html`: 服务器端 Web 控制台原型
- `prototypes/server-web/WEB_UI_DESIGN.md`: 服务器端 Web 页面说明
- `AGENTS.md`: 仓库协作与贡献指南

## Notes

- 当前均为静态原型，不包含真实后端接口与鉴权。
- 目标是快速演示业务流程、页面跳转与布局规范，不是生产代码。
