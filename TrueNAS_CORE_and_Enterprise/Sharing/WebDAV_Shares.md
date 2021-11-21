# WebDAV 共享

基于 Web 的分布式创作和版本控制 (WebDAV) 共享使得可以轻易通过 Web 共享 TrueNAS 数据集及其内容。

要创建新共享，请确保存在一个包含需要共享的数据的数据集。

## 共享配置

转至共享（**Sharing**） > WebDAV 共享（**WebDAV Shares**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingWebdavAdd.png)

输入共享名称并使用文件浏览器选择要共享的数据集。 可选的描述有助于识别共享。 为防止用户帐户修改共享数据，请设置只读。

默认情况下，设置了更改用户和组所有权。 这会将共享中所有文件的现有所有权更改为 webdav 用户和组帐户。 默认简化了WebDAV共享权限，但是这可能会导致一些问题，所以web界面会显示警告：

![](https://www.truenas.com/docs/images/CORE/12.0/SharingWebdavAddWarning.png)

当未设置更改用户和组所有权时，不会显示此警告。 在这种情况下，共享文件所有权必须手动设置为 webdav 或 www 用户和组帐户。

默认情况下，新的 WebDAV 共享会立即处于活动状态。 要创建共享但不立即激活它，请取消勾选启用（**Enabled**）。 单击提交（**SUBMIT**）以创建共享。

## 服务激活

创建共享会立即打开一个对话框以激活 WebDAV 服务：

![](https://www.truenas.com/docs/images/CORE/12.0/SharingCreateServiceEnable.png)

要稍后启用或禁用 WebDAV 系统服务，请转到服务（**Services**）并切换 WebDAV。 要在 TrueNAS 启动时自动启动服务，请设置自动启动（**Start Automatically**）。 单击编辑图标以更改服务设置。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesWebdavOptions.png)

为提高数据安全性，请将协议（**Protocol**）设置为 HTTPS。 这需要选择 SSL 证书，但 **freenas_default** 证书始终可用。 所有协议（**Protocol**）选项都需要定义端口（**Port**）号。 确保服务器上没有使用 WebDAV 服务端口。

为防止未经授权访问共享数据，请将 HTTP 身份验证（**HTTP Authentication**）设置为基本（**Basic**）或摘要（**Digest**）并创建新的 Webdav 密码（**Webdav Password**）。

进行任何更改后，请务必单击“保存”（**SAVE**）。

## 连接到 WebDAV 共享

WebDAV 共享数据可从 Web 浏览器访问。 要查看共享数据，请打开一个新的浏览器选项卡并输入 `{PROTOCOL}://{TRUENASIP}:{PORT}/{SHAREPATH}`。 将大括号 `{}` 中的元素替换为您从 WebDAV 共享和服务中选择的设置。 示例：`https://10.2.1.1:8081/newdataset`

当HTTP 身份验证（**HTTP Authentication**） 服务选项设置为基本（**Basic**）或摘要（**Digest**）时，需要用户名和密码。 输入用户名 webdav 和 WebDAV 服务中定义的密码。

