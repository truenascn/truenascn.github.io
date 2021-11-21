# Windows 共享（SMB）

SMB（也称为 CIFS）是 Windows 中的本机文件共享系统。 SMB 共享可以连接到任何主要操作系统，包括 Windows、MacOS 和 Linux。 SMB 可用于 TrueNAS 以在单个或多个用户或设备之间共享文件。

SMB 共享允许广泛的权限和安全设置，并且可以支持 Windows 和其他系统上的高级权限 (ACL)，以及 Windows 备用流和扩展元数据。 SMB 适用于大型或小型数据池的管理和管理。

TrueNAS 使用 [Samba](https://www.samba.org/) 提供 SMB 服务。 SMB 协议有多个版本。 SMB 客户端通常会在 SMB 会话协商期间协商支持的最高 SMB 协议。 在整个行业范围内，SMB1 协议（有时称为 NT1）的[正在被弃用](https://www.truenas.com/docs/core/notices/smb1advisory/)。 这种弃用是出于安全原因。 但是，大多数 SMB 客户端支持 SMB 2 或 3 协议，即使它们不是默认协议。

