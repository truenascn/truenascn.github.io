# 可调参数

TrueNAS 允许您从 Web 界面添加系统可调参数。 这些可以手动定义，或者 TrueNAS 可以运行自动调整脚本来尝试优化系统。 可调参数用于管理 TrueNAS sysctls、加载程序和 rc.conf 选项。

1、loader ：指定要在启动时传递给内核或加载其他模块的参数。
2、rc.conf : 开启系统服务和守护进程，重启后生效。
3、sysctl ：在系统运行时配置内核参数，一般立即生效。

**注意：添加 sysctl、loader 或 rc.conf 选项是一项高级功能。 sysctl 会立即影响运行 TrueNAS 系统的内核，加载程序可能会对 TrueNAS 系统成功启动的能力产生不利影响。 在测试更改的后果之前，不要在生产系统上创建可调参数。**

## 配置系统可调参数

要配置可调参数，请转至系统（**System**） > 可调参数（**Tunables**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemTunablesAdd.png)

首先，选择要添加或修改的可调参数类型。 输入要配置的加载程序、sysctl 或 rc.conf 变量的名称。

接下来，输入用于[加载程序（**loader**）](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/boot-introduction.html#boot-loader-commands)、[sysctl](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/configtuning-sysctl.html) 或 [rc.conf](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/config-tuning.html) 的值。 可以给出可选的描述。

如果您希望创建系统可调参数但不立即启用它，请取消选中启用（**Enable**）复选框。 配置的可调参数一直有效，直到被删除或未设置启用。

建议在进行 sysctl 更改后重新启动 TrueNAS 系统。 一些sysctl只在系统启动时生效，重启系统保证设置值与运行系统正在使用的值相对应。

添加或编辑默认可调参数时要小心。 更改默认可调参数会使系统无法使用。

## UI 字段参考

| 项目名称                | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 变量（**Variable**）    | 输入要配置的加载器名称、`sysctl` 或 rc.conf 变量。 加载器可调参数用于指定要在引导时传递给内核或加载其他模块的参数。 rc.conf 可调参数用于启用系统服务和守护进程，并且仅在重新启动后生效。 sysctl tunables 用于在系统运行时配置内核参数，一般立即生效。 |
| 值（**Value**）         | 输入要用于 [loader](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/boot-introduction.html#boot-loader-commands)、[sysctl]( https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/configtuning-sysctl.html) 或 [rc.conf](https://www.freebsd.org/doc/en_US. ISO8859-1/books/handbook/config-tuning.html) 变量。 |
| 类型（**Type**）        | 创建或编辑 sysctl 会立即将变量更新为配置的值。 需要重新启动才能应用加载程序或 rc.conf 可调参数。 配置的可调参数一直有效，直到被删除或未设置启用。 |
| 描述（**Description**） | 输入可调参数的说明。                                         |
| 启用（**Enabled**）     | 启用此可调参数。 取消勾选以禁用此可调参数而不删除它。        |

