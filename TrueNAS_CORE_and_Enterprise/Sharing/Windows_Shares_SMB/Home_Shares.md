# 家庭共享

TrueNAS 为希望使用单个 SMB 共享为每个用户帐户提供个人目录的组织或 SME 提供用作家庭共享（**Use as Home Share**）选项。

用作家庭共享（**Use as Home Share**）功能可用于单个 TrueNAS SMB 共享。 您可以按照 SMB 共享文章中的说明创建其他 SMB 共享，但不能将其它SMB共享用作家庭共享。

## 创建池并加入 Active Directory

首先，转到存储（**Storage**） > 池（**Pools**）并[创建一个池](https://www.truenas.com/docs/core/storage/pools/poolcreate/)。

接下来，[设置](https://www.truenas.com/docs/core/directoryservices/activedirectory/)您希望通过网络与其共享资源的 Active Directory。

## 准备数据集

转到存储（**Storage**） > 池（**Pools**）并打开您刚创建的池中根数据集旁边的三个小点（选项），然后单击添加数据集（**Add Dataset**）。

命名数据集（本文以 Home_Share_Dataset 为例），并设置 共享类型（**Share Type**） 为 SMB。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsOptionsDatasetCreateOurhome.png)

创建数据集后，转到存储（**Storage**） > 池（**Pools**）并打开新数据集旁边的三个小点（选项）。 选择编辑权限（**Edit Permissions**）。

单击组（**Group**）下拉菜单并将所属组更改为您的 Active Directory 的域管理员。

![](https://www.truenas.com/docs/images/CORE/12.0/GroupDomainAdmins.png)

单击“选择 ACL 预设”（**Select an ACL Preset**）并选择“家庭”（**HOME**）。 然后，单击“保存”（**SAVE**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsOptionsEditPermissionsACLPresetHome.png)

## 创建共享

转至共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)**），然后单击添加（**ADD**）。

将 路径（**Path**） 设置为准备好的数据集（例如 Home_Share_Dataset）。

名称会自动更改为与数据集相同。 将此保留为默认值。

将预设目的（**Purpose**）设置为无预设（**No presets**），然后单击高级选项（**ADVANCED OPTIONS**）并选中用作家庭共享（**Use as Home Share**）。 单击提交（**SUBMIT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAddHomeShareExample.png)

单击保存（**SAVE**）并在服务（**Services**）中启用 SMB 服务（**SMB service**）以使共享在您的网络上可用。

## 添加用户

转至帐户（**Accounts**） > 用户（**Users**），然后单击添加（**ADD**）。 创建新的用户名和密码。 默认情况下，用户主目录将以用户帐户名命名，并作为为 Home_Share_Dataset 的新子目录。

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsUsersEditHomeDir.png)

如果现有用户需要访问家庭共享，请转至帐户（**Accounts**） > 用户（**Users**）并编辑现有帐户。

将用户的主目录调整为适当的数据集并为其命名以创建自己的目录。

添加用户帐户并配置权限后，用户可以登录共享并查看与其用户名匹配的文件夹。

