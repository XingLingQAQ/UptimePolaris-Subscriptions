# Uptime Polaris subscriptions

Import public website and model status into Uptime Polaris with versioned JSON configurations.

Open **Subscriptions → Subscription library**, choose a configuration, fill in its parameters, preview the source, select items, and save. New subscriptions keep public visibility and notifications off until you enable them.

The catalog is available at [catalog.json](https://raw.githubusercontent.com/XingLingQAQ/UptimePolaris-Subscriptions/main/catalog.json). You can also paste a descriptor's raw HTTPS URL into **Import from link** or upload its JSON file.

| Configuration | Parameters | Data |
| --- | --- | --- |
| Uptime Kuma | baseUrl, slug | Public groups, heartbeat history and source 24-hour availability |
| Uptime Polaris websites | baseUrl | Published website status |
| Uptime Polaris models | baseUrl | Published model observations |
| JSON website / model | baseUrl | Custom service or model JSON |
| Statuspage | baseUrl | Current public component status |
| GitHub Status / Cloudflare Status | Address is prefilled | Public service components |

Choose website or model explicitly. A provider's general service status does not measure an individual model. Model subscriptions require AI monitoring to be enabled in Polaris.

## Configure your own source

Start with the JSON website or model descriptor. Set the HTTPS base URL, request path, item identity and status mapping. Supported requests are GET and read-only POST returning JSON. The [schema](schema/subscription.schema.json) describes the accepted fields. [The POST example](examples/json-post-pagination.json) shows authentication, a request body and bounded pagination. [Fixtures](fixtures/) show the corresponding response shapes.

Use stable source IDs. Map probe timestamps to observedAt and status-change timestamps to changedAt. Snapshot sources record observed changes and do not produce synthetic probe counts or SLA. Secret inputs have no defaults; enter them in Polaris, where they are encrypted separately from the descriptor.

Kuma uses status 2 for pending and 3 for maintenance. Its public heartbeat API omits failure details. Adjust the default 300-second observation expiry to exceed the source's actual probe interval.

## Update a subscription

Open the subscription's interface configuration and choose **Check for configuration updates**. Compare changes, run a preview, then apply. Existing subscriptions keep their saved configuration until you apply an update. You can preview and restore the previous configuration. A changed destination requires re-entering credentials.

Keep published version files immutable. To maintain a configuration, add a new version file and update its catalog entry. A verified date applies only to the listed public endpoint and response shape on that date. Unverified generic templates require testing against your own instance.

## 中文使用说明

进入 **订阅 → 订阅仓库**，选择模板或站点，填写参数、预览并勾选条目后保存。也可从外部 HTTPS 链接或本地 JSON 文件导入。网站与模型分别创建订阅，公开展示、监控告警和同步告警默认关闭。

使用自定义接口时，修改请求地址、条目路径、稳定 ID、状态映射和时间字段。认证参数在应用中填写，不写进公开描述文件。支持读取型 POST、认证头、请求体与有界分页；查看 examples 和 fixtures 中的样例。

配置更新需要先比较、预览再应用。本地显示名称、分组和通知配置独立保存。来源同步失败只更新同步状态，不把所有条目判为故障。备份部署时应同时备份数据库和凭据解密所需的密钥文件。

Licensed under [MIT](LICENSE). Public service names identify the source; this repository is not affiliated with those providers.
