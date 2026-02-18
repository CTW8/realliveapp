# Index.html 设计稿拆解（仅基于原型代码）

## 1. 拆解来源
- Mobile 原型：`/Users/lizhen/Documents/vsproj/realliveapp/prototypes/mobile-app/index.html`（1707 行）
- Web 原型：`/Users/lizhen/Documents/vsproj/realliveapp/prototypes/server-web/index.html`（5195 行）

说明：本拆解仅依据 `index.html` 中的页面结构、注释分组、DOM 标识和内联 JS 函数，不依赖外部文档。

## 2. Mobile（index.html）拆解

## 2.1 页面分组（按原型注释）
- Flow A（认证）：A1 Splash、A2 Login、A3 Register
- Flow B（主入口）：B1 Dashboard、B2 Search、B3 Alerts
- Flow C（摄像头视图）：C1 Single Live Entry、C2 Camera List、C3 Live View
- Flow D（Live 动作）：D1 PTZ、D2 Snapshot Gallery、D3 Share Bottom Sheet
- Flow E（回放）：E1 History Playback、E2 Calendar、E3 Event Detail
- Flow F（设备管理）：F1 Scan、F2 Setup、F3 Camera Settings
- Flow G（设置子页）：G1 Settings、G2 Profile、G3 Storage、G4 Security
- Flow H（边界状态）：H1 Forgot Password、H2/H3/H4 设备接入状态、H5 删除确认、H6 搜索空态、H7 权限引导、H8 2FA、H9 升级套餐

## 2.2 Mobile 功能域拆解
1. Auth 域：登录注册重置密码 + 2FA
2. Hub 域：首页指标、搜索、通知流
3. Live 域：实时观看、PTZ、抓拍、分享
4. Playback 域：时间轴回看 + 事件详情
5. Device 域：扫码接入 + 配置与删除
6. Settings 域：账号、安全、存储
7. Edge 域：异常态/空态/引导态

## 2.3 Mobile 必实现状态机
- 设备接入：`Scanning -> Connecting -> Success | Failed`
- 安全设置：`2FA Disabled -> Setup -> Enabled`
- 搜索：`HasInput -> HasResult | EmptyResult`

## 3. Web（index.html）拆解

## 3.1 页面路由拆解（`id="page-*"`）
- `page-login`
- `page-dashboard`
- `page-monitor`
- `page-playback`
- `page-alerts`
- `page-devices`
- `page-storage`
- `page-settings`

## 3.2 Web 顶层交互模块（由函数聚类）
1. Shell & Routing
  - `showApp`、`switchPage`、`checkResponsive`、`toggleSidebar`、`collapseSidebar`
2. Global Feedback & Overlay
  - `showToast`、`open/closeQuickModal`、ESC 全局关闭逻辑
3. Dashboard Map Filter
  - `setMapFilter`、`setMapGroup`、`applyMapFilters`、`refreshMapGroupCounts`
4. Live Grid & Patrol
  - `setGrid`、拖拽相关事件、`togglePatrolRun`、`open/closePatrolModal`
5. Alert Drawer
  - `openAlertDrawer`、`closeAlertDrawer`
6. Rule Engine
  - `applyRuleTableView`、`validateRuleForm`、`saveRuleModal`
  - `sort/filter/pagination` 全套函数
  - 批量操作与确认：`batchSetRuleEnabled`、`batchDeleteRules`
  - Undo：`openRuleUndoBar`、`undoDeletedRules`
7. Device View
  - `switchDeviceView`
8. Playback Sidebar
  - `syncPlaybackCameraState`、相机搜索、全选/清空
9. Settings Tabs
  - `switchSettingsTab`

## 3.3 Web 功能域拆解
- Auth：登录
- Dashboard：指标、Quick Access、地图筛选、设备跳转
- Live：网格布局、拖拽布屏、聚焦模式、轮巡
- Playback：日期、相机选择、事件列表、分页行为
- Alerts：列表筛选、批量动作、详情抽屉
- Rules：筛选/排序/分页/编辑/批量/Undo
- Devices：表格/卡片双视图
- Storage：容量概览、趋势、策略入口
- Settings：Profile/Security/Users/Audit/Notifications/System 六分区

## 3.4 从代码中可确认的业务规则
1. 规则名长度：4-48
2. 规则名需唯一（前端内存层）
3. 条件长度 >= 12
4. 动作长度 >= 8
5. 高优先级规则：
  - 不能设置 quiet hours
  - 不能禁用
  - 不能设置 `After 180 seconds`
6. 批量删除后支持 Undo（6 秒）
7. ESC 会尝试关闭当前可关闭层（抽屉/弹层/Undo/QuickModal）

## 4. 组件拆解（Web）

## 4.1 布局组件
- `AppShell`（Sidebar + Topbar + Content）
- `PageContainer`

## 4.2 业务组件
- Dashboard：`StatsCards`、`QuickAccess`、`MapOverview`、`RecentAlertsPanel`
- Monitor：`GridToolbar`、`CameraBank`、`VideoGrid`、`PatrolStrip`、`PatrolModal`
- Alerts：`AlertTable`、`AlertDrawer`、`RuleBoard`、`RuleModal`、`RuleBatchConfirm`
- Playback：`CalendarPanel`、`CameraSelector`、`TimelinePanel`、`EventList`
- Devices：`DeviceToolbar`、`DeviceTable`、`DeviceCardGrid`
- Storage：`StorageOverview`、`TrendChart`、`PolicyPanel`
- Settings：`SettingsTabs` + 6 子面板

## 4.3 公共组件
- `Toast`
- `Modal`
- `Drawer`
- `ConfirmDialog`
- `Switch`
- `TablePagination`

## 5. AI 研发可执行拆解（先 Web）

## 5.1 第一批（壳层 + 核心）
1. 路由与 Shell（8 页）
2. 全局 Toast + Modal/Drawer 管理
3. Dashboard（含 QuickAccess，不含地图联动）
4. Live（2x2 基础布局 + 单元激活）

## 5.2 第二批（高交互域）
1. Dashboard 地图筛选联动
2. Alerts 列表 + Drawer
3. Rule Board（筛选/排序/分页）
4. Rule Modal + 校验
5. 批量操作 + Undo

## 5.3 第三批（治理域）
1. Playback 日期/相机选择/事件联动
2. Devices 双视图
3. Storage 概览与策略
4. Settings 六分区

## 6. 每个任务包的交付模板（给 AI）
1. 输入：页面ID/功能域 + 目标交互
2. 输出：
  - 变更文件清单
  - 状态模型定义
  - 组件结构
  - API 占位/接口契约
  - 自测步骤（含边界态）
3. 验收：
  - 交互与原型一致
  - 状态/空态/错误态完整
  - 不破坏现有页面切换与全局 ESC 行为

## 7. 立即可用的实现顺序（Web）
1. Shell + Routing
2. Dashboard
3. Live Grid + DnD
4. Alert Center + Rule Engine
5. Playback
6. Devices
7. Storage
8. Settings
