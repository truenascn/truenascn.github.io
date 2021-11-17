# 远程复制任务

在创建远程复制任务之前，在 TrueNAS 中配置 [SSH](https://www.truenas.com/docs/core/system/systemssh/) 和[自动创建数据集快照](https://www.truenas.com/docs/core/tasks/periodicsnapshottasks/)。 这确保了两个系统可以相互连接，并且新的快照可以定期用于复制。

为了简化创建简单复制配置的过程，复制向导会帮助创建新的 SSH 连接并自动为没有现有快照的源创建定期快照任务。

流程总结

- 任务（**Tasks**） > 复制任务（**Replication Tasks**）
  - 选择快照复制的源路径。
    - 远程源需要 SSH 连接。
    - TrueNAS 显示将复制多少快照。
  - 定义快照目标。
    - 远程目标需要 SSH 连接。
    - 选择目的地或通过键入路径手动定义。
      - 在路径末尾添加新名称会创建一个新数据集。
  - 选择复制加密选项。
    - 我们始终建议使用加密进行复制。
    - 禁用加密仅针对绝对安全的网络。
  - 安排复制。
    - 计划可以是标准化的预设或自定义的计划。
    - 运行一次后会立即运行复制。
      - 任务仍然保存并且可以重新运行或编辑。
  - 选择将复制的快照保留多长时间。

## 创建远程复制任务

转至任务（**Tasks**） > 复制任务（**Replication Tasks**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAdd.png)

您可以加载任何已保存的复制任务以使用该配置预填充向导。 保存对配置的更改会创建一个新的复制任务，而不会更改加载到向导中的任务。 在相同的两个系统之间创建多个复制任务时会节省一些时间。

## 选项

### 来源（Sources）

首先配置复制源。 源是具有用于复制的快照的数据集或 zvol。 选择远程源需要选择一个到该系统的 SSH 连接。 展开目录浏览器会显示可用于复制的当前数据集或 zvol。 您可以选择多个来源或在字段中手动输入名称。

TrueNAS 显示有多少快照可用于复制。 我们建议您在创建复制任务之前手动对源进行快照或创建定期快照任务。 但是，当源在本地系统上没有任何现有快照时，TrueNAS 可以创建一个基本的定期快照任务并在开始复制之前对源进行快照。 启用递归复制包含在选定源数据集快照中的所有快照。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddRemoteSource.png)

远程源需要输入快照命名格式来标识要复制的快照。 命名格式是 strftime 时间和日期字符串以及用户可能已添加到快照名称的任何标识符的集合。

本地源也可以使用命名格式来标识要包含在复制任务中的任何自定义快照。

### 目标（Destination）

目标是存储复制快照的地方。 选择远程目标需要到该系统的 SSH 连接。 展开目录浏览器会显示可用于复制的当前数据集。 您可以选择目标数据集或在字段中手动键入路径。 Zvols 不能用作远程复制目标。 在路径末尾添加名称会在该位置创建一个新数据集。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddRemoteDest.png)

![](https://www.truenas.com/docs/images/CORE/12.0/remote_rep_encrypt.png)

加密：要在复制数据时使用加密，请选中加密框。 选中该框后，其他加密选项将可用。

加密密钥格式允许用户在十六进制（基数为 16 的数字）或密码短语（字母数字）样式的加密密钥之间进行选择。
在发送 TrueNAS 数据库中存储加密密钥（**Store Encryption key in Sending TrueNAS database**）允许用户将加密密钥存储在发送 TrueNAS 数据库中（选中框）或为加密密钥选择一个临时位置以解密复制数据（未选中框）。

### 安全和任务名称（Security and Task Name）

始终建议对 SSH 传输安全使用加密。在绝对安全网络中的两个系统用于复制的情况下，禁用加密会加快传输速度。 但是，数据完全不受恶意来源的保护。

为任务选择不加密与从高级选项屏幕中选择 SSH+NETCAT 传输方法相同。 NETCAT 使用通用端口设置，但可以通过切换到高级选项屏幕或在创建后编辑任务来覆盖这些设置。

TrueNAS 根据选定的源和目的地建议一个名称，但这可以用自定义名称覆盖。

### 时间计划表和保存时间（Schedule and Lifetime）

添加计划会根据您选择的时间自动运行任务。 您可以在多个预设计划之间进行选择，也可以为复制运行的时间创建自定义计划。 选择运行一次复制将在保存任务后立即运行复制，但任何额外的复制都必须手动触发。

最后，定义您希望在目标系统上保留快照的时间。 我们通常建议定义快照保存时间，以防止过时的快照使系统混乱。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddLocalSourceLocalDestCustomLife.png)

### 开始复制（Starting the Replication）

启动复制（**Start Replication**）保存新的复制任务。 默认情况下启用新任务，并根据其计划或在未选择计划时立即激活。 复制任务第一次运行时需要更长的时间，因为必须将快照完全复制到目的地。 后来的复制运行得更快，因为只复制对快照的后续更改。 单击任务状态会打开该任务的日志。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksRemoteLogs.png)

