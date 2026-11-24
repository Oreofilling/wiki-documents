---
description: NeoLinker 安装与环境说明：下载解压即用、WebView2 Evergreen 运行时、蓝牙适配器要求、语言与主题设置，以及各通道对运行环境的要求。
keywords: [NeoLinker, 安装, WebView2, 蓝牙适配器, 系统要求, Windows]
tags: [NeoLinker, 安装配置]
sidebar_label: "Install & Setup"
---

# 安装与环境

NeoLinker 是免安装的绿色软件：解压即用。运行环境的要求集中在**WebView2 运行时**与**蓝牙适配器**两点。

## 下载与运行

1. 从 [GitHub Releases](https://github.com/camthink-ai/NeoLinker/releases) 下载最新的 `NeoLinker_v*_windows_x64.zip`；
2. 解压到任意有写权限的目录；
3. 双击 `NeoLinker.exe` 运行。

以普通权限即可运行；但 **softAP 通道的热点枚举需要管理员权限**——未提权时该通道自动降级（BLE / LAN 通道不受影响）。卸载即删除目录。

## 系统要求

| 项目 | 要求 | 说明 |
| ---- | ---- | ---- |
| 操作系统 | Windows 10/11 x64 | 暂无其他平台计划公布 |
| 运行时 | WebView2 Evergreen | Windows 11 及多数已更新的 Windows 10 自带；NE503 内嵌管理页依赖它 |
| 蓝牙 | 支持 BLE 的蓝牙适配器 | 仅 BLE 通道需要；softAP / LAN 通道不需要 |
| 网络 | WLAN 网卡 / 以太网 | softAP 通道用 WLAN；LAN 通道（CT-Disc）用以太网或同网段 WLAN |

## WebView2 Evergreen 运行时

应用内嵌浏览器（NE503 设备管理页）基于 WebView2。若启动或打开管理页时提示缺少运行时：

1. 打开 [WebView2 下载页](https://developer.microsoft.com/microsoft-edge/webview2/)；
2. 下载 **Evergreen Standalone** 安装包（x64）并安装；
3. 重启 NeoLinker。

## 蓝牙适配器（BLE 通道）

- 笔记本通常自带蓝牙，确认系统蓝牙开关已打开；
- 台式机可使用任意支持 BLE（蓝牙 4.0+）的 USB 适配器；
- 应用启动时的**自检页**会逐路检查 BLE / WLAN / LAN 的可用性——哪一路报红就先解决哪一路。

## 语言与主题

- 界面支持**中文 / English** 双语与**亮 / 暗**双主题；
- 在应用右上角或**设置页**切换，选择会被持久化，下次启动保持。

## 从源码构建（可选）

NeoLinker 开源（Apache-2.0），可用 Flutter 自行构建：

- 前置：Windows 10/11 x64、Flutter 3.47.8（钉版）、Visual Studio 2022 Build Tools（*使用 C++ 的桌面开发* 工作负载）、`PATH` 上的 `nuget.exe`；
- 步骤：克隆 [NeoLinker 仓库](https://github.com/camthink-ai/NeoLinker) 后，在 `apps/desktop_app` 下依次执行 `flutter pub get` → `flutter gen-l10n` → `flutter run -d windows`（或 `flutter build windows --release`）。

详见仓库 [README](https://github.com/camthink-ai/NeoLinker)。
