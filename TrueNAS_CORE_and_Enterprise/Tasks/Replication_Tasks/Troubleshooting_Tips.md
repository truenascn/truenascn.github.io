# 故障排除提示

本文包含一些有关调查或解决复制任务问题的建议。

## 复制任务日志

要查看和下载复制任务日志，请转至任务（**Tasks**） > 复制任务（**Replication Tasks**）。 单击复制任务的状态(**state**)。

![](https://www.truenas.com/docs/images/CORE/12.0/RepTaskErrorCORE.png)

![](https://www.truenas.com/docs/images/CORE/12.0/RepTaskLogDownloadCORE.png)

单击“下载日志”(**DOWNLOAD LOGS**)按钮下载日志文件。

## 编辑复制任务

要编辑复制任务，请转至任务（**Tasks**） > 复制任务（**Replication Tasks**）。 单击 > 展开复制任务信息，然后单击 编辑（**EDIT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/RepEditTaskCORE.png)

有关可用字段的说明，请参阅[高级复制任务](https://www.truenas.com/docs/core/tasks/replicationtasks/advanced/)。

## 复制任务警报优先级

要自定义复制任务警报（成功或失败）的重要性和频率，请转至系统（**System**） > 警报设置（**Alert Settings**）并向下滚动到任务（**Tasks**）区域。 设置警告级别（**Warning Level**）和警报通知的发送频率。

![](https://www.truenas.com/docs/images/CORE/12.0/AlertTaskReplication.png)

有关此页面的更多信息，请参阅[警报设置](https://www.truenas.com/docs/core/system/alert/)。

## FAQ

**Q**：如果 Internet 连接中断一段时间，复制是否会从中断处重新启动 - 包括任何中间快照？ **A**：是的。

**Q**：如果一个站点一次修改了很多数据，在下一个开始之前，互联网带宽不足以完成发送快照，那么复制作业会一个接一个地运行而不是相互冲突吗？ **A**：是的。





































