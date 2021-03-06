# Power BI 网关概览
>作者：朱剑夫 发布于2019年2月21日

## 网关的工作原理
* 本地数据网关的作用好似一架桥，提供本地数据（不在云中的数据）与 Power BI、Microsoft Flow、逻辑应用以及 PowerApps 服务之间快速且安全的数据传输。
* 你可以同时将单个网关与不同的服务一起使用。 如果使用的是 Power BI 和 PowerApps，可以对它们使用同一个网关。 它依赖于你登录的帐户。
* 组织中的用户可以访问本地数据（他们已经具有该数据的访问授权），但在这些用户可以连接到本地数据源之前，需要安装和配置本地数据网关。 该网关便于云中的用户与你的本地数据源相互进行快速安全的后台通信，然后返回到云。
* 安装和配置网关通常由管理员完成。 它可能要求具备本地服务器的专门知识，在某些情况下可能需要服务器管理员权限。

![Image of gateway](https://docs.microsoft.com/en-us/power-bi/includes/media/gateway-onprem-how-it-works-include/on-prem-data-gateway-how-it-works.png)
让我们首先看一下当用户与连接到本地数据源的元素交互时，会发生什么情况。

1. 如图所示在云中运行的服务（如：Azure Analysis Service、Power BI、Power Apps 等）将创建查询以及本地数据源的加密凭据，并将其发送到队列中以让网关的云服务进行处理。
2. 网关云服务将分析该查询，并将请求推送到 [Azure 服务总线](https://docs.microsoft.com/zh-cn/azure/service-bus-messaging/service-bus-messaging-overview/)。
3. 本地数据网关将对存储在 Azure 服务总线队列中的查询请求进行轮询。
4. 网关获取查询请求、解密凭据并使用这些凭据连接到数据源。
5. 网关将查询发送到数据源执行。
6. 执行的结果从数据源发出，返回到网关，然后到云服务上。 然后，服务将使用该结果。

### 使用本地数据网关，可以：
* 管理数据源
* 合并本地和云数据源
* 组建网关高可用性群集
* 启用单一登录

## 限制和注意事项
* 暂不支持 Azure 信息保护。
* 暂不支持 Access Online。
* 仅当在个人模式下运行网关时，才支持 R 脚本。
## 租户级别管理
* 租户管理员可以查看租户中安装的所有本地数据网关并对其进行管理。 此功能目前以公共预览版提供。 有关详细信息，请参阅 [Power 平台管理中心]()文档。<font color=#FF0000>目前为止该功能为国际版使用，国内支持遥遥无期.</font>
* 或者，如果你是租户管理员，建议你请求组织中的用户将你添加为他们所安装的每个网关的管理员。 这样你便可以通过“网关设置”页或通过 [PowerShell 命令]()管理组织中的所有网关
## 启用出站 Azure 连接
本地数据网关依赖 Azure 服务总线提供云连接，并相应地建立到其关联 Azure 区域的出站连接。 默认情况下，这是你的 Power BI 租户的位置。 查看我的 [Power BI 租户位于何处?](https://powerbi.microsoft.com/documentation/powerbi-admin-where-is-my-tenant-located/) 如果防火墙阻止出站连接，则必须配置防火墙，使其允许从本地数据网关到其关联 Azure 区域的出站连接。 请参阅 [Microsoft Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)详细了解每个 Azure 数据中心 IP 地址范围。
