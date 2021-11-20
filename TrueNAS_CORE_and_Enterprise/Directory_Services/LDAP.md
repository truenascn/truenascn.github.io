# 轻型目录访问协议（LDAP）

TrueNAS 包括一个 Open LDAP 客户端，用于从 LDAP 服务器访问信息。 LDAP 服务器提供用于查找网络资源（例如用户及其相关权限）的目录服务。

**Q**：LDAP 是否适用于 SMB？

**A**：SMB 共享的 LDAP 身份验证被禁用，除非已为 LDAP 目录配置并填充了 Samba 属性。 执行此任务最流行的脚本是 **smbldap-tools**。 LDAP 服务器必须支持 SSL/TLS，并且必须导入 LDAP 服务器 CA 的证书。 当前不支持非 CA 证书。

## 基本配置

要将 LDAP 服务器与 TrueNAS 集成，请转至目录服务（**Directory Services**） > **LDAP**。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesLDAP.png)

输入 LDAP 服务器主机名或 IP 地址。 用空格分隔条目。 输入多个主机名或 IP 地址会创建 LDAP 故障转移优先级列表。

**Q**：这有什么作用？

**A**：如果当前主机没有响应，则尝试列表中的下一个主机，直到建立新连接。

输入基本 DN（**Base DN**）。 这是搜索资源时要使用的 LDAP 目录树的顶层。 例如，dc=test,dc=org。

输入绑定 DN（**Bind DN**）。 这是 LDAP 服务器上的管理帐户名称。 例如，cn=Manager,dc=test,dc=org。

接下来，输入绑定密码（**Bind Password**）。 这是与绑定 DN 帐户关联的密码。

最后一个基本选项是启用（**Enable**）。 取消勾选可以禁用 LDAP 配置而不删除它。 它可以在以后启用而无需重新配置选项。

## 高级配置

要进一步修改 LDAP 配置，请单击高级选项（**ADVANCED OPTIONS**）。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesLDAPAdvanced.png)

设置允许匿名绑定（**Allow Anonymous Binding**）会禁用身份验证并允许对任何客户端进行读写访问。

如果 [Kerberos](https://www.truenas.com/docs/core/directoryservices/kerberos/) 领域已添加到 TrueNAS，则可以从 Kerberos 领域（**Kerberos Realm**）下拉列表中进行选择。 同样，如果已创建 Kerberos 密钥表，请在 **Kerberos Principal** 下拉列表中选择它。

如果需要 LDAP 连接的加密模式，请从加密模式（**Encryption Mode**）下拉列表中选择以下选项之一：

- 不加密（**OFF**）: 不加密 LDAP 连接。
- 启用加密（**ON**）: 在端口 636 上使用 SSL 加密 LDAP 连接。
- 使用START TLS加密（**START_TLS**）: 在默认 LDAP 端口 *389* 上使用 STARTTLS 加密 LDAP 连接。

使用用户名/密码或 Kerberos 身份验证时不需要证书。如果需要证书身份验证，请从证书（**Certificate**）下拉列表中选择要使用的证书。要配置基于 LDAP 证书的身份验证，请为 LDAP 提供程序[创建证书签名请求](https://www.truenas.com/docs/core/system/certificates/)以进行签名。

要验证证书的真实性，请勾选验证证书（**Validate Certificates**）。

设置禁用 LDAP 用户/组缓存（**Disable LDAP User/Group Cache**）以禁用在大型 LDAP 环境中缓存 LDAP 用户和组。禁用缓存后，LDAP 用户和组不会出现在下拉菜单中，但在手动输入时仍会被接受。

如果 Kerberos 票证查询在默认时间内没有响应，则增加 LDAP 超时（**LDAP timeout**）。 LDAP 超时以秒为单位。如果 DNS 查询响应时间过长，则增加 DNS 超时（**DNS timeout**）。 DNS 超时以秒为单位。

Samba Schema 在 [Samba 4.13.0](https://www.samba.org/samba/history/samba-4.13.0.html) 中已弃用。如果需要 SMB 共享的 LDAP 身份验证并且 LDAP 服务器已经配置了 Samba 属性，则勾选Samba 架构（**Samba Schema**）。如果设置了Samba 架构（**Samba Schema**），请从架构下拉列表中选择架构类型。

可以为 [nslcd.conf](https://arthurdejong.org/nss-pam-ldapd/nslcd.conf.5) 指定辅助参数。