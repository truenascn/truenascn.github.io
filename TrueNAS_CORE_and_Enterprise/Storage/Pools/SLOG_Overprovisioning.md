# SLOG 超配置

对SSD SLOG进行超配置对不同的场景很有用。 超配置的最有用的好处是大大延长了 SSD 的使用寿命。 超配置 SSD 会在驱动器上的更多闪存块之间分配写入和擦除的总数。

希捷对超配置 SSD 进行了深度的调查：https://www.seagate.com/tech-insights/ssd-over-provisioning-benefits-master-ti/。

某些 SATA 设备在每个电源循环中只能调整一次大小。 某些 BIOS 可能会在引导期间阻止调整大小，并需要实时重启。

# Web用户界面

要超配置 SLOG 设备，请登录 TrueNAS 并转到系统（**System**） > 高级（**Advanced**）。 输入与新大小对应的预留空间值（以 GB 为单位）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsOptionsLogOverprovision.png)

应用此值时，只要使用 SLOG 设备创建池，就会应用超配置值。 在首先销毁它所属的池并发出完整的电源循环后，如果不运行 disk_resize，则无法将超配置的 SLOG 设备恢复回原始容量。

每个电源循环仅发生一次超配置/欠配置操作。
清除预留空间设置并将其设置为 none 可防止将来的 SLOG 设备过度配置。

## Shell终端

在 Shell 中使用 `disk_resize` 来过度配置。

超配置 SSD 的命令是 `disk_resize {DEVICE} {SIZE}`，其中 *{DEVICE}* 是 SSD 设备名称，*{SIZE}* 是 GiB 或 TiB 中的新配置大小。 示例：`disk_resize ada5 16GB`。 当未指定大小时，它会将配置恢复为设备的完整大小。

