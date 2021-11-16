# TrueNAS用户组

在 TrueNAS 中使用组是管理用户帐户权限的有效方式。 请参阅本章的*用户*节以管理用户。 该界面提供对 UNIX 样式组的管理。 如果网络使用活动目录（域服务），请使用 [Active Directory](https://www.truenas.com/docs/core/directoryservices/activedirectory/) 中的说明导入现有帐户信息。

## 查看现有组

要查看已保存的组，请转到帐户（**Accounts**） > 组（**Groups**）

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsGroupsList.png)

默认情况下，系统内置的组是隐藏的。 要查看内置组，请单击齿轮图标。

## 添加新组

要创建新组，请转至帐户（**Accounts**） > 组（**Groups**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsGroupsAdd.png)

每个组都分配有一个组 ID (**GID**)。 为新组输入 1000 以上的数字。 以后不能更改 GID。 系统服务使用的组必须具有与服务使用的默认端口号匹配的 ID。

接下来，输入一个描述性的组名称。 组名不能以连字符 `(-)` 开头，也不能包含空格、制表符或以下字符：`, : + & # % ^ ( ) ! @~*？ < > =`。

默认情况下，不会勾选 **Permit Sudo** 选项。勾选这个选项将允许组成员使用 sudo 来执行 root 命令。 为了安全，最好禁用此功能。

默认情况下会勾选 Samba 身份验证（**Samba Authentication**）。 这允许将组成员用于 [SMB](https://www.truenas.com/docs/core/sharing/smb/smbshare/) 权限和身份验证以使用Windows SMB访问共享文件。

最后，允许重复 GID（**Allow Duplicate GIDs**） 允许设置重复的组 ID，但会使系统配置变得非常复杂。 建议不要设置此选项。

## 组成员管理

使用组来统一规划和管理用户可以简化设置大量用户帐户的权限和访问这一复杂过程。 要管理组成员，请转至帐户（**Accounts**） > 组（**Groups**），单击选中的组的”>"，然后单击成员（**MEMBERS**）：

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsGroupsMembers.png)

要将用户帐户添加到组中，请在所有用户（**All users**）中选择它们并单击右箭头➡ 。 您也可以使用 CTRL 来同时选择多个用户。

