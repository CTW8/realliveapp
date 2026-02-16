# RealLive App UI 设计说明

## 1. 文档目的
本文档说明当前 `index.html` 中的 App UI 设计结构与规范，用于设计评审、前端实现对齐、以及后续迭代扩展。

## 2. 设计定位
- 产品类型：家庭/园区远程监控应用（实时视频 + 告警 + 回看 + 设备管理）。
- 视觉方向：深色基底、低饱和紫粉色点缀、信息密度中高、偏专业监控控制台风格。
- 交互目标：高频操作可直达（播放、回看、抓拍、分享、设备设置）。

## 3. 视觉系统（Design Tokens）
### 3.1 颜色（核心）
- 主色：`--pri #C8BFFF`
- 辅色：`--sec #C9C3DC`
- 第三色：`--ter #EFBDD3`
- 背景：`--bg #1C1B1F`
- 表面层：`--sc / --sc2 / --sc3`
- 语义色：成功 `--green`、告警 `--orange`、错误 `--red`

### 3.2 圆角与阴影
- 圆角体系：`--r1 4px` 到 `--r6 9999px`
- 阴影层级：`--e1`、`--e2`、`--e3`

### 3.3 字体与图标
- 字体：Roboto
- 图标：Material Symbols Outlined
- 手机画板：`375 x 812`（模拟设备窗口）

## 4. 页面信息架构
当前共 22 个界面，按 7 组流程组织：
- A 认证流程：Splash、Login、Register
- B 主入口：Dashboard、Search、Notifications
- C 摄像头视图：Multi View、Camera List、Live View
- D Live 子页：PTZ、Snapshot Gallery、Share Bottom Sheet
- E 回看：History Playback、Calendar Picker、Event Detail
- F 设备管理：Add Camera(Scan)、Camera Setup、Camera Settings
- G 账户设置：Settings、Profile、Storage、Security

## 5. 关键组件规范
- 顶部区：状态栏 + Top App Bar（返回/标题/操作）
- 内容区：滚动容器 `.pc`，统一隐藏滚动条样式
- 底部导航：4 Tab（Dashboard/Cameras/Alerts/Settings）
- 数据组件：卡片、Chip、开关、时间轴、底部分享面板、Snackbar
- 视频组件：Live/History/Event 顶部播放器，叠加控制层（标题、操作、进度）

## 6. 交互与状态表达
- 在线/离线/录制通过颜色与标签双重表达
- 告警事件用图标 + 时间 +摘要分层展示
- 历史回看使用“时间轴线 + 时间点 + 事件卡片”结构
- 可横向滚动区域（筛选、分享渠道）已做文本溢出保护

## 7. 当前实现边界
- 当前为静态高保真原型（无真实数据与后端联动）
- 无暗/亮主题切换，仅深色主题
- 适合作为下一步组件化拆分与交互接入基础稿

