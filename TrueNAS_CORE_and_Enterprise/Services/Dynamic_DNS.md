# 动态域名解析（DDNS）

当 TrueNAS 连接到定期更改系统 IP 地址的 ISP 时，[动态域名服务 (DDNS)](https://tools.ietf.org/html/rfc2136) 非常有用。 使用动态 DNS，系统可以自动将其当前 IP 地址与域名相关联，即使系统 IP 地址发生变化，也可以继续提供对 TrueNAS 的访问。

## 配置动态 DNS

DDNS 需要在配置 TrueNAS 之前注册 DDNS 服务，例如 DynDNS。 配置 TrueNAS 时，让 DDNS 服务设置可用或在另一个浏览器选项卡中打开。 登录 TrueNAS 网页界面并进入服务（**Services**） > 动态 DNS（**Dynamic DNS**）。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesDynamicDNSOptions.png)

### 常规选项

| 设置选项                                | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| 提供商（**Provider**）                  | 支持多个提供程序。 如果未列出特定提供程序，请选择自定义提供程序并在自定义服务器和自定义路径字段中输入信息。 |
| 使用SSL检测IP（**CheckIP-Server SSL**） | 使用 HTTPS 连接到 CheckIP 服务器。                           |
| IP检查服务器（**CheckIP Server**）      | 报告外部 IP 地址的服务器的名称和端口。 例如，输入 checkip.dyndns.org:80 使用 [Dyn IP detection](https://help.dyn.com/remote-access-api/checkip-tool/)。 发现远程套接字IP地址。 |
| IP检查路径（**CheckIP Path**）          | CheckIP 服务器的路径。 例如，`no-ip.com` 使用 `dynamic.zoneedit.com` 的 CheckIP Server 和 /checkip.html 的 CheckIP Path。 |
| 启用SSL（**SSL**）                      | 使用 HTTPS 连接到更新 DNS 记录的服务器。                     |
| 域名（**Domain Name**）                 | 具有动态 IP 地址的主机的完全限定域名。 使用空格、逗号 (,) 或分号 (;) 分隔多个域。 示例：`myname.dyndns.org; myothername.dyndns.org`。 |
| 更新周期（**Update Period**）           | 以秒为单位检查 IP 的频率。                                   |

### 凭据

| 设置选项               | 描述                                 |
| ---------------------- | ------------------------------------ |
| 用户名（**Username**） | 用于登录提供程序并更新记录的用户名。 |
| 密码（**Password**）   | 登录提供者和更新记录的密码。         |

