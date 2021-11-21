# 链路层发现协议 (LLDP)

网络设备使用[链路层发现协议 (LLDP)](https://tools.ietf.org/html/rfc4957) （**Link Layer Discovery Protocol (LLDP)**）在以太网网络上通告其身份、功能和邻居。 TrueNAS 使用 [ladvd](https://github.com/sspans/ladvd) LLDP 实现。 当本地网络包含管理型交换机时，配置和启动 LLDP 服务会让 TrueNAS 系统在网络上通告自己。

要配置 LLDP，请转到“服务”（**Services**）页面，找到 LLDP 条目，然后单击编辑图标 。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesLLDPOptions.png)

## 通用选项

| 设置选项                                  | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| 网络接口描述（**Interface Description**） | 启用接收模式。 任何接收到的对等信息都保存在接口描述中。      |
| 国家/地区代码（**County Code**）          | 用于启用 LLDP 位置支持的两个字母 [ISO 3166-1 alpha-2](https://www.iso.org/obp/ui/) 代码。 |
| 位置（**Location**）                      | 主机的物理位置。                                             |

在打开 LLDP 服务之前，请设置网络接口描述（**Interface Description**）并输入国家/地区代码（**County Code**）。

