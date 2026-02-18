# RealLive Web 开发技术拆解（详细版）

## 1. 目标与范围
- 目标：将 `prototypes/server-web/index.html` 的静态原型拆解为可工程化开发的 Web 版本（可迭代、可测试、可上线）。
- 范围：Web Console（Login、Dashboard、Live Monitor、Playback、Alert Center、Devices、Storage、Settings）。
- 非范围：移动端实现、底层视频协议栈、支付系统闭环。

## 2. 原型能力盘点（代码级）

## 2.1 页面路由（原型）
- `page-login`
- `page-dashboard`
- `page-monitor`
- `page-playback`
- `page-alerts`
- `page-devices`
- `page-storage`
- `page-settings`

## 2.2 核心交互函数（原型）
- 导航/壳：`showApp`、`switchPage`、`checkResponsive`、`toggleSidebar`
- 设置页：`switchSettingsTab`
- 反馈层：`showToast`、`openQuickModal`、`openRuleModal`、`openPatrolModal`
- 地图筛选：`setMapFilter`、`setMapGroup`、`applyMapFilters`
- 规则中心：`applyRuleTableView`、`validateRuleForm`、`saveRuleModal`、`batchDeleteRules`
- Live 布屏：`setGrid`、拖拽相关事件、`togglePatrolRun`
- 告警抽屉：`openAlertDrawer`、`closeAlertDrawer`
- 设备视图：`switchDeviceView`
- 回放侧栏：`syncPlaybackCameraState`、搜索/全选逻辑

## 2.3 明确的业务约束（原型已体现）
1. 规则名长度限制（4-48）。
2. 规则名唯一性检查（前端内存层）。
3. 高优先级规则不能静默、不能禁用、不能 180s 延迟升级。
4. 批量删除有确认弹层和 Undo（6 秒）。
5. ESC 统一关闭可关闭层（抽屉/弹层/Undo/Quick Modal 等）。

## 3. 工程化目标架构（Web）

## 3.1 推荐技术栈
- 框架：React + TypeScript + Vite
- 路由：React Router
- 服务状态：TanStack Query
- 本地状态：Zustand（或 Redux Toolkit）
- 表单与校验：React Hook Form + Zod
- 样式：CSS Modules 或 Tailwind（推荐保持设计 token，自研组件）
- 测试：Vitest + Testing Library + Playwright

## 3.2 代码目录建议
```text
web/
  src/
    app/
      router.tsx
      providers.tsx
      store/
    pages/
      login/
      dashboard/
      monitor/
      playback/
      alerts/
      devices/
      storage/
      settings/
    features/
      auth/
      map-filter/
      live-grid/
      alert-center/
      rule-engine/
      playback-query/
      device-management/
      storage-center/
      settings-center/
    components/
      layout/
      feedback/
      table/
      form/
      modal/
      drawer/
      chart/
    services/
      api-client.ts
      endpoints/
    types/
    utils/
    styles/
      tokens.css
      global.css
```

## 3.3 分层约束
1. `pages` 只做页面组装，不直接写请求细节。
2. `features` 承担业务状态与交互逻辑。
3. `services` 统一管理 API、鉴权、错误处理。
4. `components` 仅承载可复用 UI，不耦合业务数据源。

## 4. Web 模块技术拆解

## 4.1 App Shell（导航与全局层）
### 4.1.1 模块职责
- 侧边导航、顶部栏、标题联动、响应式折叠、页面容器。
- 全局 ESC 行为和全局反馈（Toast）协调。

### 4.1.2 组件拆分
- `AppShellLayout`
- `SidebarNav`
- `Topbar`
- `PageContainer`
- `GlobalToastHost`
- `GlobalOverlayManager`

### 4.1.3 状态模型
```ts
type ShellState = {
  activePage: 'dashboard'|'monitor'|'playback'|'alerts'|'devices'|'storage'|'settings';
  sidebarCollapsed: boolean;
  sidebarOpenMobile: boolean;
}
```

### 4.1.4 开发任务
1. 建立路由与导航映射（路径与菜单项一一对应）。
2. 实现断点行为（>=1024 固定侧栏，<1024 抽屉侧栏）。
3. 统一 ESC 事件分发（优先关闭最上层弹层）。

## 4.2 Dashboard + Map Filter
### 4.2.1 模块职责
- 指标卡、快速入口、地图标记、设备列表、分组计数联动。

### 4.2.2 核心状态
```ts
type MapFilterState = {
  status: 'all'|'online'|'recording'|'offline';
  zone: 'all'|'a'|'b';
  group: 'all'|'entry'|'perimeter'|'critical';
}
```

### 4.2.3 API 依赖
- `GET /dashboard/overview`
- `GET /dashboard/map-devices?status=&zone=&group=`

### 4.2.4 关键技术点
1. 筛选联动采用纯函数计算，避免状态漂移。
2. 统计数字由后端返回 + 前端兜底重算。
3. 点击地图设备跳转 Live 时应携带 `cameraId` 参数。

### 4.2.5 风险点
- 大量标记渲染性能下降：需要标记聚合/分批渲染策略。

## 4.3 Live Monitor（网格、拖拽、PTZ、轮巡）
### 4.3.1 模块职责
- 网格布局切换、拖拽布屏、单元激活、聚焦模式、实时操作。

### 4.3.2 状态模型
```ts
type GridSize = 1|2|3|4;
type CameraTile = { slot: number; cameraId?: string; status?: 'online'|'recording'|'offline' };
type LiveState = {
  gridSize: GridSize;
  activeSlot?: number;
  tiles: CameraTile[];
  patrolRunning: boolean;
  patrolIndex: number;
}
```

### 4.3.3 API 依赖
- `GET /live/cameras`
- `POST /live/ptz`
- `POST /live/snapshot`
- `POST /live/patrol/plan`

### 4.3.4 关键技术点
1. 拖拽使用 `dnd-kit` 或原生 DnD；建议 `dnd-kit` 以提升可维护性。
2. 网格切换时保留已有分配（超出格子的分配入“待分配池”）。
3. 聚焦模式不破坏原布局数据，只切换视图层。
4. 离线设备按钮禁用策略统一由 `statusGuard` 实现。

### 4.3.5 测试重点
- 布局切换、拖拽覆盖、ESC 恢复、离线按钮禁用。

## 4.4 Alert Center（列表/批量/详情）
### 4.4.1 模块职责
- 告警列表、筛选、批量操作、详情抽屉、分页。

### 4.4.2 状态模型
```ts
type AlertFilter = {
  type: 'all'|'motion'|'alarm'|'offline'|'system';
  range: 'today'|'7d'|'30d'|'custom';
  page: number;
  pageSize: number;
}
```

### 4.4.3 API 依赖
- `GET /alerts`
- `PATCH /alerts/batch`
- `GET /alerts/{id}`
- `POST /alerts/export`

### 4.4.4 关键技术点
1. 表格行点击与 checkbox 事件冲突需阻断冒泡。
2. 批量操作前显示操作影响数（已选数量）。
3. 抽屉详情与列表状态更新同步（标记已读）。

## 4.5 Rule Engine（规则自动化）
### 4.5.1 模块职责
- 规则列表、筛选排序分页、编辑弹窗、批量操作、Undo 撤销。

### 4.5.2 状态模型
```ts
type RuleFilter = {
  priority: 'all'|'high'|'medium'|'low';
  enabled: 'all'|'enabled'|'disabled';
  escalation: 'all'|'Immediately'|'After 60 seconds'|'After 180 seconds';
  query: string;
  sortBy: 'priority'|'name'|'escalation';
  order: 'asc'|'desc';
  page: number;
  pageSize: number;
}
```

### 4.5.3 前端校验规则（与后端一致）
1. `name.length` 在 4-48。
2. 规则名租户内唯一（忽略大小写）。
3. `condition.length >= 12`。
4. `actions.length >= 8`。
5. `priority=high` 时：
  - `quietHours === Disabled`
  - `enabled === true`
  - `escalation !== After 180 seconds`

### 4.5.4 API 依赖
- `GET /alert-rules`
- `POST /alert-rules`
- `PATCH /alert-rules/{id}`
- `POST /alert-rules/batch`
- `DELETE /alert-rules/{id}`

### 4.5.5 关键技术点
1. 列表状态由 Query 参数驱动，保证可分享 URL。
2. Undo 采用“客户端暂存删除快照 + TTL”策略。
3. 批量操作支持 scope（currentPage / filteredAll）。

## 4.6 Playback（日期、时间轴、多相机）
### 4.6.1 模块职责
- 日期选择、事件列表、时间轴定位、多相机选择与搜索。

### 4.6.2 状态模型
```ts
type PlaybackState = {
  date: string;
  selectedCameraIds: string[];
  keyword: string;
  speed: 0.5|1|1.5|2|4;
  cursorTime?: string;
}
```

### 4.6.3 API 依赖
- `GET /playback/calendar`
- `GET /playback/events`
- `GET /playback/stream-meta`
- `POST /playback/export`

### 4.6.4 关键技术点
1. “全选/清空”只针对当前可见项还是全量项需提前定规则（建议当前过滤结果）。
2. 搜索过滤不自动清空已选集合。
3. 事件跳转需驱动播放器与时间轴同步。

## 4.7 Device Management（表格/卡片）
### 4.7.1 模块职责
- 设备列表筛选、视图切换、排序、批量操作入口。

### 4.7.2 API 依赖
- `GET /devices`
- `PATCH /devices/{id}`
- `POST /devices/batch`
- `DELETE /devices/{id}`

### 4.7.3 关键技术点
1. 表格与卡片共享同一数据源和筛选状态。
2. 删除操作必须通过 `ConfirmDangerModal` 统一处理。

## 4.8 Storage（容量/趋势/策略）
### 4.8.1 模块职责
- 展示容量结构、趋势预测、设备占用明细、策略配置。

### 4.8.2 API 依赖
- `GET /storage/overview`
- `GET /storage/trend`
- `GET /storage/by-device`
- `PATCH /storage/policy`

### 4.8.3 关键技术点
1. 图表组件要支持空态和低数据量场景。
2. 策略保存需做 optimistic update 或保存中锁定。

## 4.9 Settings（Profile/Security/Users/Audit/Notifications/System）
### 4.9.1 模块职责
- 设置分区管理与多治理能力承载。

### 4.9.2 子模块拆分
- `settings-profile`
- `settings-security`
- `settings-users-roles`
- `settings-audit`
- `settings-notifications`
- `settings-system`

### 4.9.3 API 依赖
- `GET/PATCH /settings/profile`
- `GET/PATCH /settings/security`
- `GET/POST/PATCH /users`
- `GET /audit-logs`、`POST /audit-logs/export`
- `GET/PATCH /settings/notifications`
- `GET/PATCH /settings/system`

### 4.9.4 关键技术点
1. Tab 切换使用路由子路径（如 `/settings/security`）提升可直达性。
2. 安全与系统动作必须记录审计日志。
3. 渠道测试按钮需要防抖/节流，避免误触发风暴。

## 5. 公共技术模块拆解

## 5.1 API Client
- 统一鉴权头注入、401 刷新、错误码映射、重试策略。
- 支持 request-id 透传，便于链路追踪。

## 5.2 权限控制（RBAC）
- 前端权限点：
  - `live.control`
  - `rule.write`
  - `device.manage`
  - `users.manage`
  - `audit.export`
  - `system.write`
- 控制方式：
  - 路由守卫 + 按钮级禁用/隐藏双策略。

## 5.3 反馈与弹层中心
- 统一 `ModalService` / `DrawerService` / `ToastService`。
- 统一 z-index 管理，避免层级冲突。
- ESC 关闭按“最后打开优先”策略执行。

## 5.4 表单与校验
- Zod schema 与后端 DTO 一一对齐。
- 所有高风险表单必须显式错误摘要区。

## 6. 数据类型定义（前端 TypeScript）
```ts
type DeviceStatus = 'online'|'recording'|'offline';
type AlertStatus = 'new'|'read'|'resolved'|'archived';
type RulePriority = 'low'|'medium'|'high';
type Role = 'admin'|'operator'|'viewer';
```

## 7. Web 开发任务清单（技术视角）

## 7.1 Phase 0（工程基建）
1. 初始化 React + TS + 路由 + Query + Store。
2. 接入设计 token 与基础组件库。
3. 建立 API client、错误边界、日志上报。

## 7.2 Phase 1（壳与核心页面）
1. 完成 Shell + Login + Dashboard + Live。
2. 打通基础数据接口。
3. 完成全局反馈和响应式框架。

## 7.3 Phase 2（复杂交互域）
1. Alert + Rule + Playback。
2. 完成批量操作、Undo、规则强校验。
3. 完成抽屉/弹层/分页联动。

## 7.4 Phase 3（治理域）
1. Devices + Storage + Settings 全量。
2. 完成 IAM、Audit、Notifications、System 配置。
3. 完成高风险操作审计链路。

## 8. 测试拆解（Web）

## 8.1 单元测试
- 规则校验函数（高优先级约束、唯一性、长度限制）。
- 地图筛选函数（状态/区域/分组组合）。
- 分页计算与排序函数。

## 8.2 组件测试
- RuleModal 提交行为与错误展示。
- LiveGrid 拖拽赋值与布局切换。
- AlertTable 批量选择与抽屉联动。

## 8.3 E2E（Playwright）
1. 登录 -> Dashboard -> Live -> 返回。
2. 告警筛选 -> 打开详情 -> 批量已读。
3. 规则新建（非法）-> 校验失败 -> 修正 -> 保存成功。
4. 规则批量删除 -> Undo 恢复。
5. Playback 日期切换 + 相机全选。
6. Settings 多 Tab 切换 + 保存反馈。

## 9. 性能与稳定性要求（Web）
1. 首屏可交互（Dashboard）<= 2.5s。
2. 列表筛选/排序前端反馈 <= 200ms（1000 条）。
3. 大表格建议虚拟滚动（>500 行触发）。
4. 路由级代码分割，减少首包。
5. 异常链路（接口失败、超时）必须有 UI 兜底。

## 10. 风险与技术对策
| 风险 | 技术对策 |
|---|---|
| 单文件原型逻辑耦合高 | 按 feature 拆分，先抽纯函数，再迁移交互 |
| 规则域复杂、易回归 | 规则校验 schema 单测覆盖 + E2E 回归 |
| 弹层层级冲突 | 统一 Overlay Manager 和层级规范 |
| 响应式断点交互复杂 | 每个断点单独截图基线 + E2E viewport 测试 |
| 实时数据抖动 | Query 缓存 + debounce + 局部更新 |

## 11. Web DoD（Definition of Done）
1. 功能满足 PRD 验收项，且通过 FE 自测 + QA 用例。
2. 关键交互有自动化覆盖（至少 1 条 E2E）。
3. 错误态、空态、加载态完整。
4. 埋点、日志、审计链路完整。
5. 文档更新：接口、状态枚举、已知限制。

## 12. 下一步建议
1. 先落地 Phase 0 的工程骨架（1-2 天）。
2. 先实现 Web Shell + Dashboard + Live 基线链路。
3. 再进入 Alert/Rule 的高复杂域，最后做治理域（Settings/Storage）。
