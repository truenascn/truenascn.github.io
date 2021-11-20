# 块共享 (iSCSI)

iSCSI（Internet 小型计算机系统接口）代表使用基于 Internet 的协议来链接二进制数据存储设备聚合的标准。 IBM 和 Cisco 于 2000 年 3 月提交了标准草案。从那时起，iSCSI 被广泛采用到企业 IT 环境中。

iSCSI 通过封装发挥作用。 OSI（开放系统互连模型）将 SCSI 命令和存储数据封装在会话堆栈中。 OSI 进一步将会话堆栈封装在传输堆栈中，将传输堆栈封装在网络堆栈中，并将网络堆栈封装在数据堆栈中。 以这种方式传输数据允许通过 LAN、WAN 甚至 Internet 本身对存储设备进行块级访问（尽管如果您的数据流量通过 Internet，性能可能会受到影响）。

下表显示了 iSCSI 在 OSI 网络堆栈中的位置：

| OSI 层数 | OSI 层名称 | 与 iSCSI 相关的活动                                          |
| -------- | ---------- | ------------------------------------------------------------ |
| 7        | 应用层     | 层一个应用程序告诉 CPU 它需要将数据写入非易失性存储器。      |
| 6        | 表示层     | OSI 创建 SCSI 命令、SCSI 响应或 SCSI 数据有效负载来保存应用程序数据并将其传送到非易失性存储 |
| 5        | 会话层     | 源设备和目标设备之间的通信开始。 这种交流建立了对话何时开始、将传输什么以及转换何时结束。 整个交流代表会话。 OSI 将包含应用程序数据的 SCSI 命令、SCSI 响应或 SCSI 数据有效负载封装在 iSCSI 协议数据单元 (PDU) 中。 |
| 4        | 传输层     | OSI 将 iSCSI PDU 封装在 TCP 段中。                           |
| 3        | 网络层     | OSI 将 TCP 段封装在 IP 数据包中。                            |
| 2        | 数据链路层 | OSI 将 IP 数据包封装在以太网帧中。                           |
| 1        | 物理层     | 以太网帧作为位（零和一）传输。                               |

与 TrueNAS 上的其他共享协议不同，iSCSI 共享允许[块共享](https://www.ibm.com/cloud/learn/block-storage)和文件共享。 块共享提供了对 TrueNAS 上的数据进行块级访问的好处。 iSCSI 通过网络导出磁盘设备（TrueNAS 上的 zvols），其他 iSCSI 客户端（启动器）可以连接和安装该网络驱动器。

## iSCSI 配置方法

有几种不同的方法来配置和管理 iSCSI 共享数据：

- TrueNAS CORE 网络界面: TrueNAS 网络界面完全能够配置 iSCSI 共享。这需要使用数据创建和填充 [zvol 块设备](https://www.truenas.com/docs/core/storage/pools/zvols/)，然后设置 [iSCSI Share](https://www.truenas.com/docs/core/sharing/iscsi/iscsishare/)。 TrueNAS Enterprise 许可客户还可以使用其他选项来配置与 [光纤通道](https://www.truenas.com/docs/core/sharing/iscsi/fiberchannel/) 的共享。
- TrueNAS SCALE Web 界面: TrueNAS SCALE 提供与 TrueNAS CORE 类似的体验，用于使用 iSCSI 管理数据；创建并填充块存储，然后配置 iSCSI 共享。
- 连接了许多 TrueNAS 系统的 TrueCommand 实例可以从 TrueCommand Web 界面[管理 iSCSI 卷](https://www.truenas.com/docs/truecommand/iscsimanagement/)。 TrueCommand 允许从一个中心位置创建块设备并配置 iSCSI 目标和启动器。
- 使用 vCenter 管理系统的 TrueNAS Enterprise 客户可以使用 [TrueNAS vCenter Plugin](https://www.truenas.com/docs/core/solutions/integrations/vmware/truenasvcenterplugin/#system-management) 连接他们的TrueNAS 系统到 vCenter 并创建和共享 iSCSI 数据存储。这一切都通过 vCenter Web 界面进行管理。