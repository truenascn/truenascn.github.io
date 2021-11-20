# Kerberos

Kerberos 是一种 Web 身份验证协议，它使用强密码术通过不安全的网络连接来证明客户端和服务器的身份。

Kerberos 使用“领域（*realms*）”和“密钥表（*keytabs*）”来验证客户端和服务器。 Kerberos 领域是 Kerberos 服务器可用于对客户端进行身份验证的授权域。 默认情况下，TrueNAS 为本地系统创建一个 Kerberos 领域。 [密钥表（“keytabs”）](https://web.mit.edu/kerberos/krb5-devel/doc/basic/keytab_def.html)是一个文件，用于存储各种身份验证方案的加密密钥。

TrueNAS 允许配置 Kerberos 领域和密钥表。

## Kerberos 领域

您的网络必须包含密钥分发中心 (KDC) 才能添加领域。 用户可以通过导航到目录服务（**Directory Services**）> Kerberos 领域（**Kerberos Realms** ）并单击添加（**ADD**）来配置 Kerberos 领域。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesKerberosRealmsAdd.png)

输入领域名称并单击提交（**SUBMIT**）。

### 高级选项

| 设置选项                          | 描述                                                     |
| --------------------------------- | -------------------------------------------------------- |
| Kerberos 域中心服务器（**KDC**）  | 输入密钥分发中心的名称。 按 Enter 分隔多个值。           |
| 管理服务器（**Admin Server**）    | 定义对数据库执行所有更改的服务器。 按 Enter 分隔多个值。 |
| 密码服务器（**Password Server**） | 定义执行所有密码更改的服务器。 按 Enter 分隔多个值。     |

## Kerberos 密钥表

Kerberos 密钥表允许系统和客户端在没有密码的情况下加入 Active Directory 或 LDAP。 使用密钥表，TrueNAS 系统数据库不存储 Active Directory 或 LDAP 管理员帐户密码，这在某些环境中可能存在安全风险。

使用密钥表时，创建并使用权限较低的帐户来执行任何所需的查询。 TrueNAS 系统数据库存储该帐户的密码。

### 在 Windows Server 上为 Active Directory 创建 Keytab

要在 Windows Server 系统上创建密钥表，请打开命令提示符（**CMD**）并使用 [ktpass](https://techjogging.com/create-keytab-file-for-kerberos-authentication-in-windows.html) 命令：

```
ktpass -princ USERNAME@REALM.COM -pass PASSWORD -crypto ENCRYPTION TYPE -ptype KRB5_NT_PRINCIPAL -kvno 0 -out c:\PATH\KEYTABNAME.KEYTAB
```

`USERNAME@REALM.COM` 是以 username@KERBEROS.REALM 格式编写的 Windows Server 用户和主体名称。 Kerberos 领域通常全部大写，但 Kerberos 领域大小写应与领域名称匹配。 有关更多详细信息，请参阅有关使用 `/princ` 的[说明](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/ktpass#BKMK_remarks)。

`PASSWORD` 是 Windows Server 用户的密码。

`ENCRYPTION TYPE` 是您要使用的加密类型。 将 `ENCRYPTION TYPE` 设置为 `ALL` 允许使用所有支持的加密类型。 用户可以指定每个密钥而不是 ALL：

- *DES-CBC-CRC* 用于保证兼容性。
- *DES-CBC-MD5* 用于兼容性并更接近于 MIT 实现。
- *RC4-HMAC-NT* 使用 128 位加密。
- *AES256-SHA1* 使用 AES256-CTS-HMAC-SHA1-96 加密。
- *AES128-SHA1* 使用 AES128-CTS-HMAC-SHA1-96 加密。 指定加密类型会创建一个具有足够权限来授予票证的密钥表。

`PATH\KEYTABNAME.KEYTAB` 是您要保存密钥表的路径以及您希望它具有的名称。

示例 ktpass 命令如下所示：

```
ktpass -princ admin@WINDOWSSERVER.NET -pass Abcd1234! -crypto ALL -ptype KRB5_NT_PRINCIPAL -kvno 0 -out c:\kerberos\freenas.keytab
```

### 将 Windows Keytab 添加到 TrueNAS

生成密钥表后，在目录服务（**Directory Services**）> Kerberos 密钥表（**Kerberos Keytabs**）> 添加Kerberos 密钥表（ **Add Kerberos Keytab**）中将其添加到TrueNAS 系统中。

要指示 Active Directory 服务使用密钥表，请转至目录服务（**Directory Services**） > 活动目录（**Active Directory**），然后单击高级选项（**Advanced Options**）。 使用 **Kerberos Principal** 下拉列表选择已安装的密钥表。

在 Active Directory 中使用密钥表时，密钥表中的用户名（**username**）和用户密码（**userpass**）应与目录服务（**Directory Services**） >活动目录（**Active Directory**）中的域帐户名称（**Domain Account Name**）和域帐户密码（**Domain Account Password**）字段匹配。

要指示 LDAP 使用密钥表中的主体，请转至目录服务（**Directory Services**） > 活动目录（**Active Directory**）并单击高级选项（**Advanced Options**），然后使用 Kerberos 主体（**Kerberos Principal**）下拉列表选择已安装的密钥表。

## Kerberos 设置

其他 Kerberos 选项位于目录服务（**Directory Services**） > Kerberos 设置（**Kerberos Settings**）中。

![](https://www.truenas.com/docs/images/CORE/12.0/DirectoryServicesKerberosSettings.png)

- Appdefaults 辅助参数（**Appdefaults Auxiliary Parameters**）: 定义供某些 Kerberos 应用程序使用的其他设置。 可用的设置和语法列在 [[appdefaults\] section of krb.conf(5)](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/conf_files/krb5_conf.html#appdefaults).
- Libdefaults 辅助参数（**Libdefaults Auxiliary Parameters**）: 定义 Kerberos 库使用的设置。 可用的设置及其语法列在 [[libdefaults\] section of krb.conf(5)](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/conf_files/krb5_conf.html#libdefaults).

