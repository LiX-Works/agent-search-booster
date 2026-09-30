# 工具组合与依赖对照

用于检查本Skill整合的组件是否准备好。用户依此确认安装/连接，任务执行中的工具组合由Agent负责。工具分类采用本方案的接入方式；同一项目可能还提供其他运行方式。备注中的备选、补充和协作项不重复安装，也不把历史接通情况当作当前用户环境可用。

### 网页与 MCP

| 工具名称 | 工具类型 | 原链接 | 工具作用 | 备注 |
|---|---|---|---|---|
| Exa | 远程 MCP / 插件 | [项目 / 文档](https://exa.ai/docs/get-started/exa-mcp) | 补充搜索来源、语义检索、指定网页读取 | 已有插件或独立 MCP 均可；同一服务不必重复接入 |
| Tavily | 远程 MCP | [项目 / 文档](https://github.com/tavily-ai/tavily-mcp) | 网页搜索、时间/域名过滤、正文抽取 | OAuth 或服务凭据；额度按账户 |
| Firecrawl | 远程 MCP | [项目 / 文档](https://docs.firecrawl.dev/mcp-server) | 网页抓取、解析和结构化抽取 | 额度敏感；按已授权入口使用 |
| Context7 | 远程 MCP | [项目 / 文档](https://github.com/upstash/context7) | 查询软件库和 SDK 文档 | 本方案使用 HTTP/OAuth；也有本地运行方式 |
| Jina Reader | HTTP 正文服务 | [项目 / 文档](https://jina.ai/reader) | 将可访问网页转换为可读正文 | 补充入口；公共 HTTP 与 MCP 分别确认 |
| Jina MCP | 远程 MCP | [项目 / 文档](https://github.com/jina-ai/MCP) | 提供 Reader 等工具的 MCP 接入 | 备选接入；按工具确认认证，历史未作为默认依赖 |

### 平台与 CLI

| 工具名称 | 工具类型 | 原链接 | 工具作用 | 备注 |
|---|---|---|---|---|
| OpenCLI | 本地 CLI | [项目 / 文档](https://github.com/jackwener/OpenCLI) | 站内搜索、帖子、评论、字幕和浏览器读取 | 浏览器类命令依赖 Chrome、Bridge 与站点会话 |
| Browser Bridge | Chrome 扩展 | [项目 / 文档](https://chromewebstore.google.com/detail/opencli/ildkmabpimmkaediidaifkhjpohdnifk) | 连接 OpenCLI 与浏览器 | 需安装并启用；与 OpenCLI 配套 |
| GitHub CLI | 本地 CLI | [项目 / 文档](https://cli.github.com/) | 读取仓库、代码、Issue、PR 与发布资料 | 命令 gh；认证范围按任务与账户 |
| Grok Build | CLI / 辅助 Agent | [项目 / 文档](https://docs.x.ai/build/overview) | X 原帖、讨论串与补充网页研究 | 命令 grok；登录及账户额度需确认 |
| Agent-Reach | 本地 CLI / 渠道管理 | [项目 / 文档](https://github.com/Panniantong/Agent-Reach) | 辅助接通和检查各内容渠道 | 补充组件；实际读取依赖各渠道工具 |

### 本地材料与服务

| 工具名称 | 工具类型 | 原链接 | 工具作用 | 备注 |
|---|---|---|---|---|
| Trafilatura | 本地 CLI | [项目 / 文档](https://github.com/adbar/trafilatura) | 从 HTML 或 URL 提取文章正文 | 适合可直接访问的文章页 |
| MarkItDown | 本地 CLI | [项目 / 文档](https://github.com/microsoft/markitdown) | Office、CSV、PDF 等转 Markdown | 按格式准备依赖；另有 MCP 版本，本方案用 CLI |
| Docling | 本地 CLI / 本地模型 | [项目 / 文档](https://github.com/docling-project/docling) | PDF 版面、表格、扫描件与 OCR | 需准备模型；复杂内容对照原页 |
| FreshRSS | 自托管 RSS 服务 | [项目 / 文档](https://github.com/FreshRSS/FreshRSS) | 集中读取已订阅来源的更新 | 先部署和订阅；API 读取需单独启用 |
| SearXNG | 自托管搜索服务 | [项目 / 文档](https://github.com/searxng/searxng) | 聚合公开搜索引擎结果 | 常用 Docker 部署；JSON 输出需启用 |

### 运行与配置支撑

| 工具名称 | 工具类型 | 原链接 | 工具作用 | 备注 |
|---|---|---|---|---|
| Chrome | 浏览器 | [项目 / 文档](https://www.google.com/chrome/) | 承载 Bridge 和已有站点登录态 | Agent 使用用户实际授权的配置 |
| Node.js | CLI 运行环境 | [项目 / 文档](https://nodejs.org/) | 运行 npm 安装的 OpenCLI 等工具 | 版本按对应工具要求 |
| Python | CLI 运行环境 | [项目 / 文档](https://www.python.org/) | 运行本地抽取与文档工具 | 建议隔离依赖，避免改坏既有项目 |
| uv | 环境 / 工具管理 CLI | [项目 / 文档](https://docs.astral.sh/uv/) | 隔离安装和运行 Python 工具 | 管理工具，不直接提供搜索内容 |
| Docker Desktop | 容器运行环境 | [项目 / 文档](https://docs.docker.com/desktop/) | 运行本地 SearXNG 等容器服务 | 使用容器时 Docker Engine 需运行 |
| CC Switch | 本地配置管理器 | [项目 / 文档](https://github.com/farion1231/cc-switch) | 集中管理 MCP 等客户端配置 | 可沿用原配置方法；不是搜索服务 |

### 协作衔接

| 工具名称 | 工具类型 | 原链接 | 工具作用 | 备注 |
|---|---|---|---|---|
| AGY / Gemini | CLI / 辅助 Agent | [项目 / 文档](https://www.antigravity.google/docs/cli/overview) | 分析已经取得的批量材料 | 补充协作；独立工具环境，需登录 |
| subagent-manager | 配套 Skill | [项目 / 文档](https://github.com/LiX-Works/subagent-manager) | 管理子代理模型、交接和 Grok 额度 | 协作规则可衔接；不影响主搜索入口 |

## Agent检查方法

- MCP/插件：确认当前工具发现中是否暴露所需search/fetch/scrape/query类入口，有授权时选一个任务相关的最小实际调用；配置文件和登录标记不能代替返回结果。
- CLI：用Get-Command/command -v和对应--help确认入口、版本和参数；发现缺失再按原链接指路，不为检查重新安装。
- 浏览器：确认Bridge与用户授权的浏览器配置，必要时OpenCLI doctor；桥连通、站点登录和目标内容取得分别记录。
- 本地服务：使用者提供自己的实例URL，检查可达性和读取接口；不预设作者端口、Docker容器名或包装命令。
- 首次配齐时输出“可用/缺失/需登录/未验收”及能力影响，普通任务只检查将要用到的入口。不要要求用户为每次搜索选择工具。
- 完整组合的依赖可以逐项补齐；尚未配齐时仍能运行现有路线，但不能声称全部能力已启用。

必要MCP代码和运行连接条件见[配置要点](setup.md)。原链接按2026-09-30官方或原项目资料核对；上游变化时重新确认。
