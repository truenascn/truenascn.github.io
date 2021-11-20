# 活动目录（Active Directory）

Active Directory (AD) 服务共享 Windows 网络中的资源。 AD 为网络中的用户提供身份验证和授权服务，无需在 TrueNAS 上重新创建用户帐户。

加入 AD 域后，您可以在文件和目录的本地 ACL 中使用域用户和组。 您还可以将共享设置为文件服务器。

加入 AD 域还会配置 Privileged Access Manager (PAM) 以让域用户通过 SSH 登录或对本地服务进行身份验证。

用户可以在运行 Samba 版本 4 的 Windows 或类 Unix 操作系统上配置 AD 服务。

要配置连接，您需要知道 Active Directory 域控制器的域名和该域管理员的帐户凭据。

## 准备

用户可以在配置 Active Directory 之前采取一些步骤，以确保连接过程顺利进行。

### 验证名称解析

要确认名称解析正常运行，请转到**Shell**并使用 ping 检查与 AD 域控制器的连接。

![](https://www.truenas.com/docs/images/CORE/12.0/ShellDomainControllerPing.png)

当数据包被无丢失地发送和接收时，连接被验证。 按 Ctrl + C 取消 ping。

另一种选择是使用 `host -t srv _ldap._tcp.domainname.com` 来检查网络的 SRV 记录并验证 DNS 解析。

**Q**；ping不通！怎么办？

**A**：如果 ping 失败，请转至网络（**Network**） > 全局配置（**Global Configuration**）并更新 DNS 服务器（**DNS Servers**）和默认网关（**Default Gateway**）设置，以便可以启动与 Active Directory 域控制器的连接。 为 AD 域控制器使用多个名称服务器，以便对必需的 SRV 记录的 DNS 查询可以成功。 每当域控制器不可用时，使用多个名称服务器有助于维持 AD 连接。

### 时间同步

Active Directory 依赖于 [Kerberos](https://tools.ietf.org/html/rfc1510)，这是一种对时间敏感的协议。 在域加入过程中，具有 [PDC Emulator FSMO 角色](https://support.microsoft.com/en-us/help/197132/active-directory-fsmo-roles-in-windows)的 AD 域控制器被添加为首选 NTP 服务器。 如果您的环境需要不同的设置，您可以在系统（**System**） > NTP 服务器（**NTP Servers**）中更改 NTP 服务器设置。

在默认 AD 环境中，本地系统时间与 AD 域控制器时间时差不能超过五分钟。 配置虚拟化域控制器时使用外部时间源。 如果系统时间与 AD 域控制器时间不同步，TrueNAS 会创建警报。

TrueNAS 中有几个选项可以确保两个系统同步：

- 转到系统（**System**） >通用（**General**） 并确保 时区（**Timezone**） 与 AD 域控制器匹配。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemGeneralTimezoneOptions.png)

- 在系统 BIOS 中设置本地时间或世界时间。

## 连接到 Active Directory 域

要连接到 Active Directory，请转至目录服务（**Directory Services**） >活动目录（**Active Directory**），然后输入 AD 域名（**Domain Name**）和帐户凭据。 勾选“启用”（**Enable**）可以在保存配置后立即尝试加入 AD 域。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesActiveDirectoryExample.png)

高级选项可用于微调 AD 配置，但预配置的默认值通常适用。

**Q**：我没有看到任何域信息！

**A**：配置 Active Directory 服务后，TrueNAS 可能需要几分钟来填充 AD 信息。 要查看 AD 加入进度，请打开右上角的任务管理器（**Task Manager** ）。 TrueNAS 会在任务管理器中显示加入过程中的任何错误。

导入完成后，AD 用户和组在配置基本数据集权限或启用 TrueNAS 缓存（默认启用）的[访问控制列表 (ACL)](https://www.truenas.com/docs/core/storage/pools/permissions/) 时可用。

加入 AD 还会添加默认 Kerberos 领域并生成默认 `AD_MACHINE_ACCOUNT` 密钥表。 TrueNAS 会自动开始使用此默认密钥表并删除存储在 TrueNAS 配置文件中的所有管理员凭据。

高级选项

![](https://www.truenas.com/docs/images/CORE/12.0/ActiveDirectoryAdvancedOptions.png)

| 设置选项                                       | 描述                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 详细日志记录（**Verbose logging**）            | 设置为将加入域的尝试记录到 /var/log/messages。               |
| 允许受信任的域（**Allow Trusted Domains**）    | 设置后，用户名不包含域名。 取消设置以强制将域名添加到用户名之前。 取消设置此值的一个可能原因是在设置了允许受信任的域（**Allow Trusted Domains**） 并且在多个域中有相同的用户名时防止用户名冲突 |
| 使用默认域（**Use Default Domain**）           | 取消设置以在用户名前添加域名。 取消设置以防止在设置了允许受信任的域（**Allow Trusted Domains**）且多个域使用相同用户名时发生名称冲突。 |
| 允许 DNS 更新（**Allow DNS Updates**）         | 设置为在加入域时启用 Samba 进行 DNS 更新。                   |
| 禁用 FreeNAS 缓存（**Disable FreeNAS Cache**） | 设置为禁用缓存 AD 用户和组。 当无法绑定到具有大量用户或组的域时，这会有所帮助。 |
| 限制 SSH 连接（**Restrict PAM**）              | 设置为在某些情况下将 SSH 访问限制为仅限 BUILTIN\Administrators 的成员。 |
| 站点名称（**Site Name**）                      | 在 Active Directory 中输入站点对象的相对专有名称。           |
| Kerberos 领域（**Kerberos Realm**）            | 选择在目录服务（**Directory Services**） > Kerberos 领域（**Kerberos Realms**）中添加的现有领域。 |
| Kerberos 主体（**Kerberos Principal**）        | 在目录服务（**Directory Services**）  > Kerberos 密钥表（**Kerberos Keytabs**）中创建的密钥表中选择主体的位置。 |
| 计算机帐户 OU（**Computer Account OU**）       | 在其中创建新计算机帐户的 OU。 OU 字符串是从上到下读取的，没有 RDN。 斜线（“/”）用作分隔符，如计算机/服务器/NAS。 反斜杠 ("\") 用于转义字符，但不用作分隔符。 反斜杠在多个级别进行解释，可能需要加倍甚至四倍才能生效。 当此字段为空时，将在 Active Directory 默认 OU 中创建新的计算机帐户。 |
| AD超时（**AD Timeout**）                       | 连接AD域控制器超时前的秒数。 查看AD连接状态，打开任务管理器界面。 |
| DNS超时(**DNS Timeout**)                       | 查询DNS超时前的秒数。 如果 AD DNS 查询超时，请增加此值。     |
| Winbind NSS 信息(**Winbind NSS Info**)         | 选择在查询 AD 以获取用户/组信息时使用的架构。 *rfc2307* 使用 Windows 2003 R2 中包含的模式支持，*sfu* 用于 Service For Unix 3.0 或 3.5，*sfu20* 用于 Service For Unix 2.0。 |
| Netbios 名称(**Netbios Name**)                 | 在Netbios中此NAS 的名称。 此名称必须与工作组名称不同，并且不得超过 15 个字符。 |
| NetBIOS 别名(**NetBIOS alias**)                | SMB 客户端在连接到此 NAS 时可以使用的替代名称。 不能超过 15 个字符。 |
| 编辑 IDMAP(**EDIT IDMAP**)                     | 导航到目录服务（ **Directory Services**）> **Idmap **，以便用户可以编辑 Active Directory 的 Idmap |
| 退出域(**LEAVE DOMAIN**)                       | 断开 TrueNAS 系统与 Active Directory 的连接。                |

## FTP访问

虽然建议使用 SFTP 而非 FTP，但加入的系统确实允许 FTP 访问。 请牢记以下注意事项：

- 默认情况下，身份验证使用 **DOMAIN\username** 作为用户名。
- 加入前需要存在用户主目录。
- 无法将 AD 用户添加到 FTP 组。 为 FTP 启用本地用户身份验证。
- 在 GUI 中创建的现有 samba **homes** 共享被设置为 AD 用户的 *template homedir*。 这意味着 AD 用户主目录设置在该路径中。 适当的权限至关重要。
- 没有关于 `proftpd` 如何处理 ACL 的保证。
- 当 AD 用户在他们的 LDAP 架构中填充 homedir 信息时，管理员（或 `pam_mkhomedir`）必须确保路径存在。
- 当管理员从他们的 LDAP 架构中提取主目录时，要格外小心以确保用户不会将文件写入引导设备。

## 故障排除

如果缓存变得不同步或权限编辑器中可用的用户少于预期，请使用目录服务（**Directory Services**）> 活动目录（**Active Directory**） > 重建目录服务缓存（**REBUILD DIRECTORY SERVICE CACHE**） 重新同步它。

如果您使用的是 2008 R2 或更早版本的 Windows Server，请尝试在 Windows 服务器组织单位 (OU) 上创建一个计算机条目。 创建此条目时，请在名称字段中输入 TrueNAS 主机名。 确保它与网络（**Network**） > 全局配置（**Global Configuration**）中的主机名字段中设置的名称相同，以及目录服务（**Directory Services**）> 活动目录（**Active Directory**） > 高级选项（**Advanced Options**）中的 NetBIOS 别名。

### Shell命令

您可以转到 Shell 并输入各种命令以获取有关 AD 连接和用户的更多详细信息：

- AD 当前状态： `midclt call activedirectory.get_state`.
- 当前连接的轻量级目录访问协议 (LDAP) 服务器的详细信息：`midclt call activedirectory.domain_info | jq`。

示例：

```
truenas# midclt call activedirectory.domain_info | jq
{
  "LDAP server": "192.168.1.125",
  "LDAP server name": "DC01.HOMEDOM.FUN",
  "Realm": "HOMEDOM.FUN",
  "Bind Path": "dc=HOMEDOM,dc=FUN",
  "LDAP port": 389,
  "Server time": 1593026080,
  "KDC server": "192.168.1.125",
  "Server time offset": 5,
  "Last machine account password change": 1592423446
}
```

- 查看 AD 用户： `wbinfo -u`。 要查看有关用户的更多详细信息，请输入 `getent passwd DOMAIN\\<user>`，将 <user> 替换为所需的用户名。如果 `wbinfo -u` 显示的用户多于配置权限时可用的用户数并且启用了 TrueNAS 缓存，请转至目录服务（**Directory Services**）> 活动目录（**Active Directory**） 并增加AD超时（**AD Timeout**）值。
- 查看 AD 组： `wbinfo -g`。 要查看更多详细信息，请输入 `getent group DOMAIN\\domain\ users`。
- 查看域：`wbinfo -m`。
- 测试 AD 连接： `wbinfo -t`.成功的测试显示一条消息，类似于 `checking the trust secret for domain YOURDOMAIN via RPC calls succeeded`。
- SMB 共享的用户连接测试： `smbclient '//127.0.0.1/smbshare -U AD01.LAB.IXSYSTEMS.COM\ixuser`，将 `127.0.0.1` 替换为您的服务器地址，将 `smbshare` 替换为 SMB 共享名称 `AD01.LAB.IXSYSTEMS .COM` 与您的受信任域，将 `ixuser` 替换为用于身份验证测试的用户帐户名。