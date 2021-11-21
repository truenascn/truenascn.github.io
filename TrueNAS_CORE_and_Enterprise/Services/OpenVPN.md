# OpenVPN

虚拟专用网络 (VPN) 是专用网络在公共资源上的扩展。 它允许客户端安全地连接到专用网络，即使他们远程使用公共网络。 TrueNAS 提供 [OpenVPN](https://openvpn.net/) 作为系统级服务，以提供 VPN 服务器或客户端功能。 这意味着 TrueNAS 可以充当主要 VPN 服务器，允许远程客户端使用单个 TCP 或 UDP 端口访问存储在系统上的数据。 或者，即使系统位于单独的物理位置或只能访问公开可见的网络，TrueNAS 也可以集成到专用网络中。

在将 TrueNAS 配置为 OpenVPN 服务器或客户端之前，您需要一个现有的公钥基础设施 (PKI)，其中包含在 TrueNAS 中创建或导入的证书和[证书颁发机构](https://www.truenas.com/docs/core/system/cas/)。

**Q**：这有什么作用？

**A**：这允许 TrueNAS 通过确认网络凭据由有效的主证书颁发机构 (CA) 签名来与客户端或服务器进行身份验证。 要了解有关 OpenVPN 所需 PKI 的更多信息，请参阅 [OpenVPN PKI 概述](https://community.openvpn.net/openvpn/wiki/HOWTO?__cf_chl_jschl_tk__=92022277e38bff707b1684f49a2af61f5eb4c632-1605712222-0-AQxKxUAlHKMcfHHNdSMOLL25Lr3e8icKHu3CgjMFRe6GXS1Z72EgXMieNrGaBdWa0m3R5CEZcxwGdwhgaRO392FTivdOQis5Pa2Bm-4jEzydUBTqhx_F4XWN7ujVee5CUxG6AoyOet91SaWM-siqV0_d0ppGnSsfwX9HFOmKuAnJexAjqpofUlP6xjru4Qujw72uR-yUT3fuFDMyukAAtEAP_zPXtewdS_kcSC5eSdf-RC6V8T_QZ2UT6GfqxxSr5shwe0rFkNinTCOKLk_67UIU2zEkpuiQ8C7p3ysh1DS_ONAzR2pfwdgetKm3HiBJ38C86956W6D8-mpOulfP26E#Overview)。

在 TrueNAS 上配置 OpenVPN（服务器或客户端）的一般过程是选择网络凭据、设置连接详细信息并选择任何其他安全或协议选项。

## OpenVPN客户端

转到服务（**Services**）页面并找到 OpenVPN 客户端（**OpenVPN Client**）条目。 单击编辑图标以配置服务。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesOpenVPNClientOptions.png)

选择要用作 OpenVPN 客户端的证书。 此证书必须存在于 TrueNAS 中并且处于活动（未撤销）状态。 输入远程 OpenVPN 服务器的主机名或 IP 地址。

继续检查并选择适合您的网络环境和性能要求的任何其他[连接设置](https://www.truenas.com/docs/core/services/openvpn/#connection-settings)。 设备类型（**Device Type**）必须与 OpenVPN 服务器设备类型匹配。 **Nobind** 防止客户端使用固定端口。 默认情况下，启用此选项以允许 OpenVPN 客户端和服务器同时运行。

最后，查看[安全选项](https://www.truenas.com/docs/core/services/openvpn/#security-options)并选择满足您的网络安全要求的设置。 当 OpenVPN 服务器使用 TLS 加密时，复制静态 TLS 加密密钥并将其粘贴到 **TLS Crypt Auth** 字段中。

## OpenVPN 服务器

转到服务（**Services**）页面并找到 OpenVPN 服务器（**OpenVPN Server**）条目。 单击编辑图标以配置服务。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesOpenVPNServerOptions.png)

为此 OpenVPN 服务器选择服务器证书（**Server Certificate**）。 该证书必须同时存在于 TrueNAS 系统上并且处于活动（未撤销）状态。

现在为 OpenVPN 服务器定义 IP 地址和网络掩码。 继续选择适合您的网络环境和性能要求的其余连接设置。 选择 TUN 设备类型（**TUN Device Type**）后，您可以为服务器选择虚拟寻址拓扑（**Topology**）：

- *NET30*: 在点对点拓扑中，每个客户端使用一个 */30* 子网。 专为在连接客户端为 Windows 系统时使用而设计。
- *P2P*: 将本地服务器和远程客户端端点相互指向的点对点拓扑。 每个客户端都有一个 IP 地址。 仅当所有客户端都不是 Windows 系统时才建议这样做。
- *SUBNET*:该接口使用 IP 地址和子网。 每个客户端都有一个 IP 地址。 Windows 客户端需要*TAP-Win32 驱动程序* 8.2 或更高版本。 *TAP* 设备始终使用 *SUBNET* *拓扑 *。

拓扑（**Topology**）选择会自动应用于任何连接的客户端。

当 **TLS Crypt Auth Enabled** 被设置时，TrueNAS 会在保存选项后为 **TLS Crypt Auth** 字段生成一个静态密钥。 要更改此密钥，请单击 重新设置静态密钥（**RENEW STATIC KEY**）。 连接到服务器的任何客户端都需要此密钥。 密钥存储在系统数据库中并自动包含在生成的客户端配置文件中，但一个好的做法是将密钥备份在安全位置。

最后，检查[安全选项](https://www.truenas.com/docs/core/services/openvpn/#security-options)并选择满足您的网络安全要求的设置。

配置并保存您的 OpenVPN 服务器后，生成客户端配置文件以导入到连接到此服务器的任何 OpenVPN 客户端系统。 您需要来自系统上已导入的客户端系统的证书。 要生成配置文件，请单击“下载客户端配置”（**DOWNLOAD CLIENT CONFIG**）并选择“客户端证书”（**Client Certificate**）。

## 通用选项（客户端或服务器）

许多用于配置 OpenVPN 服务器或客户端的字段是相同的。 本节将讨论这些字段，并在“[服务器](https://www.truenas.com/docs/core/services/openvpn/#openvpn-server)”（**Server**）和“[客户端](https://www.truenas.com/docs/core/services/openvpn/#openvpn-client)”(**Client**)部分列出了特定的配置选项。

附加参数字段手动设置任何核心 OpenVPN 配置文件选项。 有关每个选项的说明，请参阅 OpenVPN [参考手册](https://openvpn.net/community-resources/reference-manual-for-openvpn-2-4/)。

### 连接设置（Connection Settings）

- 根证书（**Root CA**）: 证书颁发机构 (CA) 必须是用于签署客户端和服务器证书的根 CA。
- 端口（**Port**）: 这是将用于 OpenVPN 连接的端口。
- 压缩（**Compression**）: 为流量选择压缩算法。 将该字段留空以发送未压缩的数据。 *LZO* 是一种标准压缩算法，向后兼容以前（2.4 之前）版本的 OpenVPN。 *LZ4* 是较新的选项，通常速度更快，所需的系统资源更少。
- 协议（**Protocol**）: 在 OpenVPN 的 *UDP* 或 *TCP* 协议之间进行选择。 *UDP* 以连续流的形式发送数据包，而 *TCP* 则按顺序发送数据包。 UDP 通常比 TCP 更快，并且对丢弃的数据包的严格程度较低。 要强制连接为 IPv4 或 IPv6，请选择 `4` 或 `6` *UDP* 或 *TCP* 选项之一。
- 设备类型（**Device Type**）: 使用 *TUN* 或 *TAP* 虚拟网络设备和 OpenVPN 层。 这在 OpenVPN 服务器和任何客户端之间必须相同。

### 安全选项（Security Options）

因为使用 VPN 涉及连接到专用网络，同时仍然通过不太安全的公共资源发送数据，所以 OpenVPN 包括几个安全选项。 虽然不是必需的，但这些安全选项有助于保护传入或传出专用网络的数据。

- 认证算法（**Authentication Algorithm**）: 这用于验证通过网络连接发送的数据包。 您的网络环境可能需要特定的算法。 如果不需要特定算法，*SHA1 HMAC* 是一个很好的标准算法。
- 密码（**Cipher**）: 这是一种加密通过连接发送的数据包的算法。 虽然不是必需的，但选择 *Cipher* 可以提高连接安全性。 您可能需要验证您的网络环境需要哪些密码。 如果没有特定的密码要求，*AES-256-GCM* 是一个不错的默认选择。
- TLS加密（**TLS Encryption**）: 设置 *TLS Crypt Auth Enabled* 后，所有 TLS 握手消息都会加密以增加另一层安全性。 这需要在 OpenVPN 服务器和客户端之间共享的静态密钥。

## 激活服务

完成服务器或客户端服务的配置后，单击“保存”（**SAVE**）。 通过单击服务（**Services**）页面中的相关切换来启动服务。 要检查服务的当前状态，请将鼠标悬停在切换按钮上。

设置自动启动（**Start Automatically**）意味着只要 TrueNAS 完成启动并且网络和数据池正在运行，服务就会启动。
