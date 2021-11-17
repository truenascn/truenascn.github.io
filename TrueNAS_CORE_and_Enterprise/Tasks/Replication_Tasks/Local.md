# 本地复制任务

## 流程总结

要求：在存储（**Storage**） > 池（**Pools**）中创建的存储池和数据集。

转至任务（**Tasks**） > 复制任务（**Replication Tasks**），然后单击添加（**ADD**）

* 选择来源
  * 将源位置设置为本地系统
  * 使用文件浏览器或输入源路径
* 定义目标路径
  * 将目标位置设置为本地系统
  * 选择或手动定义指向快照副本的单个目标位置的路径。
* 将复制计划设置为运行一次
* 定义快照将在目标中存储多长时间
* 单击 开始复制（**START REPLICATION**） 立即对所选源进行快照并将这些快照复制到目标
  * 对话框可能会要求从目标中删除现有快照。 在删除任何内容之前，请确保所有重要的重要数据都受到保护。
  * 单击任务状态会显示该复制任务的日志。

## 使用复制向导进行快速备份

TrueNAS 提供了一个向导来快速配置不同的简单复制场景。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAdd.png)

虽然我们建议将定期计划复制到远程位置作为最佳备份方案，但该向导可以非常快速地创建 ZFS 快照并将其复制到同一TrueNAS系统上的另一个位置。当没有可用的远程备份位置或磁盘有直接的故障危险时，这很有用。

在创建快速本地复制之前，您唯一需要的是存储池中用作复制源的数据集或 zvol，以及（最好）用于存储复制快照的第二个存储池。您可以完全在复制向导中设置本地复制。

要打开复制向导，请转至任务（**Tasks**） > 复制任务（**Replication Tasks**）并单击添加（**ADD**）。将源位置设置为本地系统并选择要快照的数据集。当找不到现有的源快照时，向导会为源创建新的快照。
启用递归复制所选源数据集快照中包含的所有快照。本地源还可以使用命名模式来标识要包含在复制中的任何自定义快照。命名模式是 [strftime](https://www.freebsd.org/cgi/man.cgi?query=strftime) 时间和日期字符串以及用户可能已添加到快照名称的任何标识符的集合。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddLocalSource.png)

将目标设置为本地系统并定义复制快照的存储位置的路径。 手动定义目的地时，请务必键入目的地位置的完整路径。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddLocalSourceLocalDest.png)

TrueNAS 根据选定的源位置和目标位置建议任务的默认名称，但您可以为复制键入自己的名称。 您可以将任何保存的复制任务加载到向导中，以便更轻松地创建新的复制计划。

您可以为此复制定义一个特定的计划或选择在保存新任务后立即运行它。 未计划的任务仍保存在复制任务列表中，可以手动运行或稍后编辑以添加计划。

目标保存期是复制的快照在被删除之前在目标中存储的时间。 我们通常建议定义快照保存时间以防止存储问题。 选择无限期保留快照可能会要求您在目标已满时或在目标已满时手动清除系统中的旧快照。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksAddLocalSourceLocalDestCustomLife.png)

单击 **START REPLICATION** 保存新任务并立即尝试将快照复制到目标。 当 TrueNAS 检测到目的地已经有不相关的快照时，它会要求删除不相关的快照并对新快照进行完整拷贝。 这可能会删除重要数据，因此请确保目标位置不存在任何现有快照或将其备份到其他位置。

简单复制将添加到复制任务列表中，并将显示它当前正在运行。 单击任务状态会显示复制日志，其中包含将日志下载到本地系统的选项。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksLocalLogs.png)

要确认快照已被复制，请转到存储（**Storage**） > 快照（**Snapshots**）并验证目标位置是否具有带有正确时间戳的新快照。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksReplicationTasksLocalSnapshots.png)



