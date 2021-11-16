# 云服务与云同步

要将 TrueNAS 与云存储提供商集成，请在系统上保存云服务帐户凭据。 保存凭据后，云同步任务允许从该云存储提供商发送或接收数据。

## 保存云存储凭据

将数据从 TrueNAS 传输到云需要在系统上保存云存储提供商凭据。

为了最大限度地提高安全性，这些凭据在保存时会进行加密。 但是，这意味着要从 TrueNAS 配置文件恢复任何云凭据，您必须在生成该配置备份时启用导出密码秘密种子。 请记住保护任何备份的 TrueNAS 配置文件。

建议打开另一个浏览器选项卡并登录到您打算与 TrueNAS 链接的云存储提供商帐户。 某些提供程序需要在存储提供程序帐户页面上生成的其他信息。 例如，在 TrueNAS 上保存 Amazon S3 凭证可能需要登录 S3 账户并在安全凭证（*Security Credentials*） > 访问密钥(*Access Keys*)页面上生成访问密钥对。

要保存云存储提供商凭据，请转至系统(**System**) > 云凭据( **Cloud Credentials**)并单击添加(**Add**)。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemCloudCredentialsAddS3.png)

输入凭证名称并选择提供者。 其余选项根据所选的提供者而变化：

### Amazon S3

| 项目名称                                 | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 访问密钥 ID（**Access Key ID**）         | 亚马逊网络服务Key ID。 这是在 [Amazon AWS](https://aws.amazon.com/) 上通过我的帐户（**My account**） >  安全凭证（**Security Credentials** ）> 访问密钥（**Access Keys**）（访问 ID 和访问密钥）找到的。 必须是字母数字，长度介于 5 到 20 个字符之间。 |
| 访问密钥（**Secret Access Key**）        | 亚马逊网络服务密码。 如果无法找到或记住秘密访问密钥，请转到我的帐户（**My account**） >  安全凭证（**Security Credentials** ）> 访问密钥（**Access Keys**）并创建一个新的密钥对。 必须是字母数字且介于 8 到 40 个字符之间。 |
| 最大上传块数（**Maximum Upload Ports**） | 定义分段上传的最大块数。 如果服务不支持 10,000 块 AWS S3 规范，这会很有用。 |

Amazon S3 高级选项

| 项目名称                                       | 描述                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 端点网址（**Endpoint URL**）                   | [S3 API 端点 URL](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteEndpoints.html)。 使用 AWS 时，端点字段可以为空以使用区域的默认端点，并自动获取可用的存储桶。 有关 [Simple Storage Service Website Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_website_region_endpoints target=) 的列表，请参阅 AWS 文档。 |
| 区域（**Region**）                             | [某个地理区域中的 AWS 资源](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html)。 留空以自动检测存储桶的正确公共区域。 输入私有区域名称允许与在该区域中创建的 Amazon 存储桶进行交互。 例如，输入 us-gov-east-1 以发现在东部 [AWS GovCloud](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/whatis.html) 区域中创建的存储。 |
| 禁用的端点区域（**Disable Endpoint Region**）  | 跳过端点 URL 区域的自动检测。 在配置自定义端点 URL 时设置此项。 |
| 用户签名版本 2（**User Signature Version 2**） | 强制使用 [签名版本 2](https://docs.aws.amazon.com/general/latest/gr/signature-version-2.html) 签署 API 请求。 在配置自定义端点 URL 时设置此项。 |



### BackBlaze B2

| 项目名称                        | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| 密钥ID（**Key ID**）            | 由字母数字组成的 [Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) 应用程序密钥 ID。 要生成新的应用程序密钥，请登录 Backblaze 帐户，转到“应用程序密钥”页面，然后添加新的应用程序密钥。 将应用程序 key ID 字符串复制到此字段。 |
| 应用密钥（**Application Key**） | [Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) 应用密钥。 要生成新的应用程序密钥，请登录 Backblaze 帐户，转到“应用程序密钥”页面，然后添加新的应用程序密钥。 将 application Key 字符串复制到此字段。 |

### Box

| 项目名称                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 访问令牌（**Access Token**） | [Box](https://developer.box.com/) 的用户访问令牌。 [访问令牌](https://developer.box.com/reference) 使 Box 能够验证请求属于已授权会话。 示例令牌：T9cE5asGnuyYCCqIZFoWjFHvNbvVqHjl。 |

### DropBox

| 项目名称                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 访问令牌（**Access Token**） | Dropbox 帐户的访问令牌。  将其添加到此处之前在[Dropbox 帐户]()中[生成令牌](https://blogs.dropbox.com/developers/2014/05/generate-an-access-token-for-your-own-account/) 。 |

### FTP

| 项目名称               | 描述                                                     |
| ---------------------- | -------------------------------------------------------- |
| 主机名（**Host**）     | 要连接的 FTP 主机。 示例：ftp.example.com。              |
| 端口（**Port**）       | FTP 端口号。 留空以使用默认端口 21。                     |
| 用户名（**Username**） | FTP 主机系统上的用户名。 此用户必须已存在于 FTP 主机上。 |
| 密码（**Password**）   | 用户帐户的密码。                                         |

### Google Cloud Storage

| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 预先准备的 JSON 服务帐户密钥（**Preview JSON Service Account Key**） | 上传的服务帐号 JSON 文件的内容。                             |
| 选择文件（**Choose File**）                                  | 上传 Google [服务帐户凭据文件](https://rclone.org/googlecloudstorage/#service-account-support)。 该文件是使用 [Google Cloud Platform Console](https://console.cloud.google.com/apis/credentials) 创建的。 |

### Google Drive

| 项目名称                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 访问令牌（**Access Token**）         | 使用 [Google Drive](https://developers.google.com/drive/api/v3/about-auth) 创建的令牌。 访问令牌会定期过期，必须刷新。 |
| 团队云端硬盘 ID（**Team Drive ID**） | 仅在连接到团队云端硬盘时需要。 团队云端硬盘的顶级文件夹的 ID。 |

### HTTP

| 项目名称                  | 描述            |
| ------------------------- | --------------- |
| 统一资源定位符（**URL**） | HTTP 主机 URL。 |

### Hubic

| 项目名称                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 访问令牌（**Access Token**） | [由 Hubic 帐户生成](https://api.hubic.com/sandbox/)的访问令牌。 |

### MEGA

| 项目名称               | 描述                                  |
| ---------------------- | ------------------------------------- |
| 用户名（**Username**） | [MEGA](https://mega.nz/) 账户用户名。 |
| 密码（**Password**）   | [MEGA](https://mega.nz/) 账户密码。   |

### Microsoft Azure Blob Storage

| 项目名称                    | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 账户名（**Account Name**）  | [Microsoft Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account) 帐户名称。 |
| 账户密钥（**Account Key**） | Azure 帐户的 Base64 编码密钥                                 |

### Microsoft OneDrive

| 项目名称                               | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| 访问令牌（**Access Token**）           | Microsoft Onedrive [访问令牌](https://docs.microsoft.com/en-us/onedrive/developer/rest-api/getting-started/authentication)。 登录 Microsoft 帐户以添加访问令牌。 |
| 驱动器列表（**Drives List**）          | 注册到 Microsoft 帐户的驱动器和 ID。 选择驱动器也会填写驱动器 ID 字段。 |
| 云盘账户类型（**Drive Account Type**） | Microsoft 帐户的类型。 登录 Microsoft 帐户会自动选择正确的帐户类型。 选项：个人、企业、Document_Library |
| 驱动器ID（**Drive ID**）               | 唯一的驱动器标识符。 登录 Microsoft 帐户并从“驱动器列表”下拉列表中选择一个驱动器以添加有效 ID。 |

### OpenStack Swift

| 项目名称                                 | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 用户名（**User Name**）                  | 用于登录的 Openstack 用户名。 这是来自 [OpenStack 凭证文件](https://rclone.org/swift/#configuration-from-an-openstack-credentials-file) 的 OS_USERNAME。 |
| API密钥或密码（**API Key or Password**） | Openstack API 密钥或密码。 这是来自 [OpenStack 凭证文件](https://rclone.org/swift/#configuration-from-an-openstack-credentials-file) 的 OS_PASSWORD。 |
| 身份验证URL（**Authentication URL**）    | 服务器的身份验证 URL。 这是来自 [OpenStack 凭证文件](https://rclone.org/swift/#configuration-from-an-openstack-credentials-file) 的 OS_AUTH_URL。 |
| 身份验证版本（**Auth Version**）         | AuthVersion - 可选 - 如果您的身份验证 URL 没有版本（[rclone 文档](https://rclone.org/swift/#standard-options)），则设置为 (1,2,3)。 |
| 身份验证高级选项                         |                                                              |
| 租户姓名(**Tenant Name**)                | 来自 [OpenStack 凭证文件](https://rclone.org/swift/#configuration-from-an-openstack-credentials-file) 的 OS_TENANT_NAME。 |
| 租户ID（**Tenant ID**）                  | 租户 ID - v1 身份验证可选，否则需要此或租户（[rclone 文档]（https://rclone.org/swift/#standard-options））。 |
| 身份验证令牌（**Auth Token**）           | 来自备用身份验证的身份验证令牌 - 可选（[rclone 文档]（https://rclone.org/swift/#standard-options））。 |

OpenStack Swift高级选项

| 项目名称                      | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| 区域名称（**Region Name**）   | 区域名称 - 可选（[rclone 文档]（https://rclone.org/swift/#standard-options））。 |
| 存储地址（**Storage URL**）   | 存储 URL - 可选（[rclone 文档](https://rclone.org/swift/#standard-options)）。 |
| 端点类型（**Endpoint Type**） | 从服务目录中选择的端点类型。 推荐公开，参见[rclone 文档](https://rclone.org/swift/#standard-options)。 |

### pCloud

| 项目名称                                   | 描述                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| 访问令牌（**Access Token**）               | [pCloud 访问令牌](https://docs.pcloud.com/methods/intro/authentication.html)。 这些令牌可能会过期并需要延期。 |
| 主机名（**Hostname**）输入要连接的主机名。 |                                                              |

### SFTP

| 项目名称                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 主机名（**Host**）           | 要连接的 SSH 主机。                                          |
| 算口（**Port**）             | SSH 端口号。 留空以使用默认端口 22。                         |
| 用户名（**Username**）       | SSH 用户名。                                                 |
| 密码（**Password**）         | SSH 用户名帐户的密码。                                       |
| 私钥ID（**Private Key ID**） | 从现有 SSH 密钥对导入私钥，或选择“生成新的”以为此凭证创建新的 SSH 密钥。 |

### WebDav

| 项目名称                           | 描述                                       |
| ---------------------------------- | ------------------------------------------ |
| 统一资源定位符（**URL**）          | 要连接的 HTTP 主机的 URL。                 |
| WebDav服务名（**WebDav Service**） | 正在使用的 WebDAV 站点、服务或软件的名称。 |
| 用户名（**Username**）             | WebDAV 帐户用户名。                        |
| 密码（**Password**）               | WebDAV 帐户密码。                          |

### Yandex

| 项目名称                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 访问密钥（**Access Token**） | Yandex [访问令牌](https://tech.yandex.com/direct/doc/dg-v4/concepts/auth-token-docpage/)。 |

输入所需的身份验证字符串以启用保存凭据。

## 自动认证

某些提供商可以通过登录帐户自动填充所需的身份验证字符串。 要自动配置凭证，请单击登录提供者并输入您的帐户用户名和密码。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemCloudCredentialsOAuthLogin.png)

建议在保存之前验证凭据。