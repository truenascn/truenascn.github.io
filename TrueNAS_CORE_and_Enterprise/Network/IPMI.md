# IPMI

IPMI 需要兼容的硬件！ 请参阅您的硬件文档以确定此选项是否会出现在 TrueNAS 网络界面中。

许多 [TrueNAS 存储阵列](https://www.truenas.com/docs/hardware/)提供内置的带外管理端口，如果系统通过 Web 界面不可用，可用于提供边带管理。 这允许执行一些重要的功能，例如检查日志、访问 BIOS 设置和打开系统电源，而无需对系统进行物理访问。 它还可用于允许其他人远程访问系统以协助配置或故障排除问题。

某些 IPMI 实现需要更新才能使用较新版本的 Java。 有关更多信息，请参阅 [PSA：Java 8 Update 131 破坏了华擎的 IPMI 虚拟控制台](https://forums.freenas.org/index.php?threads/psa-java-8-update-131-breaks-asrocks-ipmi-virtual-console.53911/)。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkIPMI.png)

从网络（**Network**） > **IPMI** 配置IPMI。 IPMI 配置页面提供了最基本的 IPMI 配置的快捷方式。

## IPMI 配置

| 设置选项                                 | 值                      | 描述                                                         |
| ---------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| TrueNAS控制器（**TrueNAS Controller**）  | 下拉菜单/drop-down menu | 选择一个 TrueNAS 控制器。 所有 IPMI 更改都应用于该 TrueNAS 控制器。 |
| 频道（**Channel**）                      | 下拉菜单/drop-down menu | 选择要使用的通信频道。 可用通道数因硬件而异。                |
| 口令（**Password**）                     | 字符串/string           | 输入用于从 Web 浏览器连接到 IPMI 界面的密码。 UI 中接受的最大长度为 20 个字符，但不同的硬件可能需要较短的密码。 |
| 动态主机分配协议（**DHCP**）             | 复选框/checkbox         | 如果未设置，则必须设置 IPv4 地址、IPv4 网络掩码和 Ipv4 默认网关。 |
| IPv4地址（**IPv4 Address**）             | 字符串/string           | 用于连接到 IPMI Web 界面的 IP 地址。                         |
| IPv4子网掩码（**IPv4 Netmask**）         | 下拉菜单/drop-down menu | 与 IP 地址关联的子网掩码。                                   |
| IPv4默认网关（**IPv4 Default Gateway**） | 字符串/string           | 与 IP 地址关联的默认网关。                                   |
| 虚拟局域网ID（**VLAN ID**）              | 字符串/string           | 如果 IPMI 带外管理接口与管理网络不在同一 VLAN 上，请输入 VLAN ID。 |
| 指示灯（**IDENTIFY LIGHT**）             | 按钮/button             | 显示一个对话框以激活兼容连接硬件上的 IPMI 识别灯。           |

## IPMI 选项

保存配置后，可使用 Web 浏览器和网络（**Network**） > **IPMI** 中指定的 IP 地址访问 IPMI 界面。 管理界面会提示输入登录凭据。 请参阅您的 IPMI 设备文档以了解默认管理员帐户凭据。

登录管理界面后，可以更改默认管理用户名并创建其他 IPMI 用户。 IPMI 实用程序的外观和可用的功能因硬件而异。
