# RealLive 智能监控平台 PRD（详细版）

## 1. 文档信息
- 产品名称：RealLive 智能监控平台
- 文档类型：PRD（详细版）
- 文档版本：v2.0
- 更新时间：2026-02-17
- 适用范围：Android App + Web Console
- 文档状态：评审中

## 2. 版本记录
| 版本 | 日期 | 变更内容 | 负责人 |
|---|---|---|---|
| v1.0 | 2026-02-17 | 基于 UI 原型的初版 PRD | 产品 |
| v2.0 | 2026-02-17 | 升级为详细版：需求编号、业务规则、数据模型、权限矩阵、UAT、里程碑 | 产品 |

## 3. 设计与原型依据
- 移动端设计文档：
  - `prototypes/mobile-app/UI_DESIGN.md`
  - `prototypes/mobile-app/UI_DESIGN_REVIEW.md`
  - `prototypes/mobile-app/UI_ONE_PAGER.md`
- Web 端设计文档：
  - `prototypes/server-web/UI_DESIGN.md`
- 原型文件：
  - `prototypes/mobile-app/index.html`
  - `prototypes/server-web/index.html`

## 4. 背景与问题定义
当前监控产品常见问题：
1. 监控、告警、回放、设备管理链路割裂，用户需要在多个页面反复跳转。
2. 告警处理缺乏规则化与优先级约束，误报多、漏报风险高。
3. 设备接入与运维能力不统一，移动端适合处置但不适合批量管理。
4. 缺少审计与角色管理，难以支撑团队协作和运维追溯。

RealLive 目标是以“移动端快速处置 + Web 端集中管控”的双端协同方案，打通从告警触发到处置闭环的全流程。

## 5. 产品目标与成功指标

### 5.1 产品目标
1. 缩短从“异常发现”到“确认处置”的路径和时长。
2. 提供统一状态语义与一致交互，降低学习成本和误操作。
3. 支持从个人用户到小型团队的权限管理与运维治理。

### 5.2 关键成功指标（上线后 3 个月）
| 指标 | 定义 | 目标值 |
|---|---|---|
| TTV | 用户登录后到首次可操作视频画面的时间 | <= 5 秒 |
| MTTA | 告警触发到用户首次处理动作的中位时长 | <= 2 分钟 |
| MTTR | 告警触发到告警关闭的中位时长 | <= 10 分钟 |
| 设备接入成功率 | 设备配网流程成功完成占比 | >= 95% |
| 高优先级漏通知率 | 高优先级事件未触达责任人占比 | <= 1% |
| 规则配置回退率 | 新建/编辑规则后 24h 内被回退占比 | <= 5% |

## 6. 用户角色与权限边界

### 6.1 角色定义
| 角色 | 主要诉求 | 典型终端 |
|---|---|---|
| 家庭用户 | 快速查看实时状态与异常告警 | 移动端 |
| 场地管理员 | 多设备运维、规则策略配置、事件导出 | Web 端 |
| 值班操作员 | 高频监看、告警确认、回放核查 | Web 端 |
| 审计/安全管理员 | 用户、权限、审计、会话安全 | Web 端 |

### 6.2 角色能力矩阵（目标态）
| 能力 | Admin | Operator | Viewer |
|---|---|---|---|
| 实时监控查看 | Y | Y | Y |
| PTZ/抓拍/分享操作 | Y | Y | N |
| 告警确认/归档 | Y | Y | N |
| 告警规则配置 | Y | Y（受限） | N |
| 设备新增/删除 | Y | Y（受限） | N |
| 存储策略修改 | Y | N | N |
| 用户与角色管理 | Y | N | N |
| 审计日志导出 | Y | N | N |
| 系统参数修改 | Y | N | N |

## 7. 范围定义

### 7.1 本期范围（In Scope）
1. Android：认证、首页、搜索、通知、实时查看、PTZ、抓拍、分享、历史回看、设备接入、设置与安全。
2. Web：登录、Dashboard、Live Monitor、Playback、Alert Center（含规则自动化）、Devices、Storage、System Settings。
3. 全局能力：筛选、排序、分页、抽屉、弹层、Toast、批量操作、撤销保护、响应式布局。

### 7.2 非本期范围（Out of Scope）
1. iOS/iPad 专属 UI 与交互规范。
2. 真实视频协议栈实现（RTSP/GB28181/WebRTC 底层）。
3. 支付结算闭环（保留升级入口，不含支付收银台）。
4. 对外工单、IM、第三方 IAM 深度集成。

## 8. 信息架构

### 8.1 Android 页面结构（按原型编号）
| 分组 | 页面 |
|---|---|
| A 认证 | A1 Splash, A2 Login, A3 Register |
| B 主入口 | B1 Dashboard, B2 Search, B3 Alerts |
| C 摄像头 | C1 Single Live Entry, C2 Camera List, C3 Live View |
| D Live 子页 | D1 PTZ, D2 Snapshot Gallery, D3 Share Bottom Sheet |
| E 回看 | E1 History Playback, E2 Calendar Picker, E3 Event Detail |
| F 设备管理 | F1 Add Camera Scan, F2 Add Camera Setup, F3 Camera Settings |
| G 账户设置 | G1 Settings, G2 Profile, G3 Storage, G4 Security |
| H 边界场景 | H1 Forgot Password, H2 Connecting, H3 Success, H4 Failed, H5 Delete Confirm, H6 Search Empty, H7 Permissions, H8 2FA, H9 Upgrade |

### 8.2 Web 页面结构
| 页面ID | 页面名称 | 核心目标 |
|---|---|---|
| page-login | 登录页 | 进入系统 |
| page-dashboard | 总览看板 | 快速态势感知 + 快捷入口 |
| page-monitor | 实时监控 | 多路监看与布屏操作 |
| page-playback | 历史回放 | 事件追溯与片段核查 |
| page-alerts | 告警中心 | 告警处理 + 规则自动化 |
| page-devices | 设备管理 | 设备列表、筛选、批量运维 |
| page-storage | 存储管理 | 容量、趋势、策略 |
| page-settings | 系统设置 | 安全、用户、审计、通知、系统参数 |

## 9. 核心业务流程

### 9.1 告警处置闭环
1. 用户登录进入 Dashboard 查看在线率与告警概览。
2. 在 Alert Center 收到高优先级告警并打开详情。
3. 跳转 Live/Playback 核查现场与历史片段。
4. 执行处置动作（确认、归档、转派、策略调整）。
5. 在 Settings 中更新安全策略并审计留痕。

### 9.2 设备接入闭环
1. 扫码识别设备与序列号。
2. 配置网络并进入连接中状态（进度可视化）。
3. 成功则进入 Live View；失败则展示原因与重试入口。
4. 在 Camera Settings 完成名称/位置配置。

### 9.3 规则治理闭环（Web）
1. 在 Alert Center 创建/编辑规则。
2. 系统进行表单规则校验（名称、唯一性、优先级约束等）。
3. 规则生效后可通过筛选、排序、分页与批量操作治理。
4. 批量删除后提供短时 Undo，降低误操作风险。

## 10. 详细功能需求

> 说明：以下按模块给出用户故事、详细需求、业务规则、异常处理与验收标准。

### 10.1 认证与账号（AUTH）

#### 10.1.1 用户故事
- 作为用户，我希望快速登录并进入监控主页。
- 作为忘记密码的用户，我希望能通过邮箱验证码重置密码。
- 作为安全敏感用户，我希望开启 2FA 提升账户安全。

#### 10.1.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| AUTH-001 | 支持邮箱+密码登录，含“记住我” | P0 | Android/Web |
| AUTH-002 | 提供忘记密码流程（邮箱验证码 + 新密码） | P1 | Android |
| AUTH-003 | 支持 2FA 开启（Authenticator/SMS/Email） | P1 | Android/Web |
| AUTH-004 | 展示活跃会话管理与“登出其他设备” | P1 | Web |

#### 10.1.3 关键规则
1. 密码最小长度 8，需包含数字与字母（目标态可扩展复杂度策略）。
2. 重置密码验证码 6 位数字，默认 10 分钟有效。
3. 2FA 开启后，异常设备登录需二次验证。

#### 10.1.4 验收标准
1. 正确凭据登录成功并进入默认首页。
2. 错误凭据给出可理解错误提示，不泄露账号是否存在。
3. 忘记密码可完成“发码-校验-重置”完整链路。
4. 2FA 成功开启后，账户安全状态显示为已增强。

### 10.2 Dashboard 总览（DASH）

#### 10.2.1 用户故事
- 作为值班用户，我希望在一个页面看到系统总体健康度和告警态势。
- 作为管理员，我希望通过快捷入口直达高频工作页。

#### 10.2.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| DASH-001 | 展示在线/离线/告警/设备总量核心指标卡 | P0 | Android/Web |
| DASH-002 | 展示实时预览卡片与设备状态 | P0 | Android/Web |
| DASH-003 | 提供 Quick Access（Monitor/Playback/Add Device/Export） | P1 | Web |
| DASH-004 | 提供地图概览与状态/区域/分组筛选 | P1 | Web |

#### 10.2.3 交互规则
1. 指标卡点击可跳转对应过滤视图。
2. Web Quick Access“Monitor”默认加载 2x2 布局。
3. 地图筛选支持三维组合：状态 + 区域 + 分组。
4. 分组统计计数需随筛选实时变化。

#### 10.2.4 验收标准
1. 四类指标卡文案、颜色、数量与状态一致。
2. 快捷入口可跳转并触发反馈提示。
3. 地图筛选后，标记与列表显示一致。

### 10.3 实时监控（LIVE）

#### 10.3.1 用户故事
- 作为 Web 操作员，我需要同时查看多路视频并快速聚焦单路。
- 作为移动端用户，我需要快速进入单路直播并切换摄像头，不进行多路同屏播放。
- 作为用户，我希望在实时画面中执行抓拍、PTZ、分享。

#### 10.3.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| LIVE-001 | 支持 1x1/2x2/3x3/4x4 视频网格布局 | P0 | Web |
| LIVE-002 | 支持拖拽摄像头到宫格进行布屏 | P1 | Web |
| LIVE-003 | 双击单元格进入 1x1 聚焦，ESC 退出聚焦 | P1 | Web |
| LIVE-004 | 支持 PTZ 控制页（方向+变焦） | P0 | Android/Web |
| LIVE-005 | 支持抓拍、分享、全屏等操作按钮 | P0 | Android/Web |
| LIVE-006 | 支持轮巡计划条带与启停控制 | P2 | Web |

#### 10.3.3 状态与展示规则
1. 在线：绿点 + Online/LIVE 文案。
2. 录制：橙点 + REC 文案 + 动态状态。
3. 离线：红点 + OFFLINE 覆盖层 + 重连入口。
4. 时间戳展示：在线显示当前时间，离线显示最后在线时间。

#### 10.3.4 验收标准
1. 切换布局后网格单元数量正确。
2. 拖拽分配后单元标题与状态正确更新。
3. 离线流不能执行 PTZ，界面需明确禁用。
4. ESC 能关闭聚焦与所有打开弹层（规则弹窗/抽屉/轮巡弹层等）。

### 10.4 告警中心（ALERT）

#### 10.4.1 用户故事
- 作为值班操作员，我需要按类型、时间快速筛选告警并处理。
- 作为管理员，我需要批量标记、归档、删除告警。

#### 10.4.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| ALERT-001 | 告警列表展示类型、摘要、设备、时间、状态 | P0 | Android/Web |
| ALERT-002 | 支持类型筛选、时间范围筛选 | P0 | Web |
| ALERT-003 | 支持批量已读、批量归档、批量删除 | P1 | Web |
| ALERT-004 | 点击告警打开详情抽屉，不离开当前页 | P1 | Web |
| ALERT-005 | 支持导出告警数据 | P2 | Web |

#### 10.4.3 验收标准
1. 未读告警需有视觉高亮区分。
2. 批量选择后，操作按钮状态可用。
3. 告警详情抽屉可打开/关闭，ESC 可关闭。

### 10.5 告警规则自动化（RULE）

#### 10.5.1 用户故事
- 作为管理员，我希望用规则自动触发通知与升级，减少人工盯盘。
- 作为操作员，我希望快速过滤和批量管理规则。

#### 10.5.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| RULE-001 | 支持新建/编辑规则弹层 | P1 | Web |
| RULE-002 | 支持按优先级/状态/升级延迟/关键字筛选 | P1 | Web |
| RULE-003 | 支持按 Rule/Priority/Escalation 排序 | P1 | Web |
| RULE-004 | 支持分页浏览与页码切换 | P1 | Web |
| RULE-005 | 支持批量启用/禁用/删除（作用域：当前页或筛选结果） | P1 | Web |
| RULE-006 | 批量删除后支持 Undo 撤销 | P1 | Web |

#### 10.5.3 强约束业务规则
1. 规则名称长度：4-48 字符。
2. 规则名称唯一（同租户内，不区分大小写）。
3. 触发条件最小长度：12 字符。
4. 动作描述最小长度：8 字符。
5. 高优先级规则禁止设置静默时段（Quiet Hours 必须 Disabled）。
6. 高优先级规则必须保持启用状态。
7. 高优先级规则升级延迟不能为 `After 180 seconds`。
8. 批量删除需二次确认。
9. Undo 窗口默认 6 秒，超时后不可恢复。

#### 10.5.4 验收标准
1. 违规输入时阻止保存并显示明确错误文案。
2. 筛选、排序、分页联动后统计数字正确。
3. 批量操作范围切换后目标集合正确。
4. 删除后在 Undo 窗口可恢复且数据一致。

### 10.6 历史回放（PLAYBACK）

#### 10.6.1 用户故事
- 作为用户，我希望通过日期+时间轴快速定位事件片段。
- 作为值班人员，我希望一次选择多路摄像头对比回放。

#### 10.6.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| PLAY-001 | 支持日期选择与事件标记日历 | P1 | Android/Web |
| PLAY-002 | 支持时间轴拖动与事件跳点 | P1 | Android/Web |
| PLAY-003 | 支持多摄像头勾选、全选/清空、搜索 | P1 | Web |
| PLAY-004 | 支持事件列表与详情查看 | P1 | Android/Web |
| PLAY-005 | 支持片段抓拍/导出入口 | P2 | Web |

#### 10.6.3 交互规则
1. 摄像头选择数量需实时反馈（已选计数）。
2. “全选/清空”按钮文案随状态动态切换。
3. 搜索过滤仅影响列表展示，不自动改变已选集合。

#### 10.6.4 验收标准
1. 点击日历日期后，当前选中态唯一。
2. 事件列表点击后时间轴与播放器位置联动。
3. 摄像头搜索可过滤名称，输入清空后恢复。

### 10.7 设备管理与接入（DEVICE）

#### 10.7.1 用户故事
- 作为管理员，我希望快速新增设备并看到成功/失败反馈。
- 作为操作员，我希望在表格和卡片视图间切换管理设备。

#### 10.7.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| DEV-001 | 支持扫码新增设备流程（扫描-连接-结果） | P1 | Android |
| DEV-002 | 支持设备列表状态筛选（在线/离线/录制） | P1 | Android/Web |
| DEV-003 | 支持设备表格/卡片双视图切换 | P1 | Web |
| DEV-004 | 支持设备配置入口（名称、位置、网络） | P1 | Android/Web |
| DEV-005 | 删除设备需二次确认弹层 | P0 | Android/Web |

#### 10.7.3 设备接入状态机
| 状态 | 说明 | 用户可操作 |
|---|---|---|
| SCANNING | 扫码识别设备信息 | 取消 |
| CONNECTING | 配网与绑定进行中 | 取消 |
| SUCCESS | 绑定成功，可进入 Live | 返回列表、打开 Live |
| FAILED | 绑定失败，显示原因 | 重试、查看排障 |

#### 10.7.4 验收标准
1. 连接中状态需展示步骤与进度。
2. 失败态必须给出可执行建议（频段/密码/超时）。
3. 删除确认文案需明确影响范围（不删除云端历史数据）。

### 10.8 存储管理（STORAGE）

#### 10.8.1 用户故事
- 作为管理员，我需要知道容量结构和趋势，避免存储溢出。
- 作为用户，我希望快速看到当前套餐与升级入口。

#### 10.8.2 需求列表
| 编号 | 需求 | 优先级 | 平台 |
|---|---|---|---|
| STO-001 | 展示总容量、已用、可用与分类占比 | P1 | Android/Web |
| STO-002 | 展示按设备的存储占用明细 | P2 | Android/Web |
| STO-003 | 展示趋势（近7/30天）与预测提示 | P2 | Web |
| STO-004 | 支持保留策略与清理入口 | P2 | Web |
| STO-005 | 提供升级套餐入口 | P2 | Android/Web |

#### 10.8.3 验收标准
1. 圆环/柱状/明细数据一致。
2. 超阈值（如 80%、90%）需触发预警样式。
3. 升级入口可进入套餐选择页。

### 10.9 设置中心（SETTINGS）

#### 10.9.1 模块结构（Web）
| Tab | 关键能力 |
|---|---|
| Profile | 账户信息、语言、时区、个人偏好 |
| Security | 密码、2FA、会话管理、IP 白名单 |
| Users & Roles | 用户邀请、角色分配、权限矩阵 |
| Audit Logs | 操作日志查询与导出 |
| Notifications | 渠道开关、测试发送、升级策略 |
| System | 运行配置、NTP、升级、清理、版本信息 |

#### 10.9.2 需求列表
| 编号 | 需求 | 优先级 |
|---|---|---|
| SET-001 | Settings 左侧 Tab 切换，右侧单面板显示 | P1 |
| SET-002 | Security 提供会话撤销与“登出其他设备” | P1 |
| SET-003 | Users & Roles 支持邀请用户与角色标签管理 | P2 |
| SET-004 | Audit Logs 支持条件筛选与导出 CSV | P2 |
| SET-005 | Notifications 支持渠道测试（Email/SMS/Webhook） | P2 |
| SET-006 | System 提供配置保存、NTP 同步、维护操作 | P2 |

#### 10.9.3 验收标准
1. 任意时刻仅一个 Settings 面板处于激活态。
2. 安全与系统敏感操作需二次确认或明确反馈。
3. 所有保存/测试操作都应反馈结果。

### 10.10 全局交互与反馈（GLOBAL）
| 编号 | 需求 | 规则 |
|---|---|---|
| GLB-001 | Toast 轻反馈 | 默认展示约 1.7s，覆盖成功/失败提示 |
| GLB-002 | 模态弹层 | 点击遮罩或 ESC 可关闭（危险操作除外） |
| GLB-003 | 空态处理 | 必须给出下一步建议，不允许纯空白 |
| GLB-004 | 危险动作保护 | 删除/批量停用/会话撤销需要确认 |
| GLB-005 | 状态语义一致 | 颜色 + 文案双通道表达 |

## 11. 业务规则汇总（BR）
| 编号 | 规则 |
|---|---|
| BR-001 | 在线/录制/离线状态统一为绿/橙/红并配套文案 |
| BR-002 | 高优先级规则必须启用，且不可静默 |
| BR-003 | 高优先级规则升级延迟不得超过 60 秒 |
| BR-004 | 规则名称租户内唯一，不区分大小写 |
| BR-005 | 批量删除必须二次确认并提供短时撤销 |
| BR-006 | 设备删除必须提示“仅解绑设备，不删除历史云数据” |
| BR-007 | 安全设置变更必须记录审计日志 |
| BR-008 | Viewer 角色不可执行控制类操作（PTZ/删除/配置） |
| BR-009 | 时间展示遵循用户时区，24h/12h 格式按偏好 |
| BR-010 | 响应式下隐藏次要列时不得隐藏关键动作 |

## 12. 数据模型（逻辑定义）

### 12.1 User
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| user_id | string | Y | 用户唯一标识 |
| email | string | Y | 登录账号 |
| display_name | string | Y | 显示名称 |
| role | enum(admin/operator/viewer) | Y | 角色 |
| status | enum(active/disabled) | Y | 用户状态 |
| last_login_at | datetime | N | 最近登录时间 |
| two_factor_enabled | bool | Y | 是否开启 2FA |

### 12.2 Device
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| device_id | string | Y | 设备ID |
| name | string | Y | 设备名称 |
| location | string | N | 部署位置 |
| status | enum(online/recording/offline) | Y | 当前状态 |
| firmware_version | string | N | 固件版本 |
| stream_profile | string | N | 码流档位 |
| last_seen_at | datetime | N | 最后在线时间 |

### 12.3 AlertEvent
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| alert_id | string | Y | 告警ID |
| type | enum(motion/alarm/offline/system) | Y | 告警类型 |
| severity | enum(low/medium/high) | Y | 严重级别 |
| device_id | string | N | 关联设备 |
| title | string | Y | 标题 |
| content | string | Y | 摘要 |
| status | enum(new/read/resolved/archived) | Y | 状态 |
| occurred_at | datetime | Y | 发生时间 |
| resolved_at | datetime | N | 解决时间 |

### 12.4 AlertRule
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| rule_id | string | Y | 规则ID |
| name | string | Y | 规则名称 |
| type | string | Y | 规则类型 |
| condition | string | Y | 触发条件 |
| action | string | Y | 处置动作 |
| priority | enum(low/medium/high) | Y | 优先级 |
| quiet_hours | string | Y | 静默时段，`Disabled` 表示关闭 |
| escalation_delay | enum(immediately/60s/180s) | Y | 升级延迟 |
| enabled | bool | Y | 启停状态 |
| updated_at | datetime | Y | 更新时间 |

### 12.5 PlaybackClip
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| clip_id | string | Y | 片段ID |
| device_id | string | Y | 设备ID |
| date | date | Y | 日期 |
| start_at | datetime | Y | 起始时间 |
| end_at | datetime | Y | 结束时间 |
| event_tags | string[] | N | 事件标签 |
| downloadable | bool | Y | 是否可下载 |

### 12.6 AuditLog
| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| log_id | string | Y | 日志ID |
| actor_user_id | string | Y | 操作人 |
| action_type | string | Y | 操作类型 |
| action_desc | string | Y | 操作描述 |
| target_type | string | N | 目标类型 |
| target_id | string | N | 目标ID |
| created_at | datetime | Y | 发生时间 |
| ip | string | N | 来源IP |

## 13. 接口草案（用于研发对齐）

> 仅定义接口边界，最终以后端 API 文档为准。

| 模块 | Method | Path | 说明 |
|---|---|---|---|
| Auth | POST | `/api/v1/auth/login` | 登录 |
| Auth | POST | `/api/v1/auth/forgot-password/send-code` | 发送重置码 |
| Auth | POST | `/api/v1/auth/forgot-password/reset` | 重置密码 |
| Dashboard | GET | `/api/v1/dashboard/overview` | 首页概览数据 |
| Device | GET | `/api/v1/devices` | 设备列表 |
| Device | POST | `/api/v1/devices/onboard` | 设备接入 |
| Device | PATCH | `/api/v1/devices/{id}` | 更新设备配置 |
| Alert | GET | `/api/v1/alerts` | 告警列表 |
| Alert | PATCH | `/api/v1/alerts/batch` | 告警批量操作 |
| Rule | GET | `/api/v1/alert-rules` | 规则列表 |
| Rule | POST | `/api/v1/alert-rules` | 新建规则 |
| Rule | PATCH | `/api/v1/alert-rules/{id}` | 更新规则 |
| Rule | DELETE | `/api/v1/alert-rules/{id}` | 删除规则 |
| Playback | GET | `/api/v1/playback/search` | 回放查询 |
| Storage | GET | `/api/v1/storage/overview` | 存储概览 |
| Settings | GET | `/api/v1/settings/profile` | 设置读取 |
| Settings | PATCH | `/api/v1/settings/profile` | 设置保存 |
| Audit | GET | `/api/v1/audit-logs` | 审计日志查询 |

## 14. 非功能需求（NFR）

### 14.1 性能
| 项目 | 指标 |
|---|---|
| 首屏可交互时间 | <= 2.5s（Web 主页面） |
| 页面切换反馈 | <= 300ms |
| 列表筛选/排序前端反馈 | <= 200ms（1000 条本地模拟） |
| 回放检索响应 | P95 <= 2s（服务端） |

### 14.2 可用性
| 项目 | 指标 |
|---|---|
| 服务可用性 | >= 99.9%（月） |
| 告警投递成功率 | >= 99.5% |
| 关键动作失败重试 | 支持自动或手动重试机制 |

### 14.3 安全
1. 全链路 HTTPS。
2. 会话过期与刷新策略。
3. 关键操作（删除、权限变更）审计留痕。
4. 密码与 2FA 安全策略可配置。

### 14.4 响应式与兼容
| 终端 | 要求 |
|---|---|
| Web | 1440 / 1024 / 768 三档主断点可用 |
| Mobile | 375x812 基线，不遮挡状态栏安全区 |
| 浏览器 | Chrome 最新两个主版本优先 |

### 14.5 可观测性
1. 前端错误采集（JS Error、Promise Rejection、性能指标）。
2. 后端链路追踪（登录、告警、规则保存、导出任务）。
3. 关键事件埋点覆盖核心漏斗。

## 15. 埋点与指标口径

### 15.1 关键事件
| 事件名 | 触发时机 | 关键属性 |
|---|---|---|
| `login_success` | 登录成功 | role, platform |
| `dashboard_quick_action_click` | 快捷入口点击 | action_type |
| `live_grid_change` | 切换布局 | from_grid, to_grid |
| `rule_save` | 规则保存成功 | priority, enabled |
| `rule_save_failed` | 规则保存失败 | error_code |
| `rule_batch_action` | 批量规则操作 | action, scope, count |
| `rule_undo` | 撤销删除 | restored_count |
| `alert_open_detail` | 打开告警详情 | alert_type, severity |
| `playback_event_jump` | 回放事件跳转 | camera_count |
| `device_onboard_result` | 设备接入结果 | result, fail_reason |
| `security_2fa_enabled` | 2FA 开启成功 | method |

### 15.2 核心漏斗
1. 登录成功 -> 进入 Dashboard -> 打开告警详情 -> 打开回放 -> 告警关闭。
2. 设备扫码 -> 配网中 -> 接入成功 -> 首次 Live 打开。
3. 打开规则弹窗 -> 保存成功 -> 24 小时内无回退。

## 16. 验收测试（UAT）

### 16.1 核心场景用例
| 用例ID | 场景 | Given | When | Then |
|---|---|---|---|---|
| UAT-001 | 登录成功 | 用户凭据正确 | 点击登录 | 进入 Dashboard |
| UAT-002 | 登录失败 | 密码错误 | 点击登录 | 错误提示且不跳转 |
| UAT-003 | 快捷入口跳转（Web） | 在 Dashboard | 点击 Monitor | 跳转 Live 且布局为 2x2 |
| UAT-004 | 地图筛选联动 | Dashboard 地图已加载 | 选择 Offline + Building A | 仅显示匹配设备与标记 |
| UAT-005 | Live 聚焦 | 2x2 网格 | 双击某单元 | 进入 1x1 聚焦 |
| UAT-006 | Live 退出聚焦 | 1x1 聚焦中 | 按 ESC | 恢复 2x2 |
| UAT-007 | 规则校验失败 | 创建高优先级规则 | 设置 Quiet Hours 非 Disabled | 保存失败并提示 |
| UAT-008 | 规则批量删除 | 选中若干规则 | 执行 Delete | 弹出二次确认 |
| UAT-009 | Undo 恢复 | 删除规则后 6 秒内 | 点击 Undo | 规则恢复成功 |
| UAT-010 | 告警详情抽屉 | 告警列表可见 | 点击一条告警 | 抽屉打开显示详情 |
| UAT-011 | 回放日期切换 | Playback 页面 | 点击新日期 | 日期高亮切换且内容更新 |
| UAT-012 | 回放相机全选 | Playback 页面 | 点击 Select all | 全部勾选并更新计数 |
| UAT-013 | 设备接入成功 | 扫码成功 | 完成配网 | 出现 Success 并可进入 Live |
| UAT-014 | 设备接入失败 | 配网超时 | 进入失败态 | 显示原因与重试入口 |
| UAT-015 | 删除设备确认 | 设备设置页 | 点击删除 | 弹出确认并显示影响说明 |
| UAT-016 | 设置 Tab 切换 | Settings 页面 | 切换到 Security | 仅 Security 面板显示 |
| UAT-017 | 通知渠道测试 | Notifications 面板 | 点击 SMS Test | 显示测试反馈 |
| UAT-018 | 审计导出 | Audit 页面 | 点击 Export CSV | 显示导出任务已创建 |
| UAT-019 | 响应式适配 1024 | Web 宽度 1024 | 浏览主要页面 | 布局可读且可操作 |
| UAT-020 | 响应式适配 768 | Web 宽度 768 | 浏览主要页面 | 主功能可用，无关键按钮丢失 |

### 16.2 回归重点
1. 页面路由与标题联动。
2. 规则筛选/排序/分页联动。
3. ESC 全局关闭行为。
4. Toast 触发链路（`data-toast`）。
5. 危险动作确认弹层。

## 17. 发布计划与里程碑
| 里程碑 | 时间 | 范围 | 出口标准 |
|---|---|---|---|
| M1（P0） | Week 1-2 | 登录、Dashboard、Live 核心查看与操作 | 核心链路可演示，P0 用例通过 |
| M2（P1） | Week 3-5 | Alert Center、Rule Automation、Playback、设备接入 | 规则校验闭环完成，UAT-001~015 通过 |
| M3（P2） | Week 6-8 | Storage、Settings 全量、审计与角色 | 管理能力完善，UAT 全量通过 |
| M4（Hardening） | Week 9 | 性能、安全、稳定性优化 | NFR 指标达标，上线评审通过 |

## 18. 依赖与风险

### 18.1 关键依赖
1. 后端规则引擎字段与校验一致性。
2. 告警实时推送链路（消息中间件/通知网关）。
3. 回放索引与分片服务性能。
4. 权限系统与审计系统基础能力。

### 18.2 风险与缓解
| 风险 | 影响 | 缓解措施 |
|---|---|---|
| 规则配置复杂导致误配 | 告警质量下降 | 模板化规则、强校验、预览说明 |
| 批量操作误触 | 数据误删 | 二次确认 + Undo + 审计 |
| 中小屏信息密度过高 | 可读性下降 | 响应式收敛列、保留关键动作 |
| 回放性能不稳定 | 处置效率下降 | 预索引、缓存、异步加载 |
| 通知渠道失败 | 漏告警 | 多通道兜底、失败重试、告警监控 |

## 19. 待确认问题（Open Questions）
1. 规则优先级与升级延迟是否支持租户级自定义上限。
2. Playback 多路回放最大并发路数（建议 4 路起步）。
3. Operator 是否允许创建高优先级规则（建议默认否）。
4. 审计日志保存周期（建议 180 天起）。
5. 导出任务文件保留时长（建议 7 天）。

## 20. 交付清单
1. 详细 PRD（本文档）。
2. API 字段对齐文档（后续）。
3. 设计走查清单（状态、空态、异常态、响应式）。
4. QA 用例库（基于 UAT-001~020 扩展自动化）。

---

## 附录 A：状态枚举规范
| 域 | 枚举值 |
|---|---|
| 设备状态 | `online`, `recording`, `offline` |
| 告警状态 | `new`, `read`, `resolved`, `archived` |
| 规则优先级 | `low`, `medium`, `high` |
| 规则升级延迟 | `immediately`, `60s`, `180s` |
| 用户角色 | `admin`, `operator`, `viewer` |

## 附录 B：原型到需求映射（摘要）
| 原型页面 | 对应需求模块 |
|---|---|
| Android A1-A3 | AUTH |
| Android B1-B3 | DASH + ALERT（移动） |
| Android C1-D3 | LIVE |
| Android E1-E3 | PLAYBACK |
| Android F1-F3 | DEVICE |
| Android G1-G4 | SETTINGS + STORAGE + SECURITY |
| Android H1-H9 | EDGE CASE + SECURITY + BILLING ENTRY |
| Web page-dashboard | DASH |
| Web page-monitor | LIVE |
| Web page-playback | PLAYBACK |
| Web page-alerts | ALERT + RULE |
| Web page-devices | DEVICE |
| Web page-storage | STORAGE |
| Web page-settings | SETTINGS + SECURITY + IAM + AUDIT |
