# 静态路由

默认情况下，TrueNAS 系统上没有定义静态路由。 如果需要静态路由才能到达网络的某些部分，请通过转至网络（**Network**） > 静态路由（**Static Routes**）并单击添加（**ADD**）来添加路由。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkStaticRoutesAdd.png)

| 设置选项                  | 值            | 描述                                           |
| ------------------------- | ------------- | ---------------------------------------------- |
| 目的地（**Destination**） | 整数/integer  | 使用 *A.B.C.D/E* 格式，其中 *E* 是 CIDR 掩码。 |
| 网关（**Gateway**）       | 整数/integer  | 输入网络中网关的 IP 地址。                     |
| 描述（**Description**）   | 字符串/string | 描述路线的注释或标识符。                       |

