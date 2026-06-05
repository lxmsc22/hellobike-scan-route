# 哈啰新版扫一扫 URL Scheme 记录

本仓库记录一次针对已解压 iOS IPA 的静态字符串与 plist 分析结论，用于 iOS 快捷指令打开哈啰 App 扫码界面。

## 结论

哈啰新版扫一扫入口：

```text
hellobike://hellobike.com/scan_qr_platform
```

哈啰旧版扫一扫入口：

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

另一个扫码相关候选：

```text
hellobike://hellobike.com/moped/scan/code
```

来源：

```text
Payload/HelloTrip.app/Frameworks/App.framework/App
```

## 快捷指令用法

在 iOS 快捷指令中创建：

1. `URL`
2. 填入 `hellobike://hellobike.com/scan_qr_platform`
3. `打开 URL`

如果新版入口不可用，可回退测试旧版入口：

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

App 版本、系统版本、登录状态、地区、业务开关都可能影响 URL Scheme 的实际打开效果。
