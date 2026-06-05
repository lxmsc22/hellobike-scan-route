# 哈啰新版扫一扫 URL Scheme 记录

本仓库记录 `hellobike://hellobike.com/scan_qr_platform` 等哈啰新版扫一扫 URL Scheme，用于 iOS 快捷指令打开哈啰 App 扫码界面。

## 结论

哈啰至少保留了两个可以打开新版扫码界面的入口。

日常快捷指令优先使用：

```text
hellobike://hellobike.com/scan_qr_platform
```

备用新版入口：

```text
hellobike://hellobike.com/moped/scan/code
```

旧版扫一扫入口：

```text
hellobike://hellobike.com/scan_qr
```

## 证据摘要

`hellobike://hellobike.com/scan_qr_platform` 来自 IPA 中的 Widget Extension 静态字符串：

```text
Payload/HelloTrip.app/PlugIns/HelloTripWidgetExtension.appex/HelloTripWidgetExtension
```

在本次静态分析中，该候选入口按以下规则评分后排名第一：

- 包含 `hellobike://`：+3
- 包含 `scan` / `qr`：+3
- 来源位于 `PlugIns/*.appex`：+2
- 来源关联 Widget / Intent / Shortcut / NSUserActivity 相关字符串：+2

总分：`10`

`hellobike://hellobike.com/moped/scan/code` 来自主 App 的 `App.framework`：

```text
Payload/HelloTrip.app/Frameworks/App.framework/App
```

从静态分析来源看，二者定位不太一样：

- `scan_qr_platform` 来自 `HelloTripWidgetExtension.appex`，也就是小组件扩展中发现的入口，更像“出行卡扫一扫”的通用平台扫码。
- `moped/scan/code` 来自主 App 的 `App.framework`，路径里有 `moped`，字面上更像电动车/助力车扫码入口。

## 快捷指令用法

在 iOS 快捷指令中创建：

1. `URL`
2. 填入 `hellobike://hellobike.com/scan_qr_platform`
3. `打开 URL`

如果优先入口不可用，可先回退测试备用新版入口：

```text
hellobike://hellobike.com/moped/scan/code
```

再测试旧版入口：

```text
hellobike://hellobike.com/scan_qr
```

## 分析范围

本结论仅基于静态分析：

- `Info.plist`
- Widget / Extension 相关 plist
- 可读文本中的字符串
- 二进制中的 ASCII 字符串
- 二进制中的 UTF-16LE 字符串

未进行：

- 解密
- 破解
- 重签名
- 注入
- 绕过 DRM
- 修改 IPA 或 Payload 文件

## 备注

`hellobike://hellobike.com/scan_qr_platform` 可能尚未在公开互联网被广泛记录。本记录的来源是对已解压 IPA 中 `HelloTripWidgetExtension.appex` 的静态字符串分析。

建议日常快捷指令优先使用 `hellobike://hellobike.com/scan_qr_platform`，因为它来自小组件扩展且路径名包含 `platform`，语义更通用。`hellobike://hellobike.com/moped/scan/code` 可以保留作备用；它可能在某些版本中跳到同一个扫码模块，但如果后续区分单车、电动车等业务，该入口更可能受具体业务限制。

App 版本、系统版本、登录状态、地区、业务开关都可能影响 URL Scheme 的实际打开效果。
