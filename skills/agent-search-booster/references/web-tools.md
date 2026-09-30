# 网页、正文、代码与MCP调用

面向Agent。工具接入参数见[必要配置](setup.md)，完整组件见[工具清单](tool-catalog.md)。

## 搜索与正文获取

| 入口 | 怎么用 | 什么时候接下一步 |
|---|---|---|
| 内置搜索 | 用主题、关键名词、语言、时间或域名发现线索 | 来源覆盖不足用Exa/Tavily补搜；只有摘要则读取原页 |
| Exa MCP/插件 | 调用当前schema的search类工具；fetch类工具用于指定URL内容 | 检查返回是否为摘录、字数上限或截断；不足继续原页/正文工具 |
| Tavily MCP | search用于补来源与时间/域名过滤，extract用于选中的URL | 先核对当前暴露的search/extract工具；按任务采用足够深度 |
| SearXNG | 查询已部署实例的/search JSON接口 | 返回中看真实engine/engines；聚合结果仍需读取原文 |
| 浏览器/原页 | 阅读原网页，展开任务相关的折叠或分页 | 动态内容、登录态和页面交互由已授权浏览器处理 |
| Trafilatura | 对可访问HTML提取正文与必要元数据 | 适合文章；若正文依赖动态加载，换浏览器或Firecrawl |
| Firecrawl MCP | 先用scrape单页读取；需要结构化抽取时核对extract schema | 不为一篇文章启动全站crawl；返回内容须查时间、缺页和截断 |
| Jina Reader | 通过Reader读取可访问URL，作为正文补充入口 | HTTP失败或内容不全转其他入口，MCP与公共HTTP分别确认 |

每轮搜索根据资料缺口继续，而非机械“内置失败才允许其他工具”。同主题共享来源。用一个真实搜索和一个正文读取结果区分“找到”与“读到”，不把工具存在或连接成功当业务验收。

本地HTML抽取示例（先核对本机帮助）：

```powershell
trafilatura -u 'https://example.org/article' --markdown
```

Jina公共Reader地址结构为`https://r.jina.ai/https://example.org/article`。只有当前连接可访问时才采用；密钥、付费模式与MCP身份不能由公共样例成功推断。

## 软件库与Context7

1. 从任务取库名和准确版本；已有合法library ID时可按schema使用，否则先resolve-library-id。
2. 再query-docs（或当前对应工具），查询具体API、参数与版本行为，不发只有“帮我查文档”的空问题。
3. 将返回示例与官方文档/仓库相互定位；旧版与最新版不混用。有代码问题时把代码、错误和版本作为查询上下文。

## GitHub CLI

公开仓库页面、代码、issue、PR与Release按任务使用。已授权gh需要认证范围、准确分页或结构化结果时可直接用，控制实际请求量。

```powershell
gh repo view OWNER/REPO --json name,description,url,defaultBranchRef
gh issue view 123 --repo OWNER/REPO --comments
gh release view --repo OWNER/REPO
```

阅读内容与账户写入分开。查看issue/PR的评论时注意分页和不可见内容；普通搜索不要把未知仓库名直接当已证实项目。登录问题先确认命令和认证状态，不输出令牌，不擅自修改认证存储。

## 恢复顺序

- MCP未暴露：先查当前工具发现或连接状态，不假设本机配置意味着本轮已加载。
- 401/403：区分认证、访问范围与目标站点；需要登录时按已有授权处理。
- 429：区分频率限制与额度，不自动转按量收费或轮换账号。
- 正文为空：对照原页和工具返回状态，检查动态加载、登录页、摘录上限；选下一种内容获取入口。
- 网络或TLS：用范围明确的链路检查，不为了排错改系统代理/安全校验。
