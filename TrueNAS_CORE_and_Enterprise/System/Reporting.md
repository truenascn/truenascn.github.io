# 系统报告

TrueNAS 具有内置的报告引擎，可提供有关系统的有用图表和信息。

TrueNAS 使用 [Graphite](https://graphiteapp.org/) 进行指标收集和可视化。

您可以更改系统（**System**） > 报告（ **Reporting**）中的选项来控制报告中图形的显示方式：

## 常规选项

| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 以百分比报告 CPU 使用率（**Report CPU usage in Percent**）   | 以百分比而不是内核时间单位报告 CPU 使用率。                  |
| 在Graphite中分离实例（**Graphite Separate Instances**）      | 将 *plugin instance* 和 *type instance* 作为单独的路径组件发送到 Graphite：`host.cpu.0.cpu.idle`。 禁用将 *plugin* 和 *plugin instance* 作为一个路径组件发送，*type* 和 *type instance* 作为另一个：`host.cpu-0.cpu-idle`。 |
| 远程 Graphite 服务器主机名（**Remote Graphite Server Hostname**） | 远程 [Graphite](https://graphiteapp.org/) 服务器主机名或 IP 地址。 |
| 显示以月为单位的图形（**Graph age in Months**）              | 最长时间（以月为单位） TrueNAS 存储图表（允许值为 1-60）。 更改此值会导致出现 Confirm RRD Destroy 对话框。 在 TrueNAS 销毁现有报告数据库之前，更改不会生效。 |
| 图点数（**Number of Graph Points**）                         | 每个小时、每日、每周、每月或每年图表的点数（允许值为 1-4096）。 更改此值会显示 **Confirm RRD Destroy** 对话框。 在 TrueNAS 销毁现有报告数据库之前，更改不会生效。 |
| 强制（**Force**）                                            | 强制 TruNAS 添加 NTP 服务器，即使它无法访问。                |

**注意：当CPU报告模式、图表时间或图点数改变时，报告历史会被清除。**

报告数据在系统升级和重新启动时被保存和保留。 这允许查看一段时间内的使用趋势。 此数据经常写入，所以不应存储在启动池或操作系统设备上。 报告数据保存在 /var/db/collectd/rrd/ 中。

