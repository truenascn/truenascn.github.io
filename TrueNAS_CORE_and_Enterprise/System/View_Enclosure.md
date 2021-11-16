# 监视机箱

只有 [iXsystems](https://www.ixsystems.com/) 提供的兼容 TrueNAS 硬件和扩展架允许查看监视机箱（**View Enclosure**）选项。 要了解有关可用 iXsystems 产品的更多信息，请参阅 [TrueNAS 系统概述](https://www.truenas.com/systems-overview/)或浏览[硬件文档](https://www.truenas.com/docs/hardware/)。

进入系统（**System**） > 监视机箱（**View Enclosure**）以显示连接的磁盘和硬件的状态。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureM50.png)

屏幕显示主系统。 其他检测到的 TrueNAS 硬件可从屏幕右侧的列中获得。 单击一个机箱以显示有关该硬件的详细信息。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureES102.png)

屏幕分为不同的选项卡。 这些选项卡反映了所选硬件中处于活动状态的传感器。 其中部分或全部可以显示在监视机箱（**View Enclosure**）页面中。 可以通过单击“编辑标签”（**EDIT LABEL**）从任何选项卡重命名系统。

## 磁盘（Disks）

显示 TrueNAS 硬件的图形表示以及有关连接磁盘的详细信息。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureDisksTabOverviewUpdate.png)

单击任何磁盘插槽可查看有关磁盘的特定详细信息，例如设备名称、vdev 分配、功能、驱动器插槽号、序列号和当前驱动器设置。 也可以使用**IDENTIFY DRIVE** 使所选驱动器的识别 LED 闪烁。

磁盘概览（**Disks Overview**）显示有关机箱池、状态和检测到的扩展器的统计信息。 有一些选项可以显示有关机柜中的池、磁盘状态和扩展架状态的更多详细信息。 单击任何按钮可更改图形以显示请求的详细信息。



## 散热（Cooling）

显示每个连接风扇的当前状态和 RPM。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureCoolingTab.png)

## 机箱控制器（Enclosure Services Controller Electronics）

显示机箱状态。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureEncServContElecTab.png)

## 电源（Power Supply）

显示有关每个电源的详细信息。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosurePowerSupplyTab.png)

## SAS连接器（SAS Connector）

显示 SAS 连接器组件的状态。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureSASConnectorTab.png)

## 温度传感器（Temperature Sensor）

显示每个扩展架和磁盘机箱的当前温度。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureTemperatureSensorTab.png)

## 电压传感器（Voltage Sensor）

显示每个传感器、VCCP 和 VCC 的当前电压。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemViewEnclosureVoltageSensorTab.png)

