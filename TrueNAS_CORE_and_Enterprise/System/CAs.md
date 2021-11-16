# 证书颁发机构（CA）

rueNAS 可以充当证书颁发机构（CA），在加密 SSL 或 TLS 连接到 TrueNAS 系统时，您可以导入现有的 CA，也可以在 TrueNAS 系统上创建 CA 和证书。此证书将出现在下拉列表中支持 SSL 或 TLS 的服务菜单。要添加或导入 CA，请转到系统（**System**） > CA （**CAs**）并单击添加。输入 CA 的名称，然后选择类型。三个类型选项是内部 CA（**Internal CA**）、中间 CA（**Intermediate CA**） 和导入CA（**Import CA**）。每种类型的流程略有不同。根据您所需的类型，使用下面的选项卡跳转到相应的部分。

## 内部 CA

### 标识符和类型

选择内部 CA 作为类型。

如果需要，您可以为 CA 选择配置文件。选择配置文件会自动设置某些选项，例如密钥类型、密钥长度、摘要算法等。如果您想手动设置每个选项，请不要从配置文件（**Profiles**）下拉列表选择文件。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCAIdentifierType.png)

### 证书选项

1、从下拉列表中选择密钥类型（**Key Type**）。我们推荐使用 RSA 密钥类型。
2、选择密钥长度（**Key Length**）。出于安全原因，我们建议至少为 2048。
3、选择摘要算法（**Digest Algorithm**）。我们推荐 SHA256。
4、以天为单位输入 CA 的生命周期（**Lifetime**）以设置 CA 的有效期。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCACertificateOptions.png)

### 填写通用名称

1、输入国家（**Country**）、地区（**Locality**）、组织单位（**Organizational Unit**）（可选）、通用名称（**Common Name**）、州（**State**）、组织（**Organization**）、电子邮件（**Email**）和备用名称（**Subject Alternate Names**）来填写证书的信息。
2、通用名称是[完全限定的主机名 (FQDN)](https://kb.iu.edu/d/aiuv)，并且在证书链中必须是唯一的。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCACertificateSubject.png)

### 基本约束

1、如果您想要基本约束，请勾选上**Basic Constraints**，然后会出现更多选项。
2、设置路径长度（**Path Length**）以确定在有效证书路径中可以跟随此证书的非自行颁发的中间证书的数量。输入 0 允许在证书路径中跟随单个附加证书。
3、选择 **Basic Constraints Config**。您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCABasicConstraints.png)

### 权限密钥标识符

如果您需要一个权限密钥标识符，请将其设置为已启用，然后选择权限密钥配置。 您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCAAuthorityKeyIdentifier.png)

### 密钥算法

扩展密钥算法通常用于终端实体证书。

1、如果要使用扩展密钥算法，请勾选**Extended Key Usage**，然后从下拉列表中选择公钥的用法。 您可以在下拉列表中选择多个用途。
2、如果您想将此扩展标识为证书的关键，请启用关键扩展（**Critical Extension**）。 如果 Usages 包含 ANY_EXTENDED_KEY_USAGE，则不要启用关键扩展。

注意：同时使用 Extended Key Usage 和 Key Usage 扩展要求证书的用途与这两个扩展一致。 有关更多详细信息，请参阅 [RFC 3280，第 4.2.1.13 节](https://www.ietf.org/rfc/rfc3280.txt)。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCertificateKeyUsage.png)

## 中间 CA

选择中间 CA 作为类型。

如果需要，您可以为 CA 选择配置文件。选择配置文件会自动设置某些选项，例如密钥类型、密钥长度、摘要算法等。如果您想手动设置每个选项，请不要从配置文件（**Profiles**）下拉列表选择文件。

![](https://www.truenas.com/docs/images/CORE/12.0/IntermediateCAIdentifierType.png)

### 证书选项

1、从下拉列表中选择密钥类型（**Key Type**）。我们推荐使用 RSA 密钥类型。
2、选择密钥长度（**Key Length**）。出于安全原因，我们建议至少为 2048。
3、选择摘要算法（**Digest Algorithm**）。我们推荐 SHA256。
4、以天为单位输入 CA 的生命周期（**Lifetime**）以设置 CA 的有效期。

![](https://www.truenas.com/docs/images/CORE/12.0/IntermediateCACertificateOptions.png)

### 填写通用名称

1、输入国家（**Country**）、地区（**Locality**）、组织单位（**Organizational Unit**）（可选）、通用名称（**Common Name**）、州（**State**）、组织（**Organization**）、电子邮件（**Email**）和备用名称（**Subject Alternate Names**）来填写证书的信息。
2、通用名称是[完全限定的主机名 (FQDN)](https://kb.iu.edu/d/aiuv)，并且在证书链中必须是唯一的。

![](https://www.truenas.com/docs/images/CORE/12.0/IntermediateCACertificateSubject.png)

### 基本约束

1、如果您想要基本约束，请勾选上**Basic Constraints**，然后会出现更多选项。
2、设置路径长度（**Path Length**）以确定在有效证书路径中可以跟随此证书的非自行颁发的中间证书的数量。输入 0 允许在证书路径中跟随单个附加证书。
3、选择 **Basic Constraints Config**。您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCABasicConstraints.png)

### 权限密钥标识符

如果您需要一个权限密钥标识符，请将其设置为已启用，然后选择权限密钥配置。 您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCAAuthorityKeyIdentifier.png)

### 密钥算法

扩展密钥算法通常用于终端实体证书。

1、如果要使用扩展密钥算法，请勾选**Extended Key Usage**，然后从下拉列表中选择公钥的用法。 您可以在下拉列表中选择多个用途。
2、如果您想将此扩展标识为证书的关键，请启用关键扩展（**Critical Extension**）。 如果 Usages 包含 ANY_EXTENDED_KEY_USAGE，则不要启用关键扩展。

注意：同时使用 Extended Key Usage 和 Key Usage 扩展要求证书的用途与这两个扩展一致。 有关更多详细信息，请参阅 [RFC 3280，第 4.2.1.13 节](https://www.ietf.org/rfc/rfc3280.txt)。

![](https://www.truenas.com/docs/images/CORE/12.0/InternalCertificateKeyUsage.png)

## 导入 CA

### 标识符和类型

选择导入 CA 作为类型。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCAIdentifierType.png)

### 证书主体

复制要导入的 CA 的证书并将其粘贴到“证书”（**Certificate**）字段中。
在可用时粘贴与证书关联的私钥（**Private Key**）。 提供至少 1024 位长的密钥。
输入并确认私钥的密码（**Passphrase**）。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCACertificateSubject.png)