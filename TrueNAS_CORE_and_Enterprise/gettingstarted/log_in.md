# 登陆到TrueNAS

现在您已经安装好了了 TrueNAS，是时候登录 Web 界面并开始管理数据了！

| 我可以使用 CLI 配置 TrueNAS 吗？                             |
| ------------------------------------------------------------ |
| 安装TrueNAS后，系统的配置和使用全部通过Web界面进行管理。 仅使用 Web 界面对系统进行配置更改非常重要。 默认情况下，使用命令行界面 (CLI) 修改系统不会修改设置数据库。 每当系统重新启动时，在命令行中所做的任何更改都会丢失并恢复为原始数据库设置。 TrueNAS 会自动创建多种访问 Web 界面的方式，但您可能需要调整默认设置以更好地适应网络环境中的系统。 |

## 获取网络界面（WebUI）地址

### CORE版默认设置

打开 TrueNAS 系统电源时，系统会尝试从所有实时接口连接到 DHCP 服务器并提供对 Web 界面的访问。 在支持多播域名服务 (mDNS) 的网络上，主机名和域可用于访问 TrueNAS Web 界面。 默认情况下，TrueNAS 配置为使用主机名和域 truenas.local 您可以在登录 Web 界面后通过转到网络（**Network**） > 全局配置（**Global Configuration**）并设置新的主机名（**Hostname**）和域（**Domain**）来更改此设置。

如果需要 IP 地址，请将显示器连接到 TrueNAS 系统并查看引导过程结束时显示的控制台设置菜单。

![](https://www.truenas.com/docs/images/CORE/ConsoleSetupMenu.png)

当能够自动配置连接时，系统会在控制台设置菜单底部显示 Web 界面 IP 地址。 如果需要，您可以在 TrueNAS 控制台设置菜单中或通过单击 Web 界面中的设置（**Settings**） > 更改密码（**Change Password**）来重置 root 密码。 要在显示系统控制台菜单之前要求登录系统，请转至系统（**System**） > 高级（**Advanced**）并取消设置不带密码提示的显示文本控制台（*Show Text Console without Password Prompt*.）。

### Enterprise版默认设置

iXsystems 的 [TrueNAS Enterprise 硬件](https://www.truenas.com/docs/hardware/)已使用您提供的网络详细信息进行预配置。 TrueNAS 网络界面的 IP 地址在系统销售订单或配置表上提供。 如果 TrueNAS 网络界面 IP 地址未随这些文档提供或无法从 TrueNAS 系统控制台识别，请联系 iX 支持。

| 联系方式 | 联系信息                                                     |
| -------- | ------------------------------------------------------------ |
| 网站     | [https://support.ixsystems.com](https://support.ixsystems.com/) |
| 电子邮件 | [support@ixsystems.com](mailto://support.ixsystems.com)      |
| 电话     | 周一 - 周五, 上午6:00 至 下午 6:00 太平洋标准时间：1-855-473-7449 按2（仅限美国）；1-408-943-4100 按·2（国际） |
| 电话     | 7x24小时在线：1-855-499-5131（仅限美国）；1-408-878-3140（国际） |

### 配置 Web 界面访问

如果 TrueNAS 系统未通过 DHCP 服务器连接到网络，您可以使用控制台网络配置菜单手动配置网络接口。

![](https://www.truenas.com/docs/images/CORE/ConsoleSetupMenu.png)

如果 TrueNAS 未通过 DHCP 服务器连接到网络，请使用控制台网络配置菜单手动配置接口，如下所示。在这个例子中，TrueNAS 系统有一个网络接口，`em0`。

`1) em0`
`Select an interface (q to quit): 1`
`Remove the current settings of this interface? (This causes a momentary disconnec`
`tion of the network.) (y/n) n`
`Configure interface for DHCP? (y/n) n`
`Configure IPv4? (y/n) y`
`Interface name:     (press enter, the name can be blank)`
`Several input formats are supported`
`Example 1 CIDR Notation:`
 `192.168.1.1/24`
`Example 2 IP and Netmask separate:`
 `IP: 192.168.1.1`
 `Netmask: 255.255.255.0, or /24 or 24`
`IPv4 Address: 192.168.1.108/24`
`Saving interface configuration: Ok`
`Configure IPv6? (y/n) n`
`Restarting network: ok`
`...`
`The web user interface is at`
`http://192.168.1.108`

根据网络环境，查看配置默认路由选项以定义您的 IPv4 或 IPv6 默认网关。 配置静态路由允许添加目标网络和网关 IP 地址，每个路由一个。 要更改 DNS 域并添加名称服务器，请选择配置 DNS（**Configure DNS**)。

稍后可以在 Web 界面中可用的各种网络（**Network**）选项中调整这些设置。

## 登陆

在可以访问与 TrueNAS 系统相同的网络的计算机上，在 Web 浏览器中输入主机名和域或 IP 地址以连接到 Web 界面。

![](https://www.truenas.com/docs/images/CORE/12.0/LoginCORE.png)

只有 root 用户可以登录 Web 界面。 输入在安装过程中创建的 root 帐户密码。

### 故障排除

如果无法通过浏览器的 IP 地址访问用户界面，请检查以下事项：

1、浏览器配置中是否启用了代理设置？ 如果是这样，请禁用设置并再次尝试连接。
2、如果页面未加载，请确保可以 ping 通 TrueNAS 系统的 IP 地址。 如果地址在专用 IP 地址范围内，则只能从该专用网络内访问它。

如果显示 Web 界面但似乎没有响应或不完整：

1、确保浏览器允许来自 TrueNAS 系统的 cookie、Javascript 和自定义字体。
2、尝试不同的浏览器。 推荐使用火狐浏览器。

如果在升级或其他系统操作后 UI 变得无响应，请清除站点数据并刷新浏览器 (Shift+F5)。

## 仪表盘

登录后，显示系统仪表板（**Dashboard**）。 系统版本、系统组件使用情况和网络流量的基本信息都显示在此屏幕上。 对于具有兼容 TrueNAS 硬件的用户，单击系统图像将带您进入系统（**System**） > 查看附件（**View Enclosure**）页面。

![](https://www.truenas.com/docs/images/CORE/12.0/DashboardCORE.png)

仪表板提供对所有 TrueNAS 管理选项的访问。 顶行是指向外部资源的链接和用于控制系统的按钮。 屏幕左侧还有一列选项，用于访问各种 TrueNAS 配置。

顶行中的徽标是指向 iXsystems 站点的链接。 iXsystems 徽标旁边的按钮显示 [TrueCommand](https://www.truenas.com/truecommand/) 连接选项。 接下来的两个按钮显示有关系统上正在发生的事情的信息，例如活动或以前的任务以及系统条件生成的任何警报。 其余按钮链接到系统配置选项或可用于注销、重新启动或关闭物理系统。

顶行有用于隐藏左侧列的按钮。 此列的顶部显示系统主机名和活动用户。 TrueNAS 配置屏幕链接在左侧栏中。

现在您可以访问 TrueNAS 网络界面并查看所有管理选项，是时候开始[存储数据](https://www.truenas.com/docs/core/gettingstarted/storingdata/)了！

