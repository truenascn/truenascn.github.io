# 卷影副本

卷影副本，也称为卷影复制服务 (VSS) ，是一种用于创建卷快照的 Microsoft 服务。 卷影副本可用于从 Windows 资源管理器中恢复以前版本的文件。

默认情况下，SMB 共享路径下的数据集的所有 ZFS 快照都通过卷影复制服务呈现给 SMB 客户端，或者当隐藏的 ZFS 快照目录位于 SMB 共享路径内时，可以通过 SMB 直接访问。

在激活 TrueNAS 中的功能之前，有一些关于卷影副本的注意事项：

- 当 Windows 系统没有完全修补到最新的服务包时，卷影副本可能不起作用。 如果看不到要还原的以前版本的文件，请使用 Windows 更新（**Windows Update**）以确保系统完全是最新的。
- 卷影复制支持仅适用于 ZFS 池或数据集。
- 必须对 SMB 共享的池或数据集配置适当的权限。

要启用卷影副本，请转至共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)** ） 并编辑（**Edit**）现有共享。 打开高级（**Advanced**）选项，找到其他选项（**Other Options**）并设置启用卷影副本（**Enable Shadow Copies**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAddAdvanced.png)

### Windows 10 v2004 的问题

一些用户在 Windows 10 v2004 版本中遇到了无法访问网络共享的问题。 该问题似乎来自本地组策略编辑器 gpedit.msc 中的错误。 不幸的是，即使在“计算机配置”（**Computer Configuration**）>“管理模板”（**Administrative Templates**）>“网络”（ **Network**）>“Lanman 工作站”（**Lanman Workstation**）中将“允许不安全的访客登录”标志值设置为“已启用”似乎对配置没有影响。

要解决此问题，请编辑 Windows 注册表。 使用 Regedit 并转到 HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters。 DWORD AllowInsecureGuestAuth 是一个不正确的值：0x00000000。 将此值更改为 0x00000001（十六进制 1）以允许调整 gpedit.msc 中的设置。 这可以应用于具有组策略更新的一组 Windows 计算机。