# RealLive UI Prototype

RealLive 是一个远程监控 App 的静态高保真 UI 原型仓库，当前以单文件页面展示完整业务流程（认证、实时监看、回看、设备管理、账号安全）。

## Quick Start

本项目无需安装依赖，直接本地启动静态服务即可预览：

```bash
python3 -m http.server 8080
```

打开浏览器访问：

```text
http://localhost:8080
```

## Repository Structure

- `index.html`: 主原型文件（HTML + CSS + 页面流程）
- `UI_DESIGN.md`: 详细 UI 设计说明（偏实现规范）
- `UI_DESIGN_REVIEW.md`: 评审版说明（偏产品与体验价值）
- `UI_ONE_PAGER.md`: 一页式汇报版（适合飞书/Confluence）
- `AGENTS.md`: 仓库协作与贡献指南

## Documentation Index

建议阅读顺序：

1. `UI_ONE_PAGER.md`：快速了解项目目标与成果
2. `UI_DESIGN_REVIEW.md`：用于方案评审与排期讨论
3. `UI_DESIGN.md`：用于开发落地与细节对齐

## Notes

- 当前为静态原型，不包含后端接口与真实数据联动。
- 如需扩展为工程化项目，建议下一步拆分 `src/`、`assets/`、`tests/` 目录。

