# 文件传输协议 (FTP) 

[文件传输协议 (FTP)](https://tools.ietf.org/html/rfc959) 是一种用于数据传输的简单选项。 额外的 SSH 和 Trivial FTP 选项分别提供安全或简单的配置文件传输方法。

用于配置 FTP、SSH 和 TFTP 的选项位于系统服务（**Services**）中。 单击编辑以配置相关服务。

## FTP

FTP 需要新的数据集和本地用户帐户。

转到存储（**Storage**） > 池（**Pools**）以添加新数据集。

接下来，转到“帐户”（**Accounts**）>“用户”（**Users**）>“添加”（**Add**）以在 TrueNAS 上创建本地用户。

分配用户名、密码，并将新创建的 FTP 共享数据集链接为用户的主目录。 这可以在每个用户的基础页面上完成，也可以创建一个全局的 FTP 帐户，例如 OurOrgFTPacnt 等。

返回存储（**Storage**） > 池（**Pools**），找到新数据集，然后单击 右边的三个小点（选项） > 编辑权限（**Edit Permissions**）。 将所有者（**Owner**）字段（用户和组）设置为新创建的用户帐户。 保存前请务必单击“应用用户”（**Apply User**）和“应用组”（**Apply Group**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEditPermissionsBasic.png)

### 服务配置

要配置 FTP，请转到“服务”（**Services**）页面，找到 FTP 条目，然后单击编辑。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesFTPOptions.png)

根据您的环境和安全考虑配置选项。

#### 通用选项

| 设置选项                           | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| 端口（**Port**）                   | 设置 FTP 服务侦听的端口。                                    |
| 客户端连接数（**Clients**）        | 并发客户端的最大数量。                                       |
| 单IP连接数（**Connections**）      | 设置每个 IP 地址的最大连接数。 0 是无限的。                  |
| 登录尝试次数（**Login Attempts**） | 输入客户端断开连接前的最大尝试次数。 如果用户容易打错字，则增加。 |
| 超时（**Timeout**）                | 断开连接前的最大客户端空闲时间（以秒为单位）。               |
| 证书（**Certificate**）            | 用于 TLS FTP 连接的 SSL 证书。 要创建证书，请转到证书（**Certificates**）。 |

#### 高级选项

##### 连接（Access）

| 设置选项                                         | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| 主目录访问锁定（**Always Chroot**）              | 设置为只允许用户访问他们的主目录，如果他们在轮组中。 此选项会增加安全风险。 |
| 允许root账户登陆（**Allow Root Login**）         | 允许root账户登陆FTP                                          |
| 允许匿名登陆（**Allow Anonymous Login**）        | 允许匿名 FTP 登录并访问 路径（**Path**） 中指定的目录。      |
| 允许本地用户登陆（**Allow Local User Login**）   | 允许任何本地用户登录。默认情况下，只允许 *ftp* 组的成员登录。 |
| 需要身份验证（**Require IDENT Authentication**） | 当 `identd` 没有在客户端运行时，设置这个选项会导致超时。     |
| 文件权限（**File Permissions**）                 | 为新创建的目录/目录设置默认权限。                            |

##### TLS

| 设置选项                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 启用TLS（**Enable TLS**）                                    | 允许加密连接。 需要证书（在**证书**（**Certificates**）中创建或导入。 |
| TLS政策（**TLS Policy**）                                    | 定义 FTP 会话的控制通道、数据通道、两个通道还是两个通道都必须通过 SSL/TLS 。 [此处](http://www.proftpd.org/docs/directives/linked/config_ref_TLSRequired.html) 描述了这些政策。 |
| TLS 允许客户端重新协商（**TLS Allow Client Renegotiations**） | 我们不建议这样做，因为它破坏了安全措施。 有关详细信息，请参阅 [mod_tls](http://www.proftpd.org/docs/contrib/mod_tls.html)。 |
| TLS 允许单点登录（**TLS Allow Dot Login**）                  | 如果设置，TrueNAS 检查用户主目录中包含一个或多个 PEM 编码证书的 .tlslogin 文件。 如果未找到，则会提示用户进行密码验证。 |
| TLS 允许单独用户登陆（**TLS Allow Per User**）               | 如果设置，则允许以未加密的方式发送用户密码。                 |
| 需要 TLS 通用名称（**TLS Common Name Required**）            | 设置后，证书中的通用名称必须与主机的 FQDN 匹配。             |
| TLS 启用诊断（**TLS Enable Diagnostics**）                   | 如果在排除连接故障时设置，则记录更详细。                     |
| TLS 导出证书数据（**TLS Export Certificate Data**）          | 设置为导出证书环境变量。                                     |
| TLS 无证书请求（**TLS No Certificate Request**）             | 设置客户端是否因处理服务器证书请求不当而无法连接。           |
| TLS 无空片段（**TLS No Empty Fragments**）                   | 我们不推荐此选项，因为它绕过了安全机制。                     |
| TLS 无会话重连（**TLS No Session Reuse Required**）          | 此选项会降低连接安全性。 仅当客户端不理解重复使用的 SSL 会话时才使用它。 |
| TLS 导出标准变量（**TLS Export Standard Vars**）             | 如果选中，则设置多个环境变量。                               |
| 需要 TLS DNS 名称（**TLS DNS Name Required**）               | 如果设置，客户端 DNS 名称必须解析为其 IP 地址，并且证书必须包含相同的 DNS 名称。 |
| 需要 TLS IP 地址（**TLS IP Address Required**）              | 如果设置，客户端证书 IP 地址必须与客户端 IP 地址匹配。       |

##### 带宽（Bandwidth）

| 设置选项                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 本地用户上传带宽：（示例：500 KiB、500M、2 TB）（**Local User Upload Bandwidth: (Examples: 500 KiB, 500M, 2 TB) **） | 此字段接受以 KiB 或更大（M、GiB、TB 等）为单位的人类可读输入。 默认 0 KiB 是无限制的。 |
| 本地用户下载带宽（**Local User Download Bandwidth**）        | 此字段接受以 KiB 或更大（M、GiB、TB 等）为单位的人类可读输入。 默认 0 KiB 是无限制的。 |
| 匿名用户上传带宽（**Anonymous User Upload Bandwidth**）      | 此字段接受以 KiB 或更大（M、GiB、TB 等）为单位的人类可读输入。 默认 0 KiB 是无限制的。 |
| 匿名用户下载带宽（**Anonymous User Download Bandwidth**）    | 此字段接受以 KiB 或更大（M、GiB、TB 等）为单位的人类可读输入。 默认 0 KiB 是无限制的。 |

##### 其它选项（Other Options）

| 设置选项                                             | 描述                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| 最小被动端口（**Minimum Passive Port**）             | 由客户端在 PASV 模式下使用。 默认值 0 表示 1023 以上的任何端口。 |
| 最大被动端口（**Maximum Passive Port**）             | 由客户端在 PASV 模式下使用。 默认值 0 表示 1023 以上的任何端口。 |
| 启用 FXP（**Enable FXP**）                           | 启用文件交换协议。 我们不建议这样做，因为它会使服务器容易受到 FTP 反弹攻击。 |
| 允许转接恢复（**Allow Transfer Resumption**）        | 设置为允许 FTP 客户端恢复中断的传输。                        |
| 执行反向 DNS 查找（**Perform Reverse DNS Lookups**） | 对客户端 IP 执行反向 DNS 查找。 如果未配置反向 DNS，则会导致长时间延迟。 |
| 伪装地址（**Masquerade Address**）                   | 公共 IP 地址或主机名。 设置 FTP 客户端是否无法通过 NAT 设备连接。 |
| 显示登陆（**Display Login**）                        | 认证后显示给本地登录用户的消息。 不向匿名登录用户显示。      |
| 辅助参数（**Auxiliary Parameters**）                 | 用于添加额外的 [proftpd(8](https://linux.die.net/man/8/proftpd) 参数。 |

确保主目录访问锁定（**Always Chroot**）已启用，因为这有助于将 FTP 会话限制在本地用户的主目录中并允许本地用户登录。

除非必要，否则不允许匿名或 root 访问。 为提高安全性，请尽可能启用 TLS。 这实际上是 FTPS。 当 FTP 暴露给 WAN 时，请启用 TLS。

### FTP连接

使用浏览器或 FTP 客户端连接到 TrueNAS FTP 共享。 此处的图像使用 FileZilla 显示，这是一个免费选项。

用户名和密码是 TrueNAS 上本地用户帐户的用户名和密码。 默认目录与用户的 /home 目录相同。 连接后，可以创建目录并上传和下载文件。

![](https://www.truenas.com/docs/images/CORE/FilezillaFTPConnect.png)

## SFTP

SFTP 或 SSH 文件传输协议，可通过启用对 TrueNAS 系统的 SSH 远程访问来使用。 SFTP 比标准 FTP 更安全，因为它默认对所有传输应用 SSL 加密。

转到服务（**Services**），找到 **SSH** 条目，然后单击编辑。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSSHOptions.png)

设置允许使用密码验证（**Allow Password Authentication**） 并决定是否需要使用密码以 Root 身份登录。 使用 root 的 SSH 是一个安全漏洞，因为它允许使用终端对 NAS 进行完全远程控制，而不仅仅是 SFTP 传输访问。 查看其余选项并根据您的环境或安全需求进行配置。

### SSH 服务选项

#### 通用选项（General Options）

| 设置选项                                                   | 描述                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| TCP端口（**TCP Port**）                                    | 为 SSH 连接请求打开一个端口。                                |
| 允许root用户以密码登陆（**Log in as Root with Password**） | 不鼓励 root 登录。 允许 root 登录。 必须为 root 用户帐户设置密码。 |
| 允许使用密码验证（**Allow Password Authentication**）      | 启用允许使用密码进行 SSH 登录身份验证。 警告：启用目录服务时，此设置会授予所有用户访问目录服务导入的权限。 禁用时，身份验证需要所有用户的密钥（需要额外的 SSH 客户端和服务器设置）。 |
| 允许使用Kerberos验证（**Allow Kerberos Authentication**）  | 在启用之前，请确保 **Directory Services**（Kerberos Realms 和 Keytabs）中存在有效条目，并且系统可以与 Kerberos 域控制器通信。 |
| 允许 TCP 端口转发（**Allow TCP Port Forwarding**）         | 设置为让用户使用 SSH 端口 [转发功能](https://www.symantec.com/connect/articles/ssh-port-forwarding) 绕过防火墙限制。 |

#### 高级选项（Advanced Options）

| 设置选项                               | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| 网络接口绑定（**Bind Interfaces**）    | 选择 SSH 侦听的网络接口。 不选择所有选项以让 SSH 侦听所有网络接口。 |
| 数据压缩（**Compress Connections**）   | 选择 SFTP 服务器的数据压缩级别。                             |
| SFTP日志等级（**SFTP Log Level**）     | 选择 SFTP 服务器的 syslog(3) 级别。                          |
| SFTP 日志工具（**SFTP Log Facility**） | 选择 SFTP 服务器的 [syslog(3)](https://www.freebsd.org/cgi/man.cgi?query=syslog) 工具。 |
| 弱密码（**Weak Ciphers**）             | 警告：这些密码是安全漏洞。 只允许它们处于安全的网络环境中。  |
| 辅助参数（**Auxiliary Parameters**）   | 添加更多 [sshd_config(5)](https://www.freebsd.org/cgi/man.cgi?query=sshd_config) 未在此屏幕中涵盖的选项。 每行输入一个选项。 这些选项区分大小写。 拼写错误会阻止 SSH 服务启动。 |

### SFTP 连接

与 FTP 设置类似，打开 FileZilla 或其他 FTP 客户端，或命令行。 本文以使用 FileZilla 为例。 使用 FileZilla，输入 SFTP://‘TrueNAS IP’、‘用户名’、‘密码’和端口 22 进行连接。

SFTP 没有主目录访问锁定（**Always Chroot**）。 虽然主目录访问锁定（**Always Chroot**）不是 100% 安全，但缺少主目录访问锁定（**Always Chroot**）允许用户轻松移动到根目录并查看内部系统信息。 如果需要考虑这种级别的访问，带有 TLS 的 FTP 可能是更安全的选择。

### TrueNAS Jail 中的 SFTP

允许 SFTP 访问而不授予对 NAS 其他区域的读取访问权限的另一种方法是设置监狱并启用 SSH。

转到**Jails**> 添加（**Add**）。 为监狱提供一个名称并选择一个目标 FreeBSD 映像。 11.3 用于本指南的目的。

将网络选项设置为 DHCP 或静态 IP，然后确认创建。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsAddNetworking.png)

创建之后，通过单击jail 右侧的展开图标 > 打开jail 菜单。 单击开始（**START**）并打开**Shell**。

与初始 FTP 设置类似，在 jail 中创建一个用户。 输入 `adduser` 并按照提示操作，包括密码和主目录位置。 完成后，Jail会要求确认凭据。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsShellUserAdd.png)

通过编辑 /etc/rc.conf 文件启用 SSH。 根据偏好键入 `vi /etc/rc.conf` 或 `ee /etc/rc.conf`，将 `sshd_enable = "YES"` 添加到文件中，保存并退出。 键入 `service sshd enabled` 以启用该服务（enabled vs start 指示 sshd 是启动一次还是每次重新启动时启动）。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsShellEditRCConf.png)

使用 FTP 客户端（例如 FileZilla），使用 jail IP 地址和用户凭据登录。 与 TrueNAS 上的 SSH 一样，可以浏览到用户主目录之外的其他文件夹和位置，但与直接在 TrueNAS 上运行不同，只有 jail 的组件可用。

![](https://www.truenas.com/docs/images/CORE/FilezillaJailConnectSFTP.png)

## TFTP

普通文件传输协议 (TFTP)（**Trivial File Transfer Protocol (TFTP)**） 是 FTP 的轻量级版本，通常用于在本地环境中的机器（例如路由器）之间传输配置或引导文件。 TFTP 提供了极其有限的一组命令，并且不提供身份验证。

当TrueNAS系统只存储网络设备的镜像和配置文件时，配置并启动TFTP服务。 启动 TFTP 服务会打开 UDP 端口 69。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesTFTPOptions.png)

### 路径（Path）

| 设置选项              | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| 目录（**Directory**） | 浏览到现有目录以用于存储。 某些设备可能需要特定的目录名称。 请查阅该设备的文档以查看是否有任何限制。 |

### 连接（Connection）

| 设置选项               | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| 主机（**Host**）       | 用于 TFTP 传输的默认主机。 输入 IP 地址。 示例：`192.0.2.1`  |
| 端口（**Port**）       | 侦听 TFTP 请求的 UDP 端口号。 示例：`8050`                   |
| 用户名（**Username**） | 选择用于 TFTP 请求的帐户。 此帐户必须具有目录（**Directory**）权限。 |

### 访问（Access）

| 设置选项                          | 描述                                 |
| --------------------------------- | ------------------------------------ |
| 文件权限（**File Permissions**）  | 使用复选框调整文件权限。             |
| 允许新文件（**Allow New Files**） | 设置网络设备何时需要向系统发送文件。 |

### 其它选项（Other Options）

| 设置选项                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 辅助参数（**Auxiliary Parameters**） | 从 [tftpd](https://www.freebsd.org/cgi/man.cgi?query=tftpd) 添加更多选项。 在每一行添加一个选项。 |

