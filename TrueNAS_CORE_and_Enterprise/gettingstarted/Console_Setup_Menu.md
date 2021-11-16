# TrueNAS控制台设置菜单

控制台设置菜单在引导过程结束时显示。 如果 TrueNAS 系统有键盘和显示器，此菜单可用于管理系统。

使用 SSH 或 web shell 连接时，默认情况下不显示控制台设置菜单。 它可以由 root 用户或其他具有 root 权限的用户通过输入 `/etc/netcli` 来启动。

要禁用控制台设置菜单，请转至系统(System) > 高级(Advanced)并取消设置不带密码提示的显示文本控制台(Show Text Console without Password Prompt)。

![](https://www.truenas.com/docs/images/CORE/ConsoleSetupMenu.png)

在 HA 系统上，除非已通过管理方式禁用 HA，否则其中一些菜单选项不可用。

菜单提供以下选项：

1、配置网络接口（**Configure Network Interfaces**）提供了一个配置向导来设置系统的网络接口。如果系统已获得高可用性 (HA) 许可，向导会提示输入“此控制器”和“TrueNAS 控制器 2”的 IP 地址。

2、配置链路聚合（**Configure Link Aggregation**）用于创建或删除链路聚合。

3、配置虚拟局域网（**Configure VLAN Interface**） 用于创建或删除 VLAN 接口。

4、配置默认路由器（**Configure Default Route **）用于设置 IPv4 或 IPv6 默认网关。出现提示时，输入默认网关的 IP 地址。

5、配置静态路由（**Configure Static Routes**）会提示输入目标网络和网关 IP 地址。为每个需要的静态路由重新输入此选项。

6、配置 DNS（**Configure DNS**） 提示输入 DNS 域的名称和第一个 DNS 服务器的 IP 地址。添加多个DNS服务器时，按回车键进入下一个。按 Enter 两次退出此选项。

7、重置root密码（**Reset Root Password**）用于重置丢失或忘记的根密码。选择此选项并按照提示设置密码。

8、将所有配置重置为默认值（**Reset Configuration to Defaults**） 注意！此选项会删除在管理 GUI 中进行的所有配置设置，并用于将 TrueNAS® 重置为默认值。在选择此选项之前，请对所有数据进行完整备份，并确保所有加密密钥和密码都是已知的！选择此选项后，配置将重置为默认值并重新启动系统。存储➞ 池➞ 导入池可用于重新导入池。

9、终端（**Shell**） 启动一个 shell 来运行 FreeBSD 命令。要离开shell，请键入 exit。

10、重新启动（**Reboot** ）会重新启动系统。

11、关机（**Shut Down **）关闭系统。

此菜单上的选项编号和数量可能会因软件更新、服务协议或其他因素而改变。请在选择选项之前仔细检查菜单，并在编写本地程序时牢记这一点。

在启动过程中，TrueNAS 会自动尝试从所有实时接口连接到 DHCP 服务器。如果成功接收到 IP 地址，则会显示该地址，以便可以使用它访问图形用户界面。在上面显示的示例中，TrueNAS 可在 10.0.0.102 访问。

某些 TrueNAS 系统设置没有监视器，因此很难确定分配了哪个 IP 地址。在支持多播 DNS (mDNS) 的网络上，可以在浏览器的地址栏中输入主机名和域。默认情况下，此值为 truenas.local。

如果 TrueNAS 未通过 DHCP 服务器连接到网络，请使用控制台网络配置菜单手动配置接口，如下所示。在这个例子中，TrueNAS 系统有一个网络接口，`em0`。

`

```
1) em0
Select an interface (q to quit): 1
Remove the current settings of this interface? (This causes a momentary disconnec
tion of the network.) (y/n) n
Configure interface for DHCP? (y/n) n
Configure IPv4? (y/n) y
Interface name:     (press enter, the name can be blank)
Several input formats are supported
Example 1 CIDR Notation:
 192.168.1.1/24
Example 2 IP and Netmask separate:
 IP: 192.168.1.1
 Netmask: 255.255.255.0, or /24 or 24
IPv4 Address: 192.168.1.108/24
`Saving interface configuration: Ok
Configure IPv6? (y/n) n
Restarting network: ok
...
The web user interface is at
http://192.168.1.108


```

