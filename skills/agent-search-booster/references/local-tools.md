# 本地正文、文档、聚合搜索与RSS调用

面向Agent。先识别输入类型，再运行对应已安装工具。输出写当前任务目录，保留原始文件。上游CLI与个人包装脚本分别判断；本仓库不要求作者的docling-local、freshrss-local或searxng-local命令。

## 文档与正文

```powershell
trafilatura -u 'https://example.org/article' --markdown
markitdown 'input.docx' -o 'output.md'
docling --help
docling convert 'input.pdf' --pipeline standard --to md --output 'converted'
```

| 工具 | 优先场景 | 验收 |
|---|---|---|
| Trafilatura | 可访问的文章HTML正文 | 题名、主要段落、日期与原页是否对应 |
| MarkItDown | Office、CSV、普通文本PDF转换 | 原文、字段和表格是否丢失，不据Markdown自动推断视觉结构 |
| Docling | 版面、表格、扫描件与OCR | 复杂公式/表格按原页核对；本地模型需预先准备，避免隐式付费/云模型 |

Docling旧版本可能使用不带convert的入口；按当前help选择语法。需要本地OCR时设置当前支持的引擎与语言，模型/依赖未准备不静默启用云端API。学术PDF取得与来源要求服从专用工作流。

## SearXNG

使用者在自己的实例启用JSON输出并提供实例URL；Agent复用已部署服务，不每题创建容器。例子以占位域名示意：

```powershell
$searchBase = 'https://SEARXNG_INSTANCE'
$query = [Uri]::EscapeDataString('主题')
Invoke-RestMethod "$searchBase/search?q=$query&format=json"
```

检查results中的真实来源与engine/engines，不凭请求参数声称某引擎已返回。聚合引擎零结果、验证码和限流按实际情况处理，可以继续用Exa/Tavily或其他已连接入口。

## FreshRSS

使用者先部署、订阅并准备自己授权的读取入口。Agent通过浏览器/实例提供的导出或API读取已订阅源，并跟回原文章；需要API时按实例启用状态和认证方法调用，不猜endpoint或提取数据库认证配置。

Google Reader兼容API已启用、用户已提供并授权使用API凭据时，可按[官方接口说明](https://github.com/FreshRSS/FreshRSS/blob/edge/docs/en/developers/06_GoogleReader_API.md)读取。下例只用用户预先提供的环境变量，不打印登录响应或认证token：

```powershell
$rssBase = $env:FRESHRSS_BASE_URL.TrimEnd('/')
$rssApi = "$rssBase/api/greader.php"
$loginResult = Invoke-WebRequest -Method Post -Uri "$rssApi/accounts/ClientLogin" -Body @{
    Email = $env:FRESHRSS_USER
    Passwd = $env:FRESHRSS_API_PASSWORD
}
$authLine = ($loginResult.Content -split '\r?\n') | Where-Object { $_.StartsWith('Auth=') } | Select-Object -First 1
if (!$authLine) { throw 'FreshRSS API login did not return an auth token.' }
$rssHeaders = @{ Authorization = "GoogleLogin auth=$($authLine.Substring(5))" }
$subscriptions = Invoke-RestMethod -Uri "$rssApi/reader/api/0/subscription/list?output=json" -Headers $rssHeaders
$articles = Invoke-RestMethod -Uri "$rssApi/reader/api/0/stream/contents/reading-list" -Headers $rssHeaders
$articles.items | Select-Object id,title,published,alternate,summary
```

API密码与网页登录密码分开设置；凭据未提供或API未启用时，使用已授权浏览器读取，而不是自行读取账户数据库。返回分页/continuation时继续按接口读取并记录范围；RSS摘要不完整时跟回alternate中的原文章。

RSS属于已订阅来源的更新追踪，不代替全网搜索。仅更新订阅不等于已读取每篇文章正文。部署、后台刷新和订阅变更是独立操作，按当前任务和授权执行；不默认添加定时任务。

## 工具缺失

缺命令、模型或实例时读[工具清单](tool-catalog.md)与[必要配置](setup.md)，指出缺的是程序、接入还是认证。已存在的服务优先复用，不改变系统Python、不无限重装、不打断其他任务的Docker或浏览器进程。
