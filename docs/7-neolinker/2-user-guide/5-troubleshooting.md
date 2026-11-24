---
description: NeoLinker 常见问题排查：BLE 扫不到设备、蓝牙配对失败、softAP 连不上、交付失败错误码、WebView2 缺失、NE503 发现不到等场景的现象、原因与处理办法。
keywords: [NeoLinker, 故障排除, FAQ, BLE, 配对, softAP, WebView2, 错误码]
tags: [NeoLinker, 故障排除]
sidebar_label: "Troubleshooting"
---

# 故障排除

按场景排查。仍无法解决时，到 [NeoLinker 仓库](https://github.com/camthink-ai/NeoLinker/issues) 提交 issue，并附上导出的会话历史与环境信息（Windows 版本、是否有蓝牙适配器等）。

## BLE 扫不到设备

**现象**：扫描页 BLE 一路长时间为空。

- 确认设备确实处于**配置模式**（进入方式见设备随附说明）；
- 确认自检页 **BLE 一路就绪**：系统蓝牙已开启、适配器驱动正常；台式机建议使用 USB 蓝牙适配器；
- **拉近距离**重试——RSSI 过低时广播可能收不到；
- 确认没有其他主机正在配置这台设备（设备广播 BUSY 标志时不易被发现）；
- 换一台设备交叉验证，区分是设备侧还是电脑侧问题。

## 蓝牙配对失败 / 连接超时

**现象**：点连接后 Windows 配对失败，或连接长时间无响应。

- 先在 Windows 蓝牙设置里**删除已存在的配对记录**，回到扫描页重连；
- BLE 链路使用加密，配对弹窗务必点确认（不要忽略）；
- 距离与干扰：远离微波炉 / USB 3.0 hub 等干扰源；
- 反复失败时重启电脑蓝牙（关-开飞行模式或重启蓝牙服务）后重试。

## softAP 连不上

**现象**：softAP 一路能看到 SSID，但后续连接 / 通信失败。

- 确认电脑 Wi-Fi 已按应用指引切到设备热点 `CT-AP-<地址后缀>`；
- 部分网卡会同时连接原 Wi-Fi——配置期间**先断开其他 Wi-Fi**；
- 若手动访问设备，默认地址见[发现设备](./2-discover-devices.md)中的地址表；
- 设备热点有人数限制：确认没有其他设备占着这条热点链路。

## 交付失败（错误码）

**现象**：下发后结果页报错。

对照[配置设备](./3-provision-device.md)中的错误码表：

- `INVALID_ARGUMENT`——按表单校验提示修正；
- `SECTION_NOT_SUPPORTED` / `FIELD_NOT_SUPPORTED`——该机型 / 模块不支持此配置节或字段，去掉后重试；
- `BUSY`——稍候重试，确认无其他主机在配置同一设备；
- `STORAGE_FAILED`——重试；反复出现说明设备存储异常，需进一步检修。

## 提示缺少 WebView2

**现象**：启动或打开 NE503 管理台时提示 WebView2 缺失。

安装 [WebView2 Evergreen 运行时](https://developer.microsoft.com/microsoft-edge/webview2/)（x64）后重启应用。详见[安装与环境](./1-install-setup.md)。

## NE503 在局域网里发现不到

**现象**：设备与电脑都在网内，LAN 一路仍为空。

- 确认两者在**同一网段**（CT-Disc 使用 UDP 组播，跨网段默认不可达）；
- 检查网内交换机 / 路由器是否放行组播；
- 等待数秒——设备周期性播报 `ct-announce`，有间隔；
- 确认设备固件为支持 CT-Disc 的版本。

## 交付成功了，但设备没连上 Wi-Fi

这不是工具故障：NeoLinker 只负责把配置**写入设备并持久化**，不验证运行时联网。请检查：

- SSID / 密码是否正确（2.4 GHz 与 5 GHz 是否选对）；
- 设备侧日志或设备本地页面确认关联状态；
- 路由器侧 MAC 过滤 / 黑白名单。

## 其他

- **界面语言 / 主题**：右上角或设置页切换，见[安装与环境](./1-install-setup.md)；
- **从源码构建**遇到问题：见 [NeoLinker 仓库 README](https://github.com/camthink-ai/NeoLinker) 与 CI 配置。
