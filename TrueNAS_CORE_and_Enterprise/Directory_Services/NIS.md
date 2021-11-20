# 网络信息服务（NIS）

NIS（网络信息服务）是一种客户端-服务器型目录服务协议，用于在计算机网络上的计算机之间分发系统配置数据，例如用户名和主机名。

**Q**：这究竟是做什么的？

**A**：NIS 系统在计算机网络中维护和分发用户和组信息、主机名、电子邮件别名和其他基于文本的信息表的中央目录。 在 FreeBSD 中，用户列表放在 /etc/passwd 中，身份验证哈希放在 /etc/shadow 中。 NIS 添加了另一个“全局”用户列表来标识任何 NIS 域客户端上的用户。

**注意：NIS 在可扩展性和安全性方面受到限制。 对于现代网络，[LDAP](https://www.truenas.com/docs/core/directoryservices/ldap/) 已取代 NIS。**

要配置 NIS，请转至目录服务（**Directory Services**） > 网络信息服务（NIS）。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesNIS.png)

输入 NIS 域（**NIS Domain**）名称并列出所有 NIS 服务器（**NIS Servers**）（主机名或 IP 地址）。 按 Enter 分隔服务器条目。 根据需要配置其余选项：

- 安全模式（**Secure Mode**） : 设置为 [ypbind(8)](https://www.freebsd.org/cgi/man.cgi?query=ypbind) 拒绝绑定到任何未在 TCP 端口1024上以 *root* 身份运行的 NIS 服务器。
- 任播（**Manycast**） : 设置为`ypbind` 以绑定到响应最快的服务器。
- 启用（**Enable**） : 取消勾选以禁用配置而不删除它。

