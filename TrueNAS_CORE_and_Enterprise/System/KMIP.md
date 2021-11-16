# 密钥管理互通协议（KMIP）

注意：KMIP 仅适用于 TrueNAS Enterprise 许可系统。 请联系 iXsystems 销售团队咨询有关购买 TrueNAS Enterprise 许可证的问题。

密钥管理互通协议 (KMIP) 是一种可扩展的客户端/服务器通信协议，用于存储和维护密钥、证书和机密对象。 TrueNAS Enterprise 上的 KMIP 用于将系统集成到现有的集中式密钥管理基础架构中，并使用单一可信源来创建、使用和销毁 SED 密码和 ZFS 加密密钥。

密钥可以在单个服务器上创建，然后由 TrueNAS 检索。 支持封装在密钥、对称和非对称密钥中的密钥。 或者，KMIP 可用于客户端要求服务器加密或解密数据，而客户端无需直接访问密钥。 KMIP 还可用于签署证书。

您需要有一个 KMIP 服务器，其中包含可导入 TrueNAS 的证书颁发机构和证书。 在单独的浏览器选项卡中打开 KMIP 服务器配置，或复制 KMIP 服务器证书字符串和私钥字符串，以便稍后粘贴到 TrueNAS Web 界面中。 这有助于简化 TrueNAS 连接过程。

## 将 TrueNAS 连接到 KMIP 服务器

要将 TrueNAS 连接到 KMIP 服务器，请从 KMIP 服务器导入[证书颁发机构 (CA)](https://www.truenas.com/docs/core/system/cas/) 和[证书](https://www.truenas.com/docs/core/system/certificates/)，然后配置 KMIP 选项。

登录 TrueNAS 网页界面，进入系统（**System**） > CA（**CAs**） 并点击添加（**ADD**）。 在类型下拉菜单中，选择导入 CA（**Import CA**）。 为 CA 输入一个令人难忘的名称，然后将 KMIP 服务器证书和私钥字符串粘贴到相关字段中。 将密码短语留空，然后单击提交。

接下来，转到系统（**System**） > 证书（**Certificates**）并单击添加（**ADD**）。 在类型下拉菜单中，选择导入证书（**Import Certificate**）。 为证书输入一个令人难忘的名称，并将 KMIP 服务器证书和私钥字符串粘贴到相关的 TrueNAS 字段中。 将密码短语（**Passphrase**）留空，然后单击提交。

出于安全原因，强烈建议保护 CA 和证书值。

### 在 TrueNAS 中配置 KMIP

进入系统（**System**） > KMIP（**KMIP**） 完成配置。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemKMIP.png)

输入中央密钥服务器服务器（**Server**）主机名或 IP 地址以及密钥服务器上开放连接端口（**Port**）。 选择刚刚从中央密钥服务器导入的证书（**Certificate**）和证书颁发机构（**Certificate Authority**）。 要检查证书和 CA 链是否正确，请选中验证连接（**Validate Connection**）并单击保存（**SAVE**）。

验证证书链后，选择加密值、SED 密码或 ZFS 数据池加密密钥以移动到中央密钥服务器。 勾选 **Enabled** 以在单击 **SAVE** 后立即开始移动密码和密钥转移。

刷新 KMIP 页面会显示当前的 KMIP 密钥状态（**KMIP Key Status**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemKMIPKeyStatus.png)

要取消挂起的密钥同步，请勾选强制清除（**Force Clear**）并单击保存（**SAVE**）。

