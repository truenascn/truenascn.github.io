# 不间断电源（UPS）

TrueNAS 使用 NUT（网络 UPS 工具）来提供 UPS 支持。 当 TrueNAS 系统连接到 UPS 设备时，通过转到服务，找到 UPS 条目并单击 来配置 UPS 服务。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesUPSOptions.png)

## 具体选项



### 通用选项（General Options）

| 设置选项                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 识别码（**Identifier**）             | 描述 UPS 设备。 它可以包含字母数字、句点、逗号、连字符和下划线字符。 |
| UPS模式（**UPS Mode**）              | 如果 UPS 直接插入系统串行端口，请选择 **Master**。 UPS 将保持最后一个关闭项目。 选择 **Slave** 可在 **Master** 之前关闭此系统。 请参阅 [网络 UPS 工具概述](http://networkupstools.org/docs/user-manual.chunked/ar01s02.html#_monitoring_client)。 |
| 驱动程序（**Driver**）               | 有关支持的 UPS 设备列表，请参阅 [网络 UPS 工具兼容性列表](http://networkupstools.org/stable-hcl.html)。 |
| 端口与主机名（**Port or Hostname**） | 连接到 UPS 的串行或 USB 端口。 要自动检测和管理 USB 端口设置，请选择 *auto*。 选择 SNMP 驱动程序后，输入 SNMP UPS 设备的 IP 地址或主机名。 |

### 监视（Monitor）

| 设置选项                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 监视用户（**Monitor User**）     | 输入要与此服务关联的用户。 建议保留默认值。                  |
| 监视密码（**Monitor Password**） | 更改默认密码以提高系统安全性。 新密码不能包含空格或#.Enter 具有管理访问权限的帐户。 有关示例，请参阅 upsd.users(5)。 |
| 额外用户（**Extra Users**）      | 输入具有管理访问权限的帐户。 有关示例，请参阅 [upsd.users(5)](https://www.freebsd.org/cgi/man.cgi?query=upsd.users)。 |
| 远程监视（**Remote Monitor**）   | 将默认配置设置为使用已知的用户侦听所有接口。用户：**upsmon** 和密码：**fixmepass** |

### 关机（Shutdown）

| 设置选项                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 关机模式（**Shutdown Mode**）    | 选择 UPS 何时关机。                                          |
| 关机计时器（**Shutdown Timer**） | 输入 UPS 在启动关机前等待的秒数。 如果在定时器倒计时期间恢复供电，则不会发生关机。 该值仅在关机模式设置为 UPS 使用电池供电时适用。 |
| 关机命令（**Shutdown Command**） | 输入命令以在电池电量低或关机计时器结束时关闭系统。           |
| 关闭UPS（**Power off UPS**）     | 设置为关闭系统后 UPS 断电。                                  |

### 邮件（Email）

| 设置选项                                      | 描述                                                     |
| --------------------------------------------- | -------------------------------------------------------- |
| 发信状态更新（**Send Email Status Updates**） | 设置启用向电子邮件字段中定义的地址发送消息。             |
| 邮箱（**Email**）                             | 输入任何电子邮件地址以接收电源状态。 按 Enter 分隔条目。 |
| 邮件主题（**Email Subject**）                 | 输入状态电子邮件的主题。                                 |

### 其它选项（Other Options）

| 设置选项                                            | 描述                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| 无连接警告时间（**No Communication Warning Time**） | 输入在警告服务无法到达任何 UPS 之前要等待的秒数。 警告会一直持续，直到情况得到解决。 |
| 主机同步（**Host Sync**）                           | Upsmon 将在主机模式下等待这么多秒，以便从机在关机情况下断开连接。 |
| 描述（**Description**）                             | 描述这项服务。                                               |
| 辅助参数（**Auxiliary Parameters (ups.conf)**）     | 输入来自 [ups.conf](http://networkupstools.org/docs/man/ups.conf.html) 的任何额外选项。 |
| 辅助参数（**Auxiliary Parameters (upsd.conf)**）    | 输入来自 [upsd.conf](http://networkupstools.org/docs/man/upsd.conf.html) 的任何额外选项。 |

某些 UPS 型号可能对默认轮询频率无响应。 这在 TrueNAS 日志中显示为重复出现的错误，例如 `libusb_get_interrupt: Unknown error`。 如果发生此错误，请通过在辅助参数（**Auxiliary Parameters (ups.conf)**）中添加条目来降低轮询频率：`pollinterval = 10`。默认轮询频率为两秒。

[upsc(8)](https://www.freebsd.org/cgi/man.cgi?query=upsc) 可以从 UPS 守护进程获取状态变量，例如当前电量和输入电压。 使用语法 `upsc ups@localhost` 从 **Shell** 运行它。 upsc(8) 手册页有其他使用示例。

[upscmd(8)](https://www.freebsd.org/cgi/man.cgi?query=upscmd) 可以直接向 UPS 发送命令，假设硬件支持正在发送的命令。 只有具有管理权限的用户才能使用此命令。 这些用户是在额外用户（**Extra Users**）字段中创建的。

**Q**：如何找到设备名称？

**A**：对于 USB 设备，确定正确设备名称的最简单方法是在系统（**System**） > 高级（**Advanced**）中设置显示控制台消息。 插入 USB 设备并在控制台消息中查找 /dev/ugen 或 /dev/uhid 设备名称。

**Q**：我可以将多台计算机连接到一台 UPS 上吗？

**A**；具有足够容量的 UPS 可以为多台计算机供电。 一台计算机通过串行或 USB 电缆连接到 UPS 数据端口。 该主系统使网络上的 UPS 状态可供其他计算机使用。 辅助计算机由 UPS 供电，但从主计算机接收 UPS 状态数据。 请参阅 [NUT 用户手册](https://networkupstools.org/docs/user-manual.chunked/index.html)和 [NUT 用户手册页](https://networkupstools.org/docs/man/index.html#User_man)。

