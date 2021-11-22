# 安全外壳协议（SSH）

SSH 服务允许使用[安全外壳传输层协议](https://tools.ietf.org/html/rfc4253)连接到 TrueNAS。 当TrueNAS用作SSH服务器时，网络中的用户可以使用[SSH客户端软件](https://www.bing.com/search?q=SSH%20client%20software)通过SSH传输文件。

**注意：允许外部连接到 TrueNAS 是一个安全漏洞！ 除非需要外部连接，否则不要启用 SSH。**

在“服务”（ **Services**）页面上激活或配置 SSH 服务。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesDefaults.png)

单击切换按钮启动或停止服务，具体取决于当前状态。 设置自动启动（**Start Automatically**）以使服务在 TrueNAS 启动时启动。

要配置 SSH，请禁用该服务并单击编辑图标 。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSSHOptions.png)

根据需要配置选项以匹配您的网络环境。

## SSH 服务选项

### 通用选项（General Options）

| 设置选项                                                   | 描述                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| TCP端口（**TCP Port**）                                    | 为 SSH 连接请求打开一个端口。                                |
| 允许root用户以密码登陆（**Log in as Root with Password**） | 不鼓励 root 登录。 允许 root 登录。 必须为 root 用户帐户设置密码。 |
| 允许使用密码验证（**Allow Password Authentication**）      | 启用允许使用密码进行 SSH 登录身份验证。 警告：启用目录服务时，此设置会授予所有用户访问目录服务导入的权限。 禁用时，身份验证需要所有用户的密钥（需要额外的 SSH 客户端和服务器设置）。 |
| 允许使用Kerberos验证（**Allow Kerberos Authentication**）  | 在启用之前，请确保 **Directory Services**（Kerberos Realms 和 Keytabs）中存在有效条目，并且系统可以与 Kerberos 域控制器通信。 |
| 允许 TCP 端口转发（**Allow TCP Port Forwarding**）         | 设置为让用户使用 SSH 端口 [转发功能](https://www.symantec.com/connect/articles/ssh-port-forwarding) 绕过防火墙限制。 |

### 高级选项（Advanced Options）

| 设置选项                               | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| 网络接口绑定（**Bind Interfaces**）    | 选择 SSH 侦听的网络接口。 不选择所有选项以让 SSH 侦听所有网络接口。 |
| 数据压缩（**Compress Connections**）   | 选择 SFTP 服务器的数据压缩级别。                             |
| SFTP日志等级（**SFTP Log Level**）     | 选择 SFTP 服务器的 syslog(3) 级别。                          |
| SFTP 日志工具（**SFTP Log Facility**） | 选择 SFTP 服务器的 [syslog(3)](https://www.freebsd.org/cgi/man.cgi?query=syslog) 工具。 |
| 弱密码（**Weak Ciphers**）             | 警告：这些密码是安全漏洞。 只允许它们处于安全的网络环境中。  |
| 辅助参数（**Auxiliary Parameters**）   | 添加更多 [sshd_config(5)](https://www.freebsd.org/cgi/man.cgi?query=sshd_config) 未在此屏幕中涵盖的选项。 每行输入一个选项。 这些选项区分大小写。 拼写错误会阻止 SSH 服务启动。 |

远程系统可能需要使用root账户对系统进行访问，但在允许 root 访问之前应采取所有安全预防措施。
对于 SSH 服务，还有一些额外的选项建议：

- 将 `NoneEnabled no` 添加到辅助参数（**Auxiliary Parameters**）以禁用不安全的 none 密码。
- 如果 SSH 连接总是断开，则增加 ClientAliveInterval。
- ClientMaxStartup 默认为 10。当需要更多并发 SSH 连接时增加此值。

完成所有配置更改后，不要忘记在“服务”（ **Services**）页面上重新启用 SSH 服务。 要创建和存储特定的 [SSH 连接和密钥对](https://www.truenas.com/docs/core/system/systemssh/)，请转到系统（**System**）菜单部分。

## 高级：将命令行用户限制为 scp 或 sftp

这仅适用于使用命令行版本的 scp 和 sftp 的用户。 当配置了 SSH 时，经过身份验证的用户可以使用 ssh 通过网络登录到 TrueNAS 系统。 通过转至帐户（**Accounts**） > 用户（**Users**）并单击添加（**ADD**）来创建用户帐户。

默认情况下，用户在使用 SSH 登录后会看到他们的主目录。 但是，用户仍然可以在其主目录之外找到系统位置，因此在授予用户对系统的 SSH 访问权限之前采取安全预防措施。 提高安全性的一种方法是将用户的 shell 更改为仅允许文件传输。 这允许用户使用 scp 和 sftp 在他们的本地计算机和他们在 TrueNAS 系统上的主目录之间传输文件，同时限制他们使用 ssh 登录系统。

要配置此方案，请转至帐户（**Accounts**） > 用户（**Users**）并编辑所需的用户帐户。 将**Shell**更改为 scponly。 对需要受限 SSH 访问的每个用户重复此操作。

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsUsersEditShellScponly.png)

通过以该用户帐户运行 sftp、ssh 和 scp 命令来测试来自另一个系统的配置。 sftp 和 scp 将工作，但 ssh 将失败。

