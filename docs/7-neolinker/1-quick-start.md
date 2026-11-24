---
description: NeoLinker 五分钟上手：下载解压 → 环境自检 → 扫描发现设备 → 连接 → 配置 Wi-Fi → 交付并核对回执。以 NE101 的 BLE 通道为主线，每步附检查点与排障提示。
keywords: [NeoLinker, 快速上手, BLE 配网, Wi-Fi 配置, NE101, 设备发现]
tags: [NeoLinker, 快速上手]
sidebar_label: "Quick Start"
---

# 五分钟上手

以 **NE101 + BLE 通道**为主线，把一台相机从配置模式走到配网完成。每一步都有 ✓ 检查点；卡住时看排障提示或 [故障排除](./2-user-guide/5-troubleshooting.md)。

**前置条件**：

- Windows 10/11 x64 电脑，带蓝牙适配器（BLE 通道必需）
- 一台 NE101 相机，已置于配置模式（进入方式见设备随附说明）
- 一台 2.4 GHz Wi-Fi 路由器（相机与电脑不必同网）

---

## 1. 下载并运行

从 [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases) 下载最新的 `NeoLinker_v*_windows_x64.zip`，解压到任意目录，双击 `NeoLinker.exe`。无需安装。

<img src="/img/neolinker/placeholders/selfcheck-zh.png" alt="环境自检页（占位图，待替换为真实截图）" />

✓ **检查点**：应用打开后显示中文或英文界面（可在右上角切换语言与亮暗主题）。

> 提示：若启动或打开 NE503 控制台时提示缺少 WebView2，安装 [WebView2 Evergreen 运行时](https://developer.microsoft.com/microsoft-edge/webview2/) 后重试。

## 2. 环境自检

应用启动后先进入**自检页**，逐路检查 BLE 适配器 / WLAN / LAN 的可用性。BLE 通道显示就绪即可继续。

✓ **检查点**：BLE 一路显示绿色 / 就绪。

> 排障：BLE 显示不可用时，确认电脑蓝牙已开启、适配器驱动正常；台式机无内置蓝牙可使用 USB 蓝牙适配器。详见 [安装与环境](./2-user-guide/1-install-setup.md)。

## 3. 扫描发现设备

进入**扫描页**，应用开始三路扫描（BLE 广播 / softAP SSID / 局域网 CT-Disc）。把 NE101 置于配置模式后，几秒内列表中会出现该设备，含机型、通道、信号强度与最近在线时间。

- 可按**机型 / 通道**过滤，列表分页；
- 也可以点**扫码录入**：用摄像头扫描或直接上传设备随附的二维码图片。

<img src="/img/neolinker/placeholders/scan-zh.png" alt="扫描页设备列表与过滤器（占位图，待替换为真实截图）" />

✓ **检查点**：列表中出现你的 NE101（BLE 通道），信号强度合理。

> 排障：扫不到设备时，确认设备确实处于配置模式、且与电脑距离不要太远；详见 [发现设备](./2-user-guide/2-discover-devices.md)。

## 4. 连接设备

在列表中选中设备并点**连接**。BLE 通道使用加密链路——如 Windows 弹出蓝牙配对提示，按提示确认配对。连接后应用自动读取设备信息与能力（它支持哪些配置模块）。

✓ **检查点**：连接页显示设备信息（机型 / 序列号等），并进入能力读取。

> 排障：配对失败或连接超时，回到扫描页重试；多次失败见 [故障排除](./2-user-guide/5-troubleshooting.md)。

## 5. 配置 Wi-Fi

进入配置页后，勾选 **Wi-Fi** 配置节：

1. 选择目标网络——可让设备扫描周边 Wi-Fi 后从列表选择，或手动输入 SSID；
2. 输入密码（输入框可切换明文核对）；
3. 表单下方可展开 **payload 预览**，核对即将下发的 JSON。

设备不支持或不适用的配置节不会出现——表单完全由设备上报的能力生成。

<img src="/img/neolinker/placeholders/config-zh.png" alt="配置页 Wi-Fi 表单与 payload 预览（占位图，待替换为真实截图）" />

✓ **检查点**：Wi-Fi 节表单填写完成，无红色校验错误。

## 6. 交付并核对回执

点**下发**。设备写入配置并持久化，成功后应用进入**结果页**，给出本次会话的回执：各配置节的写入时间与持久化计数等。

✓ **检查点**：结果页显示交付成功、回执内容与你填写的一致。

> 说明：交付成功表示配置已写入设备存储；设备随后自行连接 Wi-Fi，联网结果由设备侧日志或平台侧呈现，不在本工具的反馈范围内。

## 7. 收尾

- 点**配置下一台设备**回到扫描页，进入下一台（产线式循环）；
- 或到**历史**页查看 / 导出过往会话；
- NE503 用户：设备经局域网发现后，可在应用内直接打开其管理台（WebView2 内嵌）。

---

## 下一步

- [安装与环境](./2-user-guide/1-install-setup.md)——WebView2、蓝牙适配器等运行细节
- [发现设备](./2-user-guide/2-discover-devices.md)——三路通道、过滤与扫码录入
- [配置设备](./2-user-guide/3-provision-device.md)——七类配置节与交付语义
- [回执与历史](./2-user-guide/4-receipts-history.md)——回执导出与产线循环
