# Apple 共享协议（AFP）

Apple 文件协议 (AFP) 是一种允许通过网络共享文件的网络协议。 它类似于 SMB 和 NFS，但专为 Apple 系统设计。

从 2013 年开始，Apple 开始使用 SMB 共享协议作为文件共享的默认选项，并停止开发 AFP 共享协议。 除非文件需要与旧 Apple 产品共享，否则建议使用 SMB 共享而不是 AFP。 请参阅 https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html

要创建新共享，请确保存在一个包含需要共享的数据的数据集。

## AFP 共享配置

要配置新共享，请转至共享（**Sharing**） > Apple 共享 (AFP) （**Apple Shares (AFP)**）并单击添加（**ADD**）。 由于 AFP 共享已被弃用，请确认您打算创建 AFP 共享。 接下来，使用文件浏览器选择要共享的数据集并输入共享的描述性名称。

当共享有 Time Machine（时光机） 备份时，设置 Time Machine。 这会将共享作为存储 Time Machine 备份的磁盘发布给其他 Mac 系统。 不建议为 Time Machine 备份配置多个 AFP 共享。

勾选用作家庭共享（**Use as Home Share**）会为连接到共享的用户创建主目录。 只有能创建一个基于 AFP 共享的家庭共享。

默认情况下启用 AFP 共享。 要创建共享但不立即启用它，请取消勾选启用（**Enable**）。 单击提交（**SUBMIT**）创建共享。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingAFPAdd.png)

### 高级选项

打开 高级选项（**ADVANCED OPTIONS**） 允许修改共享权限、添加描述和指定任何辅助参数（**Auxiliary Parameters**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingAFPAddAdvanced.png)

要编辑现有的 AFP 共享，请前往共享（**Sharing**） > Apple 共享 (AFP) （**Apple Shares (AFP)**）并单击三个小点（选项） 。

## AFP服务

要开始广播 AFP 共享，请转到服务（**Services**）并切换打开 **AFP** 开关。 要在 TrueNAS 启动后自动启动服务，请勾选自动启动（**Start Automatically**）。

建议使用 AFP 服务的默认设置。 但是，要调整服务设置，请单击笔（编辑） 。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesAFPEdit.png)

## 连接到 AFP 共享

使用 Apple 操作系统连接到共享。 首先，打开 访达（**Finder**） 应用程序，然后单击顶部菜单栏中的前往（**Go**） > 连接到服务器...（**Connect to Server…** ）。 接下来，输入 `afp://{IPofTrueNASsystem}` 并单击连接。 例如，输入 `afp://192.168.2.2` 连接到 `192.168.2.2` 处的 TrueNAS AFP 共享。

![](https://www.truenas.com/docs/images/CORE/AppleAFPConnect.png)

