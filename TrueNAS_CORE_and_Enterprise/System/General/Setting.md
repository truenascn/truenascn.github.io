# 设置

TrueNAS 在系统（ **System**） > 常规（**General**）中包含许多设置。 这些允许对系统进行广泛的自定义，从更改 Web 界面地址、本地化选项和数据收集到 SED、控制台和存储选项。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemGeneral.png)

## 图形用户界面（GUI）



| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 图形用户界面 SSL 证书（**GUI SSL Certificate**）             | 系统使用自签名证书来启用加密的 Web 界面连接。 要更改默认证书，请选择在证书（ **Certificates**） 菜单中创建或导入的其他证书。 |
| Web 界面 IPv4 地址（**Web Interface IPv4 Address**）         | 选择 IP 地址以限制访问管理 GUI 时的使用，并在指定地址不可用时发出警报。 内置的 HTTP 服务器默认绑定到 0.0.0.0。 |
| Web 界面 IPv6 地址（**Web Interface IPv6 Address**）         | 选择 IPv6 地址以限制访问管理 GUI 时的使用，并在指定地址不可用时发出警报。 内置的 HTTP 服务器默认绑定到 0.0.0.0。 |
| 网页界面 HTTP 端口（**Web Interface HTTP Port**）            | 允许配置非标准端口以通过 HTTP 访问 GUI。 更改此设置可能需要更改 [Firefox 配置设置](https://www.redbrick.dcu.ie/~d_fens/articles/Firefox:_This_Address_is_Restricted)。 |
| Web 界面 HTTPS 端口（**Web Interface HTTPS Port**）          | 允许配置非标准端口以通过 HTTPS 访问 GUI。                    |
| HTTPS 协议（**HTTPS Protocols**）                            | 用于保护客户端/服务器连接的加密协议。 选择 TrueNAS 可以使用哪些 [传输层安全性 (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) 版本来确保连接安全。 |
| Web 界面 HTTP -> HTTPS 重定向（**Web Interface HTTP -> HTTPS Redirect**） | 将 HTTP 连接重定向到 HTTPS。 HTTPS 需要 GUI SSL 证书。 激活此功能还会将 [HTTP 严格传输安全 (HSTS)](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) 最长期限设置为 31536000 秒（一年）。 这意味着在浏览器首次连接到 Web 界面后，浏览器会继续使用 HTTPS 并每年更新此设置。 |

## 本地化（Localization）

| 项目名称                                   | 描述                       |
| ------------------------------------------ | -------------------------- |
| 语言（**Language**）                       | 从下拉菜单中选择一种语言。 |
| 日期格式（**Date Format**）                | 选择日期格式。             |
| 控制台键盘布局（**Console Keyboard Map**） | 选择键盘布局。             |
| 时区（**Timezone**）                       | 选择一个时区。             |
| 时间格式（**Time Format**）                | 选择时间格式。             |

## 其它选项（Other Options）

| 项目名称                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 崩溃报告（**Crash reporting**）      | 将失败的 HTTP 请求数据发送到 iXsystems，其中可能包括客户端和服务器 IP 地址、失败的方法调用回溯和中间件日志文件内容。 |
| 使用情况收集（**Usage collection**） | 启用向 iXsystems 发送匿名使用统计信息。                      |

进行任何更改后，单击“保存”（**SAVE**）。 在应用新设置时，对任何 Web 界面字段的更改都可能会中断 Web 界面连接。

此屏幕还包含以下按钮：

| 项目名称                        | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| 保存配置文件（**SAVE CONFIG**） | 以主机名-版本-架构格式保存当前配置数据库的备份副本。 此文件将下载到访问 Web 界面的计算机。 强烈建议在进行任何配置更改后保存配置。有关备份系统配置的更多详细信息，请参阅[系统配置备份](https://www.truenas.com/docs/core/system/general/configbackup/)。 |
| 上传配置（**UPLOAD CONFIG**）   | 浏览到以前保存的配置文件以恢复该配置。**这可能会导致系统设置发生意外更改。 在将配置文件上传到 TrueNAS 之前，调查当前系统设置和存储在其他配置文件中的设置。** |
| 重置系统设置（**RESET CONFIG**  | 将配置数据库重置为默认的基本版本。 这不会删除用户 SSH 密钥或存储在用户主目录中的任何其他数据。 由于存储在配置数据库中的配置更改会被删除，因此该选项在纠正错误或将测试系统返回到原始配置时非常有用。 |

