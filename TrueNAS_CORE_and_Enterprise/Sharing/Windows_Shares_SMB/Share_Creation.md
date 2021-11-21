# SMB 共享创建

## 背景知识

旧版 SMB 客户端依靠 NetBIOS 名称解析来发现网络上的 SMB 服务器。 默认情况下，在 TrueNAS 中禁用 NetBIOS 名称服务器 (nmbd)。 如果需要此功能，可以在网络（**Network**） > 全局配置（**Global Configuration**）中启用它。

MacOS 客户端使用 mDNS 来发现网络上是否存在 SMB 服务器。 在 TrueNAS 上默认启用 mDNS 服务器 (avahi)。

Windows 客户端使用 [WS-Discovery](https://docs.oasis-open.org/ws-dd/ns/discovery/2009/01) 来发现 SMB 服务器的存在，但根据 Windows 客户端的版本，默认情况下可以禁用网络发现。

通过广播协议的可发现性是一项便利功能，不需要访问 SMB 服务器。

## 前期准备

### 创建一个数据集

建议创建一个新的数据集，并将新的数据集的共享类型设置为 SMB。

ZFS 数据集是使用以下设置创建的：

- 访问控制列表（ACL）模式（**aclmode**） = 受限（**restricted**）
- 大小写敏感（**case sensitivity**） = 不敏感（**insensitive**）

默认访问控制列表也应用于数据集。 此默认 ACL 是限制性的，仅允许访问数据集所有者和组。 稍后可以根据您的用例修改此 ACL。

### 创建本地用户帐户

默认情况下，所有新本地用户都是内置 SMB 组（**builtin users**）的成员。 该组可用于向服务器上的所有本地用户授予访问权限。 附加[组](https://www.truenas.com/docs/core/accounts/groups/)可用于微调对大量用户的权限。 TrueNAS 内置的用户帐户或没有设置 smb 标志的用户帐户不能用于 SMB 访问。

**Q**：为什么不仅仅允许匿名访问共享？

**A**：尽管可以匿名或访客访问共享，但这是一个安全漏洞，主要 SMB 客户端供应商已弃用。 这部分是因为来宾会话无法进行签名和加密。

Q：LDAP 用户呢？

A：当在系统中已经配置了 LDAP ，并且您希望来自 LDAP 服务器的用户可以访问 SMB 共享时，请转至目录服务（**Directory Services**） > **LDAP** > 高级模式（**ADVANCED MODE**）并设置 Samba 模式。 但是，本地 TrueNAS 用户帐户将无法再访问共享。

### 调整数据集 ACL

创建数据集和帐户后，您需要了解访问需求，并调整数据集 ACL 以进行匹配。 要编辑 ACL，请转至存储（**Storage**） > 池（**Pools**），打开新数据集的选项，然后单击编辑权限（**Edit Permissions**）。 许多家庭用户通常会添加一个新条目，将完全控制（**FULL_CONTROL**） 授予内置SMB组（**builtin_users**） 组，并将标志设置为 继承（**INHERIT**）。 有关更多详细信息，请参阅[权限文章](https://www.truenas.com/docs/core/storage/pools/permissions/)。

## 创建 SMB 共享

要创建 Windows SMB 共享，请转至共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)**），然后单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAdd.png)

SMB 共享的路径（**Path**）和名称（**Name**）定义了创建新 SMB 共享所需的最少信息量。路径（**Path**）是本地文件系统上将通过 SMB 协议导出的目录树，名称（**Name**）是 SMB 共享的名称，当 SMB 客户端连接到此 SMB 服务器时，它构成“完整共享路径名”的一部分。 由于名称在 SMB 协议中的使用方式，它的长度必须小于或等于 80 个字符，并且不得包含 Microsoft 文档 MS-FSCC 部分 2.1.6 中指定的任何无效字符。 如果未提供名称，则路径的最后目录名称将用作共享名称。

您可以设置共享预设（**Purpose**）来应用和锁定共享的预先定义高级选项。 要保留对所有共享高级选项的完全控制，请选择无预设（**No presets**）。

下表显示了不同用途的预设选项以及这些选项是否被锁定。 [x] 表示该选项**已启用**，[ ] 表示该选项**已禁用**，[text] 表示**特定值**：

|           默认共享参数（Default share parameters）           | 多用户时光机（Multi-user time machine）                      | 多协议 (AFP/SMB) 共享（Multi-protocol (AFP/SMB) shares）     | 多协议 (NFSv3/SMB) 共享（Multi-protocol (NFSv3/SMB) shares） | 私有 SMB 数据集和共享（Private SMB Datasets and Shares）     | 5 分钟后文件变为 SMB 只读（Files become readonly of SMB after 5 minutes） |
| :----------------------------------------------------------: | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
|            [x] 开启ACL（**Enable ACL**） (locked)            | [x] 开启ACL（**Enable ACL**）(unlocked)                      | [x] 开启ACL（**Enable ACL**）(locked)                        | [ ] 开启ACL（**Enable ACL**）(locked)                        | [ ]开启ACL（**Enable ACL**）(unlocked)                       | [ ] 开启ACL（**Enable ACL**） (unlocked)                     |
|      [ ] 以只读方式导出（**Export Read Only**）(locked)      | [ ] 以只读方式导出（**Export Read Only**）(unlocked)         | [ ] 以只读方式导出（**Export Read Only**）(unlocked)         | [ ]以只读方式导出（**Export Read Only**） (unlocked)         | [ ] 以只读方式导出（**Export Read Only**） (unlocked)        | [ ] 以只读方式导出（**Export Read Only**）(unlocked)         |
| [x] 网络客户端可浏览（**Browsable to Network Clients**） (locked) | [x] 网络客户端可浏览（**Browsable to Network Clients**）(unlocked) | [x] 网络客户端可浏览（**Browsable to Network Clients**）(unlocked) | [x] 网络客户端可浏览（**Browsable to Network Clients**）(unlocked) | [x] 网络客户端可浏览（**Browsable to Network Clients**） (unlocked) | [x] 网络客户端可浏览（**Browsable to Network Clients**）(unlocked) |
|    [ ] 允许访客访问（**Allow Guest Access**） (unlocked)     | [ ] 允许访客访问（**Allow Guest Access**） (unlocked)        | [ ] 允许访客访问（**Allow Guest Access**） (unlocked)        | [ ] 允许访客访问（**Allow Guest Access**） (unlocked)        | [ ] 允许访客访问（**Allow Guest Access**） (unlocked)        | [ ] 允许访客访问（**Allow Guest Access**）  (unlocked)       |
| [ ] 基于访问的共享枚举（**Access Based Share Enumeration**） (locked) | [ ] 基于访问的共享枚举（**Access Based Share Enumeration**） (unlocked) | [ ] 基于访问的共享枚举（**Access Based Share Enumeration**） (unlocked) | [ ] 基于访问的共享枚举（**Access Based Share Enumeration**）  (unlocked) | [ ] 基于访问的共享枚举（**Access Based Share Enumeration**） (unlocked) | [ ] 基于访问的共享枚举（**Access Based Share Enumeration**）  (unlocked) |
|          [ ] 白名单主机（**Hosts Allow**） (locked)          | [ ] 白名单主机（**Hosts Allow**）(unlocked)                  | [ ] 白名单主机（**Hosts Allow**）(unlocked)                  | [ ] 白名单主机（**Hosts Allow**）(unlocked)                  | [ ] 白名单主机（**Hosts Allow**）(unlocked)                  | [ ] 白名单主机（**Hosts Allow**）(unlocked)                  |
|          [ ] 黑名单主机（**Hosts Deny**） (locked)           | [ ] 黑名单主机（**Hosts Deny**）(unlocked)                   | [ ] 黑名单主机（**Hosts Deny**）(unlocked)                   | [ ] 黑名单主机（**Hosts Deny**） (unlocked)                  | [ ] 黑名单主机（**Hosts Deny**）(unlocked)                   | [ ] 黑名单主机（**Hosts Deny**）(unlocked)                   |
|      [ ] 用作家庭共享（**Use as Home Share**） (locked)      | [ ] 用作家庭共享（**Use as Home Share**） (unlocked)         | [ ] 用作家庭共享（**Use as Home Share**） (unlocked)         | [ ] 用作家庭共享（**Use as Home Share**）(unlocked)          | [ ] 用作家庭共享（**Use as Home Share**）(unlocked)          | [ ] 用作家庭共享（**Use as Home Share**）(unlocked)          |
|           [ ] 时光机（**Time Machine**） (locked)            | [ ] 时光机（**Time Machine**） (unlocked)                    | [ ] 时光机（**Time Machine**） (unlocked)                    | [ ] 时光机（**Time Machine**） (unlocked)                    | [ ]时光机（**Time Machine**） (unlocked)                     | [ ] 时光机（**Time Machine**）(unlocked)                     |
|    [x] 启用卷影副本（**Enable Shadow Copies**） (locked)     | [x]启用卷影副本（**Enable Shadow Copies**）(unlocked)        | [x] 启用卷影副本（**Enable Shadow Copies**）(unlocked)       | [x] 启用卷影副本（**Enable Shadow Copies**）(unlocked)       | [x] 启用卷影副本（**Enable Shadow Copies**）(unlocked)       | [x] 启用卷影副本（**Enable Shadow Copies**） (unlocked)      |
|      [ ] 导出回收站（**Export Recycle Bin**） (locked)       | [ ] 导出回收站（**Export Recycle Bin**）(unlocked)           | [ ]导出回收站（**Export Recycle Bin**）(unlocked)            | [ ] 导出回收站（**Export Recycle Bin**）(unlocked)           | [ ] 导出回收站（**Export Recycle Bin**） (unlocked)          | [ ] 导出回收站（**Export Recycle Bin**）(unlocked)           |
| [ ] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (locked) | [ ] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (unlocked) | [x] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (locked) | [x] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (unlocked) | [x] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (unlocked) | [x] 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） (unlocked) |
| [x] 启用备用数据流（**Enable Alternate Data Streams**） (locked) | [x] 启用备用数据流（**Enable Alternate Data Streams**） (unlocked) | [x] 启用备用数据流（**Enable Alternate Data Streams**）  (locked) | [ ] 启用备用数据流（**Enable Alternate Data Streams**）  (locked) | [ ] 启用备用数据流（**Enable Alternate Data Streams**） (unlocked) | [ ] 启用备用数据流（**Enable Alternate Data Streams**） (unlocked) |
| [x] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (locked) | [x] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (unlocked) | [ ] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (locked) | [ ] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (locked) | [ ] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (unlocked) | [ ] 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**） (unlocked) |
|         [ ] 启用 FSRVP（**Enable FSRVP**） (locked)          | [ ] 启用 FSRVP（**Enable FSRVP**）(unlocked)                 | [ ] 启用 FSRVP（**Enable FSRVP**）(locked)                   | [ ] 启用 FSRVP（**Enable FSRVP**） (unlocked)                | [ ] 启用 FSRVP（**Enable FSRVP**） (unlocked)                | [ ] 启用 FSRVP（**Enable FSRVP**）(unlocked)                 |
|           [ ] 路径后缀（**Path Suffix**） (locked)           | [%U] 路径后缀（**Path Suffix**） (locked)                    | [%U] 路径后缀（**Path Suffix**） (unlocked)                  | [%U] 路径后缀（**Path Suffix**） (unlocked)                  | [%U] 路径后缀（**Path Suffix**） (locked)                    | [ ] 路径后缀（**Path Suffix**） (locked)                     |
|     [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)      | [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)          | [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)          | [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)          | [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)          | [ ] 辅助参数（**Auxiliary Parameters**） (unlocked)          |

可以指定一个可选的描述来帮助解释共享的目的。

启用（**Enabled**）允许在激活 SMB 服务时共享此路径。不勾选启用（**Enabled**）可以禁用共享而不删除配置。

### 高级选项

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAddAdvanced.png)

选项分为访问（**Access**）和其他选项（**Other Options**）。 访问选项控制允许系统或用户访问或修改共享数据的各种设置。

| 设置选项                                                 | 值              | 描述                                                         |
| -------------------------------------------------------- | --------------- | ------------------------------------------------------------ |
| 开启ACL（**Enable ACL**）                                | 复选框/checkbox | 设置为向共享添加访问控制列表 (ACL) 支持。 取消设置会禁用 ACL 支持并删除共享的任何现有 ACL。 |
| 以只读方式导出（**Export Read Only**）                   | 复选框/checkbox | 禁止写入共享。 取消设置以允许写入共享。                      |
| 网络客户端可浏览（**Browsable to Network Clients**）     | 复选框/checkbox | 确定浏览共享时是否包含此共享名称。 无论此设置如何，家庭共享仅对所有者可见。 |
| 允许访客访问（**Allow Guest Access**）                   | 复选框/checkbox | 权限与来宾帐户相同。 默认情况下，Windows 10 版本 1709 和 Windows Server 版本 1903 中禁用来宾访问。需要额外的客户端配置来提供对这些客户端的来宾访问。 在MacOS中，尝试以 FreeNAS 中不存在的用户身份连接**不会**自动以访客帐户身份连接。 *Connect As: Guest* 选项必须在 MacOS 中特别选择才能以访客帐户登录。 有关更多详细信息，请参阅 [Apple 文档](https://support.apple.com/guide/mac-help/connect-mac-shared-computers-servers-mchlp1140/mac)。 |
| 基于访问的共享枚举（**Access Based Share Enumeration**） | 复选框/checkbox | 设置此项会限制对共享具有读或写访问权限的用户的共享可见性。 请参阅 [smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html) 手册页。 |
| 白名单主机（**Hosts Allow**）                            | 字符串/string   | 输入允许的主机名或 IP 地址的列表。 按 Enter 分隔条目。 可以在 [此处](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html#HOSTSALLOW) 中找到更详细的示例说明。 |
| 黑名单主机（**Hosts Deny**）                             | 字符串/string   | 输入被拒绝连接的主机名或 IP 地址的列表。 按 Enter 分隔条目。 |

白名单主机（**Hosts Allow**） 和黑名单主机（**Hosts Deny**） 字段一起工作以产生不同的效果：

- 如果 白名单主机（**Hosts Allow**） 和黑名单主机（**Hosts Deny**） 都不包含条目，则允许任何主机访问 SMB 共享。
- 如果有白名单主机（**Hosts Allow**） 但没有黑名单主机（**Hosts Deny**） ，则只允许主机允许列表中的主机。
- 如果有 黑名单主机（**Hosts Deny**）但没有 白名单主机（**Hosts Allow**），则允许所有不在黑名单主机（**Hosts Deny**）中的主机连接。
- 如果同时存在白名单主机（**Hosts Allow**）和 黑名单主机（**Hosts Deny**）列表，则允许白名单主机（**Hosts Allow**）列表中的所有主机。 如果主机不在白名单主机（**Hosts Allow**） 和黑名单主机（**Hosts Deny**）列表中，则允许它。

其他选项具有用于提高 Apple 软件兼容性、ZFS 快照功能和其他高级功能的设置。

| 设置选项                                                     | 值              | 描述                                                         |
| ------------------------------------------------------------ | --------------- | ------------------------------------------------------------ |
| 用作家庭共享（**Use as Home Share**）                        | 复选框/checkbox | 允许共享宿主用户主目录。 每个用户在连接到其他用户无法访问的共享时都会获得一个个人主目录。 这允许个人的、动态的共享。 只有一个共享可以用作家庭共享。 详细说明见配置【首页分享文章】(https://www.truenas.com/docs/core/sharing/smb/homeshare/)。 |
| 时光机（**Time Machine**）                                   | 复选框/checkbox | 在此共享上启用 [Apple Time Machine](https://support.apple.com/en-us/HT201250) 备份。 |
| 启用卷影副本（**Enable Shadow Copies**）                     | 复选框/checkbox | 将 ZFS 快照导出为 Microsoft 卷影复制服务 (VSS) 的 [Shadow Copies](https://docs.microsoft.com/en-us/windows/win32/vss/shadow-copies-and-shadow-copy-sets) 客户。 |
| 导出回收站（**Export Recycle Bin**）                         | 复选框/checkbox | 从同一数据集中删除的文件将移动到回收站，并且不会占用任何额外空间。 **通过 NFS 删除文件将永久删除文件。** 当文件在不同的数据集或子数据集中时，它们将被复制到回收站所在的数据集。 为防止过度使用空间，大于*20 MiB* 的文件将被删除而不是移动。 调整 **Auxiliary Parameter** `crossrename:sizelimit=` 设置以允许更大的文件。 例如，`crossrename:sizelimit=*50*` 允许移动最大为 *50 MiB* 的文件。 这意味着文件可以从回收站中永久删除或移动。 **这不是 ZFS 快照的替代品。** |
| 使用 Apple 风格的字符编码（**Use Apple-style Character Encoding**） | 复选框/checkbox | 默认情况下，Samba 对 NTFS 非法字符使用散列算法。 启用此选项会以与 MacOS SMB 客户端相同的方式转换 NTFS 非法字符。 |
| 启用备用数据流（**Enable Alternate Data Streams**）          | 复选框/checkbox | 允许多个 [NTFS 数据流](https://www.ntfs.com/ntfs-multiple.htm)。 禁用此选项会导致 MacOS 将流写入文件系统上的文件。 |
| 启用 SMB2/3 长连接（**Enable SMB2/3 Durable Handles**）      | 复选框/checkbox | 允许使用可以承受短时间断开连接的打开文件句柄。 Samba 中对 POSIX 字节范围锁的支持也被禁用。 配置多协议或本地文件访问时，不建议使用此选项。 |
| 启用 FSRVP（**Enable FSRVP**）                               | 复选框/checkbox | 启用对文件服务器远程 VSS 协议的支持 ([FSVRP](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-fsrvp/dae107ec-8198-4778-a950-faa7edad125b))。 此协议允许远程过程调用 (RPC) 客户端管理特定 SMB 共享的快照。 共享路径必须是数据集挂载点。 快照具有前缀 `fss-` 后跟快照创建时间戳。 快照必须具有此前缀，RPC 用户才能将其删除。 |
| 路径后缀（**Path Suffix**）                                  | 字符串/string   | 将后缀附加到共享连接路径。 这用于在每个用户、每个计算机或每个 IP 地址的基础上提供唯一的共享。 后缀可以包含一个宏。 有关支持的宏列表，请参阅 [smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html) 手册页。 必须在客户端连接之前预设连接路径。 |
| 辅助参数（**Auxiliary Parameters**）                         | 字符串/string   | 附加 [smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html) 设置。 |

单击提交（**Submit**）创建共享并将其添加到共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)**）列表。 此时也可以选择开启SMB服务。

## 共享管理

创建 SMB 共享后，可以通过转至共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)**）并单击共享条目的三个小点（选项）来使用其他管理选项：

- 编辑（**Edit**）: 打开[共享创建屏幕](https://www.truenas.com/docs/core/sharing/smb/smbshare/#creating-the-smb-share) 以重新配置共享或禁用共享。
- 编辑共享访问控制列表（ACL）（**Edit Share ACL**）: 打开一个屏幕以配置共享的访问控制列表 (ACL)。 这与文件系统权限不同，适用于整个 SMB 共享级别。 此处定义的权限不会被其他文件共享协议的客户端或导出相同共享 路径（**Path**） 的其他 SMB 共享解释。 默认是打开的。 如果启用了 基于访问的共享枚举（**Access Based Share Enumeration**），则此 ACL 用于确定浏览列表。
- 编辑文件系统访问控制列表（**Edit Filesystem ACL**）: 打开一个屏幕，为共享 路径（**Path**） 中定义的路径配置访问控制列表 (ACL)。
- 删除（**Delete**）: 从 TrueNAS 中删除共享配置。 正在共享的数据不受影响。

### 配置共享ACL

要查看共享 ACL 选项，请单击共享条目的三个小点（选项） > 编辑共享 ACL（**Edit Share ACL**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBShareACL.png)

显示共享名称，但不能更改。 ACL 条目（**ACL Entries**）被列为一组设置。 单击添加（**ADD**）以注册新条目。

| 设置选项               | 值                 | 描述                                                         |
| ---------------------- | ------------------ | ------------------------------------------------------------ |
| **SID**                | 字符串/string      | 此 ACL 条目 (ACE) 适用于谁，显示为 [Windows 安全标识符](https://docs.microsoft.com/en-us/windows/win32/secauthz/security-identifiers)。 ACL 需要 *SID* 或带有 *Name* 的 *Domain*。 |
| 域名（**Domain**）     | 字符串/string      | 用户 *Name* 的域。 未输入 **SID** 时需要。 本地用户具有 SMB 服务器 NetBIOS 名称：*truenas\smbusers*。 |
| 权限（**Permission**） | 下拉菜单/drop down | 预定义权限组合： *读取*：对象 (RX) 的读取访问和执行权限。 *更改*：读取权限、执行权限、写入权限和删除对象 (RXWD)。 *完整*：读取访问权限、执行权限、写入访问权限、删除对象、更改权限和获取所有权 (RXWDPO)。 有关更多详细信息，请参阅 [smbacls(1)](https://www.samba.org/samba/docs/current/man-html/smbcacls.1.html)。 |
| 名称（**Name**）       | 字符串/string      | 此 ACL 条目适用于谁，显示为用户名。 需要添加用户**域**。     |
| 类型（**Type**）       | 下拉菜单/drop down | 如何将权限应用于共享。 允许（**Allowed**） 默认拒绝所有权限，手动定义的权限除外。 拒绝（**Denied**）默认允许所有权限，手动定义的权限除外。 |

单击“保存”（**SAVE**）会存储共享 ACL 并立即将其应用于共享。

### 配置文件系统ACL

单击 共享条目的三个小点（选项）  > 编辑文件系统访问控制列表（**Edit Filesystem ACL**）可快速返回 存储（**Storage**） > 存储池（**Pools**） 并编辑数据集 ACL。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEditACLOwner.png)

此 ACL 用于定义对正在共享的数据集拥有或具有特定权限的用户帐户或组。 用户和组值显示哪些帐户“拥有”或对数据集具有完全权限。 将默认设置更改为您首选的主帐户和组，并在保存任何更改之前设置应用复选框。

#### ACL 预设

要使用标准化预设重写当前 ACL，请单击 使用一个ACL预设（**SELECT AN ACL PRESET**） 并选择一个选项：

##### 开放（Open）

有三个条目：

- *owner@* 有完整的数据集控制权。
- *group@* 有完整的数据集控制权。
- 所有其他帐户都可以修改数据集内容。

##### 受限（Restricted）

有两个条目：

- *owner@* 有完整的数据集控制权。
- *group@* 可以修改数据集内容。

##### 家庭（Home）

有三个条目：

- *owner@* 有完整的数据集控制权。
- *group@* 可以修改数据集内容。
- 所有其他帐户都可以遍历（**traverse through**）（显示目录和文件，但是不能打开）数据集。

#### 添加 ACL 条目 (ACE)

要为特定用户帐户或组定义权限，请单击添加 ACL项目（**ADD ACL ITEM**）。 打开 Who 下拉菜单，选择 用户（**User**） 或 用户组（**Group**），然后选择特定的用户或组帐户。 定义如何将该权限设置应用于相关帐户，然后选择要应用于该帐户的权限。 例如，要仅允许 tmoore 用户权限查看数据集内容而不进行更改，请将 ACL 类型（**ACL Type**）设置为允许（**Allow**），将权限（**Permissions**）设置为读取（**Read**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEditACLExample.png)

## 激活 SMB 服务

当相关系统服务未激活时，无法连接到 SMB 共享。 要使 SMB 共享在网络上可用，请单击服务（**Services**）并单击 SMB 切换为启动。 如果您希望在 TrueNAS 启动时激活该服务，请设置自动启动（**Start Automatically**）。

### 服务配置

SMB 服务通过单击编辑图标进行配置。 除非需要进行特定设置或针对特定网络环境进行配置，否则建议使用 SMB 服务的默认设置。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSMBOptions.png)

| 设置选项                                 | 值              | 描述                                                         |
| ---------------------------------------- | --------------- | ------------------------------------------------------------ |
| NetBIOS名字（**NetBIOS Name**）          | 字符串/string   | 自动填充系统的原始主机名。 此名称限制为 15 个字符，并且不能是工作组（**Workgroup**）名称。 |
| NetBIOS 别名（**NetBIOS Alias**）        | 字符串/string   | 输入任何别名，以空格分隔。 每个别名最长可达 15 个字符。      |
| 工作组（**Workgroup**）                  | 字符串/string   | 必须与 Windows 工作组名称匹配。 当未配置且 Active Directory 或 LDAP 处于活动状态时，TrueNAS 将从这些服务中检测并设置正确的工作组。 |
| 描述（**Description**）                  | 字符串/string   | 这允许输入有关服务配置的任何注释或描述性详细信息。           |
| 启用SMB 1支持（**Enable SMB1 support**） | 复选框/checkbox | 允许旧版 SMB1 客户端连接到服务器。 请注意，SMB1 已被弃用，建议将客户端升级到支持 SMB 协议现代版本的操作系统版本。 |
| NTLMv1 身份验证（**NTLMv1 Auth**）       | 复选框/checkbox | 设置后，[smbd](https://www.samba.org/samba/docs/current/man-html/smbd.8.html) 尝试使用不安全且易受攻击的 NTLMv1 加密对用户进行身份验证。 此设置允许与旧版本的 Windows 向后兼容，但不建议使用，也不应在不受信任的网络上使用。 |

### 高级选项

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSMBOptionsAdvanced.png)

| 设置选项                                                     | 值                 | 描述                                                         |
| ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| UNIX 字符集（**UNIX Charset**）                              | 下拉菜单/drop down | 内部使用的字符集。 *UTF-8* 是大多数系统的标准，因为它支持所有语言的所有字符。 |
| 日志等级（**Log Level**）                                    | 下拉菜单/drop down | 将 SMB 服务消息记录到指定的日志级别。 默认情况下，会记录错误和警告级别的消息。 不建议对生产服务器使用高于 MINIMUM 的日志级别。 |
| 仅使用系统日志（**Use Syslog Only**）                        | 复选框/checkbox    | 设置为在 */var/log/messages* 中记录身份验证失败，而不是默认的 */var/log/samba4/log.smbd*。 |
| 本地掌控（**Local Master**）                                 | 复选框/checkbox    | 设置以确定系统是否参与浏览器枚举。 当网络包含 AD 或 LDAP 服务器，或者存在 Vista 或 Windows 7 机器时取消设置。 |
| 启用 Apple SMB2/3 协议扩展（**Enable Apple SMB2/3 Protocol Extensions**） | 复选框/checkbox    | macOS 可以使用这些 [协议扩展](https://support.apple.com/en-us/HT210803) 来改善 SMB 共享的性能和行为特征。 这是 Time Machine 支持所必需的。 |
| 管理员组（**Administrators Group**）                         | 下拉菜单/drop down | 该组的成员是本地管理员，并且自动拥有获取 SMB 共享中任何文件的所有权、重置权限以及通过计算机管理 MMC 管理单元管理 SMB 服务器的特权。 |
| 来宾用户组（**Guest Account**）                              | 下拉菜单/drop down | 用于访客访问的帐户。 默认为*没人*。 所选帐户必须具有共享池或数据集的权限。 要调整权限，请编辑数据集访问控制列表 (ACL)，为所选访客帐户添加新条目，并在该条目中配置权限。 如果选定的**访客帐户** 被删除，该字段将重置为 *nobody*。 |
| 文件掩码（**File Mask**）                                    | 整数/integer       | 覆盖 *0666* 的默认文件创建掩码，它为每个人创建具有读写访问权限的文件。 |
| 目录掩码（**Directory Mask**）                               | 整数/integer       | 覆盖*0777* 的默认目录创建掩码，它为每个人授予目录读取、写入和执行访问权限。 |
| IP地址绑定（**Bind IP Addresses**）                          | 下拉菜单/drop down | SMB 侦听连接的静态 IP 地址。 保留所有未选择的默认值以侦听所有活动接口。 |
| 辅助参数（**Auxiliary Parameters**）                         | 字符串/string      | 存储额外的 [smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)。 辅助参数可用于覆盖默认 SMB 服务器配置，但此类更改可能会对 SMB 服务器稳定性或行为产生不利影响。 |

## 在另一台机器上使用SMB 共享。

### Linux

验证是否为您的 Linux 发行版安装了所需的 CIFS 软件包。 然后创建挂载点：`sudo mkdir /mnt/smb_share`。

挂载卷。 `sudo mount -t cifs //computer_name/share_name /mnt/smb_share`.

如果您的共享需要用户凭据，请在 cifs 之后和共享地址之前添加带有您的用户名的开关 `-o username=`。

### Windows

要将 SMB 共享挂载到 Windows 上的驱动器号，请打开命令行并使用适当的驱动器号、计算机名称和共享名称运行以下命令。

`net use Z: \\computer_name\share_name /PERSISTENT:YES`

### Apple

打开 访达（**Finder**） > 前往（**Go**） > 连接服务器（**Connect To Server**） 输入 SMB 地址：`smb://192.168.1.111`。

如果在共享上启用了访客访问，则输入分配给该池或访客的用户的用户名和密码。

### FreeBSD

创建挂载点：`sudo mkdir /mnt/smb_share`。

挂载卷。 `sudo mount_smbfs -I computer_name\share_name /mnt/smb_share`.