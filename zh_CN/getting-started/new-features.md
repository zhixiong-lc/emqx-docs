# 全新功能

本章节描述了 EMQX 5.0 及 EMQX 5.1 引入的全新功能。

## Mria 集群架构

支持全新的 Mria 集群架构，在此架构下 EMQX 水平扩展性得到指数级提升，单个集群可以轻松支持 [1 亿 MQTT 连接](https://www.emqx.com/zh/blog/reaching-100m-mqtt-connections-with-emqx-5-0)，这使得 EMQX 5.0 成为目前全球最具扩展性的 MQTT Broker。

<img src="./assets/100m-benckmark.png" alt="100M benchmark" style="zoom:33%;" />

在构建满足用户业务需求的更大规模集群的同时，Mria 架构还能够降低大规模部署下的脑裂风险以及脑裂后的影响，以提供更加稳定可靠的物联网数据接入服务。

立即开始[创建与管理集群](../deploy/cluster/create-cluster.md)。

## 无停机滚动升级

从 EMQX Enterprise 5.1.0版本开始，系统现在支持集群的无缝滚动升级，让您无需任何服务中断就能过渡到新版本，增强了整体系统的可用性和可靠性。

## MQTT over QUIC 支持

EMQX 5.0引入了QUIC 支持（MQTT over QUIC）作为一项实验性功能，并设计了独特的消息传递机制和管理方法。在 EMQX 5.1 中，我们增加了 [QUIC多流](https://www.emqx.com/zh/blog/emqx-newsletter-202302)支持，并且从现在开始将此功能视为“常规可用”。

作为下一代互联网协议 HTTP/3的底层传输协议，[QUIC](https://datatracker.ietf.org/doc/html/rfc9000) 相对于 TCP/TLS 协议，可以为现代移动互联网提供更低的连接开销和消息延迟。因此，EMQX 尝试使用 QUIC 替代 MQTT 的传输层，从而产生了MQTT over QUIC。

为了评估 MQTT over QUIC 并验证它如何改善网络连接，请阅读[使用 MQTT over QUIC](../mqtt-over-quic/getting-started.md)。

{% emqxee %}

## 基于 MQTT 的文件传输

EMQX 5.1 引入了 MQTT 文件传输功能，支持通过 MQTT 协议传输文件。

该功能基于标准的 MQTT 协议扩展实现，无需改造现有的客户端与应用即可进行集成。客户端可以通过 MQTT 协议向特定主题传输文件分片，传输完成后服务端会对分片进行合并，并保存到本地磁盘或导出到兼容 S3 协议的对象存储中。

相比于 HTTP/FTP 协议，MQTT 具有低带宽消耗和资源占用少的特点，能够快速且高效的进行文件传输。统一的物联网数据通道也简化了系统架构，减少应用的复杂性和维护成本。

立即开始使用[基于 MQTT 的文件传输](../file-transfer/introduction.md)。

{% endemqxee %}

## 备份与恢复

EMQX 5.1 新增了一组用于备份与恢复的命令行工具，能够将内置数据库中的数据以及配置文件导出为一个压缩包，也能够将备份的数据和配置文件恢复到新的集群中。

创建备份：

```bash
$ ./bin/emqx ctl data export
...
Data has been successfully exported to data/backup/emqx-export-2023-06-21-14-07-31.592.tar.gz.
```

恢复备份：

```bash
./bin/emqx ctl data import <File>
```

更多信息，请见[备份与恢复](../operations/backup-restore.md)。

## 全新物联网数据集成

EMQX 5.x 的规则引擎在原有 SQL 的基础上集成了 [jq](https://stedolan.github.io/jq/)，支持更多复杂格式 JSON 数据的处理。更多信息详见：[jq 函数](../data-integration/rule-sql-jq.md)。

{% emqxce %}

支持将数据发送到 Webhook，或与外部 MQTT 服务建立双向数据桥接。

{% endemqxce %}

{% emqxee %}

支持双向数据桥接，您可以实时地将物联网数据实时处理并发送到 40 多个云服务和企业系统，或者从其中获取数据，经处理后下发到指定 MQTT 主题中。

{% endemqxee %}

同时，EMQX 5.0 还提供了数据集成可视化查看能力（Flows）。通过 Dashboard 页面，您可以清晰看到设备与云端之间的物联网数据处理和流转步骤。

关于 EMQX 支持的桥接类型以及如何配置，可阅读[数据桥接章节](../data-integration/data-bridges.md)。

## 灵活多样认证授权

改进了认证授权流程，并提供了灵活的使用与配置方式。通过简单配置，无需编写代码即可对接各类数据源与认证服务，为您的物联网应用开启安全防护。

EMQX 5.1 认证授权包括以下特性：

- 支持在 Dashboard 完成整个集群的认证授权配置。
- 支持通过 Dashboard 管理认证凭证与授权数据。
- 支持调整认证器与授权检查器顺序。
- 提供执行速度与次数统计指标，实现认证授权可观测性。
- 允许监听器单独配置认证，更灵活的接入能力。

关于如何在 EMQX 进行认证和授权配置，可阅读[访问控制](../access-control/overview.md)章节。

## 全新 EMQX Dashboard

EMQX 5.1 重新设计了 EMQX Dashboard，在提升视觉体验的同时，也极大提升了 EMQX 的易用性。

全新 Dashboard 包括以下更新：

- 新的 UI/UX 设计，丰富的样式与易于上手的使用交互。
- 优化菜单结构，快速直达访问内容。
- 更丰富的可视化系统，数据和状态一目了然。
- 开箱即用的认证授权配置与管理功能。
- 强大数据集成能力，[Flows](../dashboard/flows.md) 可视化编排让用户能够清晰地看到从设备或客户端流出的数据通过规则引擎处理后的流向。
- 在线配置更新，在 Dashboard 上实现配置热更新。

## 过载保护、速率限制器和桥接缓存队列

新增的**速率限制器**通过提供更精确和分层的速率控制选项来增强客户端接入和消息速率控制能力。它通过在客户端、监听器或节点级别限制客户端行为，确保系统在预期的工作负载下运行。过载保护和速率限制功能的结合，可以防止客户端过载或接收过多的请求流量，确保系统的稳定运行。

5.1 版本还为所有数据桥接添加了一个通用的缓冲队列，允许在压力情况下缓冲生成的消息。当外部资源不可用时（例如网络波动或服务停机），可以将这个缓冲区配置为将消息存储在内存或磁盘缓存中。缓冲的消息将在服务恢复后发送。然而，缓冲区中的请求可能会过期，这与版本4相比是一个很大的区别。如果缓冲数据的量超过了限制，将按照先进先出（FIFO）规则将其丢弃。

## 云原生与 EMQX Operator

水平扩展和弹性集群是云原生应用必须支持的特性。

[EMQX Kubernetes Operator](https://www.emqx.com/en/emqx-kubernetes-operator) 可以充分利用 EMQX 5.x 的 Replicant 节点。您可以使用 Kubernetes 部署（Deployment）部署一个无状态的 EMQX 节点，然后构建支持大规模 MQTT 连接和消息吞吐量的 EMQX 集群。

## 全新网关框架

网关能够实现多协议的接入和设备管理，EMQX 5.1重构了多协议接入的底层架构，使得各个网关功能定义更为清晰：

- **统一的统计和监控指标：** EMQX 5.0 提供了网关/客户端级别的统计指标，例如发送和接收的字节数、消息数量等。
- **独立的连接和会话管理：** 与 EMQX 4.x 不同，网关客户端也在 MQTT 客户端列表下进行管理，EMQX 5.0 为每个网关创建了独立的网关页面，一个客户端 ID 可以在多个网关之间重用。
- **独立的客户端身份验证：** 与 EMQX 4.x 不同，网关身份验证也是在 MQTT 客户端下进行管理，EMQX 5.0 支持为每个网关配置独特的身份验证机制。
- **清晰的规范，易于扩展：** 该框架提供了一套标准的概念和接口，使得自定义网关更加容易。

EMQX 5.0 网关实现多种协议的接入和统一管理，MQTT 之外的协议也能够充分享受 EMQX 强大的数据集成、安全可靠的认证授权，以及极高的水平扩展能力等诸多特性。

## 更多功能更新

### 全新极简配置

配置文件更换为简洁易读的 HOCON 格式配置文件，`emqx.conf` 配置文件默认仅包含常用的配置项，以提高配置文件可读性和可维护性。

### 改进的 REST API

提供符合 OpenAPI 3.0 规范的 REST API，以及清晰且丰富的 API 文档，为您带来更好的使用体验。

### 快速排查故障

提供更多的诊断工具如慢订阅、在线追踪帮助用户快速排查生产环境中的问题。

### 结构化日志

提供更友好的结构化日志以及 JSON 格式日志支持，错误日志包含唯一的 `msg` 方便定位问题原因。

### 更灵活的拓展机制

引入全新的插件架构，能够以独立插件包的形式编译、分发、安装拓展插件；支持配置多个多语言钩子扩展 exhook，并提供完善的状态与指标监控。
