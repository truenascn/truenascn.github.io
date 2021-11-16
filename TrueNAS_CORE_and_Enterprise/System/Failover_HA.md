# 故障转移与高可用

**警告：为避免数据丢失的可能性，在更换控制器或升级到高可用性之前，必须联系 iXsystems。**

## 流程简述

系统（**System**） > 支持（**Support**）
	更新许可证（**Update license**）
网络（**Network**） > 全局配置（**Global Configuration**）
	为两个 TrueNAS 控制器设置主机名
	设置虚拟主机名
网络（**Network**） > 接口（**Interfaces**）
	启用 HA 时无法编辑接口
	定义故障转移组
	为控制器设置 IP 地址
	设置虚拟IP地址
		此 IP 地址用于从此点登录 Web 界面
系统（**System**） > 故障转移（**Failover**）
	指定默认的 TrueNAS 控制器
	定义网络中断后等待多长时间触发故障转移

## 配置高可用性 (HA)

要配置 HA，请打开阵列中的两台设备并登录其中一台设备的 Web 界面。 如果这是第一次登录，UI 会显示一个对话框来上传 TrueNAS 企业许可证。 否则，请转至系统（**System**） > 支持（**Support**）来更新许可证。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemSupportLicenseEnterprise.png)

粘贴从 iXsystems 收到的 HA 许可证并保存。许可证包含机箱中两个单元的序列号。激活 HA 许可证会添加系统（**System**） > 故障转移（**Failover**）页面并修改整个 UI 中的字段，以便可以为两个控制器配置主机名和 IP 地址。

配置 HA 后，当 HA 处于活动状态或不可用时，会显示一个图标。当系统管理员禁用 HA 时，状态图标会更改为显示 HA 不可用。如果备用 TrueNAS 控制器因电源关闭、仍在启动、与网络断开连接或未配置故障转移而无法使用，则状态图标会更改为显示 HA 不可用。如果每个控制器连接了不同数量的磁盘，HA 也会变得不可用。

如果两个 TrueNAS 控制器同时重新启动，则必须在 Web 界面登录屏幕上输入加密池的密码。

### 网络配置

要确保为 HA 配置了系统网络，请首先转到网络（**Network**） > 全局配置（**Global Configuration**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkGlobalConfigurationHAEnterprise.png)

您可以为两个控制器设置主机名，并为当前处于活动状态的任何控制器设置一个虚拟主机名。

接下来，转到网络（**Network**） > 接口（**Interfaces**）并编辑主接口。

注意：当 HA 处于活动状态时，编辑界面被禁用。 要禁用 HA，请转至系统（**System**） > 故障转移（**Failover**）并禁用故障转移。 编辑网络信息后，立即重新激活故障转移。 TrueNAS 自动将配置更改同步到备用控制器

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceEditHAEnterprise.png)

您可以将某个网络接口指定为故障切换的关键接口，并将多个接口组合到一个故障切换组中。 还有一些选项可以为每个控制器配置 IP 地址，以及用于管理访问的带有虚拟主机 ID 的虚拟 IP 地址。

网络配置完成后，注销并重新登录，这次使用虚拟 IP 地址。 现在可以照常配置池和共享，并且配置会在活动和备用 TrueNAS 控制器之间自动同步。

所有后续登录都应使用虚拟 IP 地址。 不允许使用浏览器直接登陆备用 TrueNAS 控制器Web 界面。

在对 HA 网络进行故障排除时，`ifconfig` 命令在输出中添加了两个附加字段以帮助故障转移故障排除：**CriticalGroupn** 和 **Interlink**。

## 故障转移

要对故障转移设置进行常规更改，请转至系统（**System**） > 故障转移（**Failover**）

![](https://www.truenas.com/docs/images/CORE/12.0/SystemFailoverEnterprise.png)

您可以在此屏幕上手动禁用故障转移。

确保将其中一个控制器设置为默认控制器，以便在两者同时启动时默认控制器变为活动状态。 在禁用故障转移的情况下启动 HA 对会导致两个 TrueNAS 控制器都进入待机模式。 在这种情况下，Web 界面会显示强制 TrueNAS 控制器处于活动状态的选项。

要让系统在网络超时期间等待故障转移，请将 0 替换为新的秒数。

**注意：除非 iXsystems 支持工程师指导，否则不要同步 TrueNAS 配置！ TrueNAS 旨在自动同步系统配置，手动同步选项仅适用于危险或高风险的故障排除情况。**

