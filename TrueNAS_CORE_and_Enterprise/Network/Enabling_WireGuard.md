# 开启WireGuard

WireGuard 因其速度、简单性和现代加密标准而成为 VPN 市场中的流行选择。 从 FreeNAS 版本 11.3-RC1 开始，一直到 TrueNAS 12.0，可以通过几个简单的步骤将您的 NAS 直接连接到 WireGuard 网络。

首先创建一些自定义可调参数来启用服务并为其提供默认接口。 登录 TrueNAS Web 界面并转到系统（**System**）> 可调参数（**Tunables**） > 添加（**Add**）。 使用这些设置来启用服务：

- 变量名（**Variable**） = `wireguard_enable`
- 值（**Value**） = `YES`
- 类型（**Type**） = `rc.conf`

![](https://www.truenas.com/docs/images/CORE/12.0/wireguard_enable.png)

接下来，创建另一个可调参数来定义网络接口：

- 变量名（**Variable**） = `wireguard_interfaces`
- 值（**Value**） = `wg0`
- 类型（**Type**） = `rc.conf`

![](https://www.truenas.com/docs/images/CORE/12.0/wireguard_interfaces.png)

完成后，您将设置并启用这两个变量：

![](https://www.truenas.com/docs/images/CORE/12.0/wireguard_variables.png)

接下来，我们需要一个 post-init 脚本，在启动时将 WireGuard 配置放置在正确的位置。 转至“任务”（**Tasks**）>“开/关机脚本”（**Init/Shutdown Scripts**），然后单击添加（**Add**）。 配置脚本以在每次系统启动时加载 WireGuard .conf 文件：

- 类型（**Type**） = `Command`
- 命令（**Command**） = `mkdir -p /usr/local/etc/wireguard && cp /root/wg0.conf /usr/local/etc/wireguard/wg0.conf && /usr/local/etc/rc.d/wireguard start`
- 何时（**When**） = `Post Init`

您可以配置 /root/wg0.conf 文件并应用 WireGuard 配置以附加到您定义的任何 WireGuard 网络。 它可以是任何运行 WireGuard 的单点对点，甚至可以使用完整路由。 示例用例是：

- 从远程笔记本电脑访问 NAS 上的数据
- 将 NAS 连接到 NAS 以进行复制
- 将托管 NAS 连接到远程网络
- 从您的智能手机访问您的 NAS

![](https://www.truenas.com/docs/images/CORE/12.0/WireguardInitScript.png)

现在创建 /root/wg0.conf ，其中包含要在启动时应用的特定 WireGuard 配置。 此文件设置取决于您的特定网络环境和要求，这超出了本文的范围。 这里提供了[快速入门指南](https://www.wireguard.com/quickstart/)和[教程](https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/)以及内置的 wg-quick 联机帮助页。

一旦你有了一个有效的 /root/wg0.conf，重启系统就会打开 WireGuard 界面，你会在 ifconfig 的输出中看到一个 wg0 设备。

![](https://www.truenas.com/docs/images/CORE/12.0/wg0DeviceOutput.png)

恭喜，您已成功将 TrueNAS 系统链接到安全的 WireGuard 隧道！