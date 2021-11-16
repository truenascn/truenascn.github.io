# 开/关机脚本

TrueNAS 可以安排命令或脚本在系统启动或关闭时运行。 要创建新脚本，请转到“任务”（**Tasks**）>“开/关机脚本”（**Init/Shutdown Scripts**）并单击“添加”（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksInitShutdownScriptsAdd.png)

| 项目名称                | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 描述（**Description**） | 关于这个脚本的简要介绍。                                     |
| 类型（**Type**）        | 为可执行文件选择命令或为可执行脚本选择脚本。                 |
| 命令（**Command**）     | 输入带有任何选项的命令。 选择 *Script* 后，单击 *folder* 以定义脚本文件的路径。 |
| 时间设置（**When**）    | *Pre Init* 在启动过程的早期，在挂载文件系统和开始网络之后。 *Post Init* 是在启动过程的最后，在 TrueNAS 服务启动之前。 *Shutdown*是在系统关机过程中。 |
| 是否启用（**Enabled**） | 启用此任务。 取消勾选以禁用任务而不删除它。                  |
| 超时设置（**Timeout**） | 在指定的秒数后自动停止脚本或命令。                           |

您还可以在条目中包含命令的完整路径。 计划命令必须在默认路径中。 可以在 Shell 中使用 `which {COMMAND}` 测试路径。 如果可用，将显示命令的路径：

```
[root@freenas ~]# which ls
/bin/ls
```

务必首先测试脚本以验证它是可执行的，并达到预期的结果。 所有开/关机脚本都使用 `sh` 运行。

所有保存的开/关机脚本任务都显示在“任务”（**Tasks**）>“开/关机脚本”（**Init/Shutdown Scripts**）中。 单击任务旁边的三个小点以编辑（**EDIT**）或删除（**DELETE**）该任务。
