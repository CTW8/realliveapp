# RealLive 研发拆解（按移动端 / Web 端分开）

## 1. 文档信息
- 对应 PRD：`/Users/lizhen/Documents/vsproj/realliveapp/PRD.md`
- 文档版本：v2.0
- 日期：2026-02-17
- 目标：将需求拆解为移动端与 Web 端两套独立 Jira Backlog，并补充共享后端任务

## 2. 拆解原则
1. 移动端与 Web 端分别建 Epic，避免交付边界不清。
2. 同一业务能力在两端出现时，拆为两个 Story，分别验收。
3. 后端与平台能力单独建“共享 Epic”，由两端 Story 依赖。
4. 所有 Story 均要求 FE/BE/QA 子任务。

## 3. Jira 命名规范（建议）

### 3.1 Epic 命名
- 移动端：`[M-Epic] <能力域>`
- Web 端：`[W-Epic] <能力域>`
- 共享：`[S-Epic] <能力域>`

### 3.2 Issue Key 前缀建议
- 移动端 Story：`RL-M-xxx`
- Web Story：`RL-W-xxx`
- 共享后端/平台：`RL-S-xxx`

### 3.3 通用字段
| 字段 | 建议 |
|---|---|
| Priority | P0/P1/P2 |
| Story Points | 1/2/3/5/8 |
| Components | `mobile-fe` / `web-fe` / `backend` / `qa` |
| Labels | `reallive` + 模块标签 |
| Fix Version | `M1` / `M2` / `M3` / `M4` |

## 4. 移动端研发拆解（Android）

## 4.1 M-Epic-01 认证与账户安全（Mobile Auth）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-M-001 | Story | 【移动认证】邮箱密码登录与登录态保持 | 5 | P0 | S1 | RL-S-001 | 登录成功进入 Dashboard，失败提示明确 |
| RL-M-002 | Story | 【移动认证】忘记密码（发码+重置） | 5 | P1 | S2 | RL-S-002 | 可完成发码、校验、重置流程 |
| RL-M-003 | Story | 【移动安全】2FA 开启流程（App） | 8 | P1 | S2 | RL-S-003 | 可开启 2FA 并展示安全状态 |
| RL-M-004 | Story | 【移动权限】首启权限引导页 | 3 | P1 | S2 | - | 通知/麦克风/相册权限引导完整 |

## 4.2 M-Epic-02 首页与主入口（Mobile Hub）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-M-005 | Story | 【移动首页】在线/离线/告警指标卡 | 3 | P0 | S1 | RL-S-004 | 指标可正确展示与刷新 |
| RL-M-006 | Story | 【移动首页】实时预览卡与相机列表 | 5 | P0 | S1 | RL-S-005 | 预览卡状态与时间戳正确 |
| RL-M-007 | Story | 【移动搜索】搜索页与结果页 | 3 | P1 | S2 | RL-S-006 | 支持关键词检索、空态与历史 |
| RL-M-008 | Story | 【移动通知】告警通知列表页 | 5 | P1 | S2 | RL-S-007 | 未读态、类型态、时间态正确 |

## 4.3 M-Epic-03 实时监控与 Live 子页（Mobile Live）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-M-009 | Story | 【移动实时】多画面/单画面切换 | 5 | P0 | S3 | RL-S-008 | 可切换画面并保持状态一致 |
| RL-M-010 | Story | 【移动操作】抓拍/录制/分享入口 | 5 | P0 | S3 | RL-S-009 | 操作可触发，反馈可见 |
| RL-M-011 | Story | 【移动 PTZ】方向+变焦控制页 | 8 | P0 | S3 | RL-S-010 | PTZ 控制生效，离线禁用 |
| RL-M-012 | Story | 【移动相册】Snapshot Gallery 页面 | 3 | P1 | S3 | RL-S-009 | 缩略图展示和查看流程可用 |
| RL-M-013 | Story | 【移动分享】分享底部弹层 | 3 | P1 | S3 | RL-S-011 | 分享渠道与链接复制可用 |

## 4.4 M-Epic-04 回放与事件详情（Mobile Playback）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-M-014 | Story | 【移动回放】时间轴与速度控制 | 8 | P1 | S4 | RL-S-012 | 时间定位准确，速度切换生效 |
| RL-M-015 | Story | 【移动回放】日期选择器 | 3 | P1 | S4 | RL-S-012 | 日期切换与选中态正确 |
| RL-M-016 | Story | 【移动事件】事件详情页 | 5 | P1 | S4 | RL-S-007 | 事件信息完整，可分享导出入口 |

## 4.5 M-Epic-05 设备接入与设置（Mobile Device & Settings）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-M-017 | Story | 【移动接入】扫码识别与设备绑定起始 | 8 | P1 | S4 | RL-S-013 | 扫码成功后设备信息回显 |
| RL-M-018 | Story | 【移动接入】连接中进度与取消 | 5 | P1 | S4 | RL-M-017 | 进度步骤清晰，支持取消 |
| RL-M-019 | Story | 【移动接入】成功/失败分支与重试 | 5 | P1 | S5 | RL-M-018 | 成功跳转 Live，失败有建议 |
| RL-M-020 | Story | 【移动设置】Settings/Profile/Security 页面 | 8 | P1 | S5 | RL-S-003 | 设置可保存并回显 |
| RL-M-021 | Story | 【移动存储】容量与升级入口页 | 5 | P2 | S6 | RL-S-014 | 容量展示准确，升级入口可用 |
| RL-M-022 | Story | 【移动风险操作】删除确认弹层 | 2 | P0 | S3 | - | 删除前强确认且文案完整 |

## 5. Web 端研发拆解（Console）

## 5.1 W-Epic-01 应用壳与导航（Web Shell）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-001 | Story | 【Web壳】侧边导航与页面切换 | 3 | P0 | S1 | - | 导航切换、标题联动正确 |
| RL-W-002 | Story | 【Web壳】响应式侧边栏（1024/768） | 5 | P1 | S1 | RL-W-001 | 断点下功能可达且不遮挡 |
| RL-W-003 | Story | 【Web全局】Toast 与 ESC 关闭机制 | 3 | P1 | S2 | RL-W-001 | 反馈与关闭行为一致 |

## 5.2 W-Epic-02 Dashboard 与地图（Web Dashboard）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-004 | Story | 【Web首页】指标卡与预览区 | 5 | P0 | S1 | RL-S-004 | 数据与状态展示正确 |
| RL-W-005 | Story | 【Web快捷】Quick Access 四入口联动 | 3 | P1 | S2 | RL-W-001 | 入口可跳转/弹层可用 |
| RL-W-006 | Story | 【Web地图】状态/区域/分组筛选联动 | 8 | P1 | S3 | RL-S-015 | 标记、列表、统计联动正确 |
| RL-W-007 | Story | 【Web地图】分组计数动态刷新 | 3 | P1 | S3 | RL-W-006 | 计数随筛选实时更新 |

## 5.3 W-Epic-03 Live Monitor（Web Live）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-008 | Story | 【Web实时】1x1/2x2/3x3/4x4 布局 | 5 | P0 | S2 | RL-W-001 | 网格切换正确 |
| RL-W-009 | Story | 【Web实时】拖拽布屏（Camera Bank -> Grid） | 8 | P1 | S3 | RL-W-008 | 拖拽赋值成功，状态同步 |
| RL-W-010 | Story | 【Web实时】双击聚焦与 ESC 恢复 | 3 | P1 | S3 | RL-W-008 | 聚焦与恢复行为正确 |
| RL-W-011 | Story | 【Web实时】画面操作（抓拍/PTZ/录制） | 5 | P0 | S3 | RL-S-010 | 操作反馈正确，离线限制生效 |
| RL-W-012 | Story | 【Web轮巡】Patrol 条带与计划弹层 | 5 | P2 | S5 | RL-W-008 | 启停与保存可用 |

## 5.4 W-Epic-04 告警中心（Web Alert）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-013 | Story | 【Web告警】列表页与未读态 | 5 | P0 | S3 | RL-S-007 | 列字段完整、状态正确 |
| RL-W-014 | Story | 【Web告警】类型/时间筛选 | 5 | P0 | S3 | RL-W-013 | 多条件筛选准确 |
| RL-W-015 | Story | 【Web告警】批量已读/归档/删除 | 8 | P1 | S4 | RL-W-013 | 批量操作成功，结果刷新 |
| RL-W-016 | Story | 【Web告警】详情抽屉 | 3 | P1 | S4 | RL-W-013 | 抽屉可打开/关闭，ESC 生效 |
| RL-W-017 | Story | 【Web告警】导出任务 | 5 | P2 | S5 | RL-S-016 | 导出可创建并反馈 |

## 5.5 W-Epic-05 告警规则自动化（Web Rule）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-018 | Story | 【Web规则】列表筛选与关键字搜索 | 8 | P1 | S4 | RL-S-017 | 过滤结果和统计正确 |
| RL-W-019 | Story | 【Web规则】排序与分页 | 8 | P1 | S4 | RL-W-018 | 排序方向、分页结果正确 |
| RL-W-020 | Story | 【Web规则】新建/编辑弹层 | 8 | P1 | S4 | RL-W-018 | 编辑回写、新建追加生效 |
| RL-W-021 | Story | 【Web规则】强校验规则落地 | 8 | P1 | S4 | RL-W-020 | 高优先级约束、唯一性校验有效 |
| RL-W-022 | Story | 【Web规则】批量操作（页级/筛选范围） | 8 | P1 | S5 | RL-W-019 | 作用域切换后目标准确 |
| RL-W-023 | Story | 【Web规则】删除 Undo 恢复 | 5 | P1 | S5 | RL-W-022 | 6 秒内可恢复，超时不可恢复 |

## 5.6 W-Epic-06 Playback（Web Playback）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-024 | Story | 【Web回放】日期选择与事件日标记 | 5 | P1 | S4 | RL-S-012 | 日期选中与事件日展示正确 |
| RL-W-025 | Story | 【Web回放】时间轴与事件跳点 | 8 | P1 | S4 | RL-W-024 | 点击事件后定位准确 |
| RL-W-026 | Story | 【Web回放】相机选择（全选/清空/计数） | 5 | P1 | S5 | RL-W-024 | 计数、按钮状态正确 |
| RL-W-027 | Story | 【Web回放】相机搜索过滤 | 3 | P1 | S5 | RL-W-026 | 搜索只影响显示，不改已选 |
| RL-W-028 | Story | 【Web回放】片段导出入口 | 5 | P2 | S5 | RL-S-018 | 导出入口可用并反馈 |

## 5.7 W-Epic-07 设备与存储（Web Device/Storage）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-029 | Story | 【Web设备】列表与状态筛选 | 5 | P1 | S3 | RL-S-005 | 筛选准确、状态一致 |
| RL-W-030 | Story | 【Web设备】表格/卡片双视图 | 3 | P1 | S3 | RL-W-029 | 视图切换不丢功能 |
| RL-W-031 | Story | 【Web设备】删除确认与影响文案 | 2 | P0 | S3 | RL-W-029 | 删除前强确认 |
| RL-W-032 | Story | 【Web存储】容量总览与占比 | 5 | P1 | S6 | RL-S-014 | 总量与分项一致 |
| RL-W-033 | Story | 【Web存储】趋势图与预测 | 8 | P2 | S6 | RL-W-032 | 趋势正确、预测提示有效 |
| RL-W-034 | Story | 【Web存储】策略配置与清理入口 | 5 | P2 | S7 | RL-W-032 | 保存成功，危险动作确认 |

## 5.8 W-Epic-08 系统设置与治理（Web Settings）
| Issue Key* | Type | Summary | SP | Priority | Sprint | Depends On | 验收摘要 |
|---|---|---|---:|---|---|---|---|
| RL-W-035 | Story | 【Web设置】Tab 切换与单面板激活 | 3 | P1 | S6 | RL-W-001 | 同时仅一个面板激活 |
| RL-W-036 | Story | 【Web安全】密码/2FA/会话/IP白名单 | 8 | P1 | S6 | RL-S-003 | 安全配置可保存与回显 |
| RL-W-037 | Story | 【Web IAM】用户邀请与角色管理 | 8 | P2 | S7 | RL-S-019 | 用户与角色变更生效 |
| RL-W-038 | Story | 【Web审计】日志筛选与导出 | 8 | P2 | S7 | RL-S-020 | 审计可检索与导出 |
| RL-W-039 | Story | 【Web通知】渠道启停与测试 | 5 | P2 | S7 | RL-S-021 | Email/SMS/Webhook 测试反馈可见 |
| RL-W-040 | Story | 【Web系统】NTP/升级/清理/版本信息 | 5 | P2 | S8 | RL-S-022 | 系统操作可执行并反馈 |

## 6. 共享后端/平台研发拆解（S-Epic）

## 6.1 S-Epic-01 认证与安全服务
| Issue Key* | Type | Summary | SP | Priority | Sprint |
|---|---|---|---:|---|---|
| RL-S-001 | Story | 【共享Auth】登录与会话接口 | 5 | P0 | S1 |
| RL-S-002 | Story | 【共享Auth】忘记密码接口 | 5 | P1 | S2 |
| RL-S-003 | Story | 【共享Security】2FA 服务与校验 | 8 | P1 | S2 |

## 6.2 S-Epic-02 监控与设备服务
| Issue Key* | Type | Summary | SP | Priority | Sprint |
|---|---|---|---:|---|---|
| RL-S-004 | Story | 【共享Dashboard】概览指标接口 | 5 | P0 | S1 |
| RL-S-005 | Story | 【共享Device】设备列表与状态接口 | 5 | P1 | S3 |
| RL-S-006 | Story | 【共享Search】搜索聚合接口 | 5 | P1 | S2 |
| RL-S-013 | Story | 【共享Onboard】设备接入状态机接口 | 8 | P1 | S4 |

## 6.3 S-Epic-03 告警与规则服务
| Issue Key* | Type | Summary | SP | Priority | Sprint |
|---|---|---|---:|---|---|
| RL-S-007 | Story | 【共享Alert】告警列表与状态流转接口 | 8 | P0 | S3 |
| RL-S-016 | Story | 【共享Alert】告警导出任务接口 | 5 | P2 | S5 |
| RL-S-017 | Story | 【共享Rule】规则列表筛选排序分页接口 | 8 | P1 | S4 |
| RL-S-018 | Story | 【共享Playback】回放导出任务接口 | 5 | P2 | S5 |
| RL-S-021 | Story | 【共享Notify】通知渠道测试接口 | 5 | P2 | S7 |

## 6.4 S-Epic-04 回放/存储/治理服务
| Issue Key* | Type | Summary | SP | Priority | Sprint |
|---|---|---|---:|---|---|
| RL-S-012 | Story | 【共享Playback】日期/时间轴查询接口 | 8 | P1 | S4 |
| RL-S-014 | Story | 【共享Storage】容量概览与明细接口 | 8 | P1 | S6 |
| RL-S-015 | Story | 【共享Map】地图筛选与分组统计接口 | 8 | P1 | S3 |
| RL-S-019 | Story | 【共享IAM】用户角色管理接口 | 8 | P2 | S7 |
| RL-S-020 | Story | 【共享Audit】审计查询与导出接口 | 8 | P2 | S7 |
| RL-S-022 | Story | 【共享System】系统配置与维护接口 | 8 | P2 | S8 |

## 7. Sprint 排期建议（按端划分）
| Sprint | 移动端重点 | Web 端重点 | 共享后端重点 |
|---|---|---|---|
| S1 | RL-M-001,005,006 | RL-W-001,004 | RL-S-001,004 |
| S2 | RL-M-002,003,004,007,008 | RL-W-002,003,005 | RL-S-002,003,006 |
| S3 | RL-M-009,010,011,022 | RL-W-006,007,008,009,010,013,014,029,030,031 | RL-S-005,007,015 |
| S4 | RL-M-014,015,016,017,018 | RL-W-015,016,018,019,020,024,025 | RL-S-012,013,017 |
| S5 | RL-M-019,020 | RL-W-011,012,017,021,022,023,026,027,028 | RL-S-016,018 |
| S6 | RL-M-021 | RL-W-032,033,035,036 | RL-S-014 |
| S7 | - | RL-W-034,037,038,039 | RL-S-019,020,021 |
| S8 | - | RL-W-040 | RL-S-022 |
| S9 | 回归与发布支持 | 回归与发布支持 | 稳定性、灰度、回滚 |

## 8. Story 子任务模板（移动端 / Web 端）

### 8.1 移动端 Story 子任务
- `[M-FE]` 页面与交互实现
- `[M-BE]` 接口联调与错误处理
- `[M-QA]` 功能+兼容+弱网测试
- `[M-INT]` 联调与回归

### 8.2 Web 端 Story 子任务
- `[W-FE]` 页面、状态管理、响应式
- `[W-BE]` 接口与权限校验
- `[W-QA]` 功能+浏览器+响应式测试
- `[W-INT]` 联调与回归

### 8.3 共享 Story 子任务
- `[S-BE]` 服务实现与单测
- `[S-QA]` 接口测试与回归
- `[S-OPS]` 监控告警与发布配置

## 9. 可复制 Story 模板（按端）

```md
Summary: 【<移动端/Web端>】【模块】<动作 + 对象>
Issue Type: Story
Epic Link: <M-Epic / W-Epic / S-Epic>
Priority: <P0/P1/P2>
Story Points: <1/2/3/5/8>
Components: <mobile-fe/web-fe/backend/qa>
Labels: reallive,<mobile|web|shared>,<module>
Fix Version: <M1/M2/M3/M4>
Depends On: <ISSUE-KEY 可选>

Description:
1. 背景
2. In Scope
3. Out of Scope
4. 业务规则

Acceptance Criteria:
- Given ... When ... Then ...
- Given ... When ... Then ...
- Given ... When ... Then ...

Sub-tasks:
- [FE] ...
- [BE] ...
- [QA] ...
- [INT] ...
```

> `Issue Key*` 为建议命名，实际以 Jira 自动生成为准。
