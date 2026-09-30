# 必要配置与接通检查

供首次准备工具或Agent发现缺项时读取。工具下载入口在[清单](tool-catalog.md)；这里保留MCP代码和会影响调用的连接条件，不重写各网站的安装教程。

## 远程MCP：Codex配置

沿用现有配置管理方式。已有插件或同服务MCP时先检查其工具是否可用，不重复创建入口。下列是无密钥OAuth连接示例，追加到自己的配置而非覆盖整个文件：

```toml
[mcp_servers.exa]
url = "https://mcp.exa.ai/mcp?login"
auth = "oauth"

[mcp_servers.tavily]
url = "https://mcp.tavily.com/mcp"
auth = "oauth"

[mcp_servers.context7]
url = "https://mcp.context7.com/mcp/oauth"
auth = "oauth"

[mcp_servers.firecrawl]
url = "https://mcp.firecrawl.dev/v2/mcp-oauth"
auth = "oauth"
```

配置位置和支持字段见[Codex官方MCP说明](https://learn.chatgpt.com/docs/extend/mcp)。需要认证的入口，再按所用名称登录：

```powershell
codex mcp login exa
codex mcp login tavily
codex mcp login context7
codex mcp login firecrawl
```

服务是否提供免费、套餐或其他额度，以登录账户为准。本文不启用API按量消费、购买或充值；OAuth不是额度无限的保证。Exa官方插件已正常提供所需工具时，可以直接复用插件，不必再建独立MCP。

使用CC Switch时，在对应服务的MCP配置中填入客户端支持的HTTP类型和URL。例如：

```json
{
  "type": "http",
  "url": "https://mcp.context7.com/mcp/oauth"
}
```

CC Switch负责写入/同步连接定义，OAuth认证仍在客户端完成。导出或同步后核对URL、enabled状态及所需工具，避免覆盖已有连接限制。

Jina MCP作为Reader公共HTTP之外的备选入口，配置结构如下；需要时确认所用工具的认证要求再启用：

```toml
[mcp_servers.jina]
url = "https://mcp.jina.ai/v1"
enabled = false
```

## 本地MCP与凭据方式

本清单采用的主要MCP是远程HTTP。Context7等项目另提供本地stdio方式；它是同一服务的另一种接入，不代表必须再装一份：

```toml
[mcp_servers.context7_local]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
```

stdio需要本地运行时，HTTP需要服务地址。原项目规定的API key、认证或版本要求按官方说明处理；不要把远程MCP误当成本地程序。

如果服务明确要求Bearer凭据，可使用环境变量引用，而不是将真实密钥写入公开片段：

```toml
[mcp_servers.service_example]
url = "https://MCP_SERVER_URL"
bearer_token_env_var = "SERVICE_TOKEN"
```

这是配置结构示例，不是本方案新增按量API授权。凭据只留在使用者环境，Skill和报告不保存真实值。

## 会影响调用的前置条件

| 组件 | 必要设置或检查 |
|---|---|
| OpenCLI | 命令可调用；Chrome运行，Browser Bridge启用；目标网站已有授权会话 |
| GitHub CLI | gh可调用；需认证的任务在原客户端登录，权限与任务匹配 |
| Grok Build | grok可调用；用免费/订阅路径登录，核对实际可用模型和额度 |
| 本地文档工具 | 命令与格式依赖已安装；Docling所需本地模型/OCR可用 |
| FreshRSS | 实例和订阅已准备；使用读取API时，在实例启用API并配置用户API认证 |
| SearXNG | 实例可访问，使用/search JSON接口时启用json输出 |
| Docker | 容器部署的服务使用前Engine需运行 |
| 网络代理 | 直连可用时无需设置；需要时采用自己的进程/客户端配置，不复制作者固定端口 |

SearXNG配置中保留已有设置，并在search.formats包含需要的格式：

```yaml
search:
  formats:
    - html
    - json
```

Agent使用用户实例URL，不假设特定本机端口；FreshRSS和SearXNG的实例读取方法见[本地服务](local-tools.md)。

## 发生连接问题时

先区分“没安装、没加载、没登录、目标内容读取失败”。仅按实际错误恢复受影响入口，已有可用项不重新配置。Context7等OAuth回调出现redirect_uri不匹配时，可核对当前Codex帮助及服务注册要求，再使用官方支持的DCR方式：

```powershell
codex mcp login context7 --oauth-client-registration dcr
```

有真实请求成功后才记录该能力接通。本说明是配置模板与必要条件，不是对用户账户或所有网站的运行保证。

## 上游配置来源

[Exa MCP](https://exa.ai/docs/get-started/exa-mcp) · [Tavily MCP](https://github.com/tavily-ai/tavily-mcp) · [Context7](https://context7.com/docs/resources/all-clients) · [Firecrawl MCP](https://docs.firecrawl.dev/mcp-server) · [Jina MCP](https://github.com/jina-ai/MCP) · [SearXNG API](https://docs.searxng.org/dev/search_api.html) · [FreshRSS读取API](https://github.com/FreshRSS/FreshRSS/blob/edge/docs/en/developers/06_GoogleReader_API.md)
