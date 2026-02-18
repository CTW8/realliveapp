# RealLive Web Console UI 设计说明（阶段化）

## 1. 文档目的
本文档用于指导 `prototypes/server-web/index.html` 的 Web 控制台原型设计与迭代，要求：
- 对齐 Android App 已有能力（实时监控、告警、回放、设备、存储、账户安全）
- 在桌面端做信息密度与多任务能力扩展
- 采用分阶段交付，便于按优先级推进

## 2. 总体设计原则
- 视觉继承：延续 Android 深色基底与紫粉强调色（`--bg` / `--pri` 等 token）
- 交互适配：移动端单任务流程，Web 端支持并行操作与批量管理
- 结构统一：固定导航 + 内容页容器，页面切换采用单文件静态原型
- 可扩展性：关键动作通过明确 hook 暴露（如 `data-toast`）

## 3. 阶段计划与当前状态

| 阶段 | 页面范围 | 当前状态 | 说明 |
|---|---|---|---|
| P0 | Login / Dashboard / Live Monitor | 已完成 | 完成入口、核心监控能力与首页概览 |
| P1 | Alert Center / Device Management | 已完成 | 完成告警表格+抽屉、设备表格/卡片双视图 |
| P2 | History Playback / Storage | 已完成 | 完成回放时间轴、多路播放、存储趋势与策略 |
| P3 | System Settings | 已完成（本次） | 从占位页升级为完整设置中心 |
| P4 | Dashboard / Alert Center / Live 扩展 | 进行中（第十二批已落地） | Dashboard 快捷入口完成可用化，规则自动化与交互安全能力持续完善 |

## 4. P3 设计目标（本次落地）

### 4.1 目标
在 Web 端补齐 Android `Settings + Profile + Security`，并扩展桌面管理能力：
- 用户与角色管理
- 审计日志
- 通知渠道策略
- 系统运行配置与维护入口

### 4.2 信息架构
`System Settings` 采用左右结构：
- 左侧：垂直 Tab 导航（Profile / Security / Users & Roles / Audit Logs / Notifications / System）
- 右侧：对应面板内容（`settings-panel-*`）

### 4.3 与 Android 功能映射
- Android `Profile` -> Web `Profile`（账户信息、语言、时区、偏好）
- Android `Security` -> Web `Security`（密码、2FA、会话、IP 白名单）
- Android 设置项扩展 -> Web `Users & Roles`（多账号协作场景）
- Android 无对应 -> Web `Audit Logs`（运维追溯）
- Android 通知开关扩展 -> Web `Notifications`（渠道与升级策略）
- Android 系统信息扩展 -> Web `System`（NVR 运行参数、维护操作、版本信息）

### 4.4 关键组件
已在 `index.html` 完成以下组件化区域：
- `settings-layout`：设置页主布局
- `settings-tabs` / `stab`：左侧分区导航
- `settings-panel`：右侧可切换内容面板
- `set-card`：设置模块卡片
- `user-table` / `audit-row` / `notif-channel`：三类高频管理列表
- `web-toast`：统一轻反馈提示

### 4.5 关键交互
- Tab 切换：`switchSettingsTab(tabId, tabEl)`
- 操作反馈：按钮使用 `data-toast`，统一触发 toast
- 开关交互：复用 `sw-web`（on/off）

### 4.6 响应式适配
- `<=1023px`：设置页改为单列，Tab 变网格
- `<=768px`：表格列进一步收敛，优先保留核心信息与动作

## 5. 当前 Web 原型覆盖清单
`prototypes/server-web/index.html` 已覆盖：
- Login
- Dashboard
- Live Monitor
- History Playback
- Alert Center（含详情抽屉）
- Device Management（表格/卡片）
- Storage Management
- System Settings（6 分区）

## 6. 手动验收建议
1. 页面切换：侧边导航切换 8 个页面是否正确更新标题与内容。
2. Settings Tab：6 个分区切换是否只展示一个面板。
3. 操作反馈：点击含 `data-toast` 的按钮是否出现 toast。
4. 响应式：在 1440 / 1024 / 768 宽度下检查设置页布局是否可读。
5. 不回归：确认 P0-P2 页面交互（告警抽屉、设备视图切换、回放选择等）未受影响。

## 7. P4 已落地内容（第十二批）
- Dashboard 新增 `Site Map Overview`：地图标记、热点区域、一键跳转到 Live Monitor
- Dashboard 地图新增筛选：按状态/区域过滤，联动地图标记与热点列表
- Dashboard 地图新增分组联动：`All / Entry / Perimeter / Critical` 与状态/区域筛选可叠加
- Dashboard 地图分组卡片新增动态计数：随状态/区域筛选实时更新可见设备数
- Alert Center 新增 `Alert Rule Automation`：规则列表、优先级、静默时段、启停开关
- 规则编辑弹层已支持回写联动：编辑/新建后可更新规则列表（原型内联动）
- 规则编辑弹层新增表单校验：名称长度/重名检查、条件与动作最小长度、高优先级约束
- 规则编辑弹层接入 `Escalation Delay` 参数：支持编辑回填、保存回写与高优先级延迟约束
- 规则自动化列表新增 `Escalation` 列，并支持按优先级筛选、按优先级/名称/升级延迟排序
- 规则自动化列表新增多条件过滤：优先级 + 启用状态 + 升级延迟 + 名称关键字（支持一键重置）
- 规则启停开关与列表筛选联动：切换状态后即时刷新当前过滤结果
- 规则自动化列表新增分页视图：筛选结果按页展示并支持页码跳转
- 规则自动化列表新增页级批量操作：`Enable Page / Disable Page / Delete Page`
- 规则自动化表头支持点击排序：`Rule / Priority / Escalation` 与排序下拉双向联动
- 批量操作新增确认弹层（防误操作）：执行启用/停用/删除前二次确认
- 批量操作新增作用域切换：支持对“当前页”或“当前筛选结果全部”执行启用/停用/删除
- 批量删除新增 `Undo` 撤销条：删除后可在短时间内恢复规则（原型内联动）
- Login 页面布局优化：将品牌介绍区左移并拉开与登录卡片的视觉间距
- Dashboard `Quick Access` 完成可用化：四个快捷卡片均具备真实动作（跳转/布局预设/导出流程）
- 新增 Quick Export 弹层：支持时间范围与导出格式选择，并在原型内反馈任务提交
- Live Monitor 新增 `Patrol Schedule` 条带：当前轮巡组、运行状态、段位切换可视化
- 新增 `Patrol Planner` 配置弹层：组别、停留时长、时间窗、启停控制
- Live Monitor 新增拖拽布屏：可将摄像头标签拖入宫格空位完成分配
- History Playback `Date & Cameras` 侧栏重构：日期与摄像头分区卡片化，新增选中计数、全选/清空与摄像头搜索，移动端比例同步优化

## 8. 下一阶段建议（P4 后续）
- 将规则编辑弹层与后端规则引擎参数对齐（字段 schema、错误码、服务端校验返回）
- Live Monitor 接入真实视频轮巡切换（当前为静态占位流）
- Dashboard 地图接入真实设备拓扑与分组配置（当前为原型静态数据）
- 将设置页关键表单接入真实配置数据模型
