# RealLive Mobile 开发技术拆解（详细版）

## 1. 目标与范围
- 目标：将 `prototypes/mobile-app/index.html` 静态原型拆解为可工程化 Android 客户端实现。
- 范围：认证、首页、搜索、通知、实时监看、PTZ、回放、设备接入、设置、安全、存储。
- 非范围：iOS 客户端、支付闭环、底层视频传输协议实现。

## 2. 原型结构盘点

## 2.1 页面分组（原型标识）
- A: Auth（Splash/Login/Register）
- B: Main Hub（Dashboard/Search/Notifications）
- C: Camera Views（Single Live Entry/Camera List/Live View）
- D: Live Sub-pages（PTZ/Snapshot/Share）
- E: History（Playback/Calendar/Event Detail）
- F: Device Management（Scan/Setup/Settings）
- G: Account（Settings/Profile/Storage/Security）
- H: Edge Cases（Forgot Password/Connecting/Success/Failed/2FA 等）

## 2.2 明确交互约束
1. 状态表达（在线/录制/离线）需颜色+文案双通道。
2. 设备接入存在明确状态机（连接中/成功/失败）。
3. 删除类操作需确认弹层。
4. 权限引导是首启关键路径之一。

## 3. 推荐工程架构（Android）

## 3.1 技术栈建议
- Kotlin + Jetpack Compose
- 架构：Clean + MVVM
- 状态：StateFlow
- 网络：Retrofit + OkHttp
- 序列化：Kotlinx Serialization
- 本地：Room / DataStore
- 依赖注入：Hilt
- 测试：JUnit + Turbine + Compose UI Test

## 3.2 目录建议
```text
mobile/
  app/
    src/main/
      java/.../
        core/
          network/
          ui/
          model/
          util/
        data/
          remote/
          local/
          repository/
        domain/
          usecase/
        feature/
          auth/
          dashboard/
          live/
          playback/
          alerts/
          device/
          settings/
          security/
          storage/
```

## 4. 模块级技术拆解

## 4.1 Auth
- 功能：登录、注册、忘记密码、2FA。
- 关键状态：`Idle/Loading/Success/Error`。
- 关键接口：`/auth/login`、`/auth/forgot-password/*`、`/auth/2fa/*`。

## 4.2 Dashboard / Search / Notifications
- 功能：首页指标、预览、搜索、通知列表。
- 关键点：列表性能、空态、未读态、快速入口。

## 4.3 Live + PTZ + Share
- 功能：单画面实时查看、抓拍、PTZ、分享。
- 关键点：移动端单路播放约束、离线禁用策略、操作反馈、底部弹层一致性。

## 4.4 Playback
- 功能：日期切换、时间轴定位、事件详情。
- 关键点：时间定位精度、事件跳转联动、快进速度控制。

## 4.5 Device Onboarding
- 功能：扫码识别、连接中、成功/失败分支、设备配置。
- 状态机：`Scanning -> Connecting -> Success/Failed`。
- 关键点：失败原因可解释、可重试、可取消。

## 4.6 Settings / Security / Storage
- 功能：账号信息、安全选项、存储信息。
- 关键点：2FA 引导、危险动作确认、容量告警阈值样式。

## 5. 移动端公共能力

## 5.1 UI 规范
- 主题 token 与原型一致（深色基底 + 语义色）。
- 统一按钮、卡片、Chip、Dialog、BottomSheet 组件。

## 5.2 状态与错误处理
- 所有网络请求必须覆盖 `loading/error/empty/success`。
- 错误统一映射（错误码 -> 用户可理解文案）。

## 5.3 权限管理
- 通知、麦克风、相册权限按场景请求。
- 权限被拒绝时提供二次引导和设置页入口。

## 6. 移动端测试拆解

## 6.1 单元测试
- 登录状态流转
- 设备接入状态机
- 回放时间轴定位算法

## 6.2 UI 测试
- 登录流程
- 实时操作按钮可用性（在线/离线）
- 设备接入失败重试路径

## 6.3 回归关键场景
1. 登录 -> Dashboard -> Live
2. 告警 -> 回放 -> 事件详情
3. 扫码接入 -> 成功/失败分支
4. Security -> 2FA 开启

## 7. Definition of Done（Mobile）
1. 对应 PRD 验收通过。
2. 关键状态完整（空态/错误态/加载态）。
3. 关键路径有自动化或可复现测试记录。
4. 权限、删除、2FA 等高风险流程已验证。
