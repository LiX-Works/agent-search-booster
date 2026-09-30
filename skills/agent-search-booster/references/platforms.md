# 平台搜索、正文、评论与浏览器调用

面向Agent。OpenCLI负责把站内操作转成CLI，Browser Bridge连接浏览器和已有平台会话；直接浏览器可补足适配器未提供的页面内容。平台适配器支持范围随版本改变，先查目标帮助。

## 发现与连接

```powershell
opencli list
opencli doctor
opencli xiaohongshu --help -f yaml
opencli reddit read --help
opencli youtube transcript --help
```

doctor只检查桥接连接，不证明目标平台已登录或全文已取得。使用者实际安装的扩展和浏览器配置为准，不预设作者的Chrome配置名、安装路径或账号。需要启动浏览器时复用用户已授权配置，避免扰动现有窗口和其他Agent任务。

## 按场景取得材料

| 场景 | 方法 | 继续读取什么 |
|---|---|---|
| 小红书笔记 | search后取得工具返回的页面链接，note读正文 | comments与必要子回复；图片/视频内容需按任务单独理解 |
| Reddit | search选中帖子，再read | 调整limit/depth/replies/max-length与more展开，核对截断 |
| YouTube | search找到目标，再transcript | 语言轨道与时间段；评论用对应comments子命令 |
| 知乎、微信公众号、V2EX等 | 使用当前列表中的站内搜索/详情，或已授权浏览器 | 正文、分页和相关回复，标题列表不代替内容 |
| X | Grok CLI追原帖/线程；浏览器补充核对 | 作者、日期、上下文、重要回复与引用，见[Grok](grok.md) |

调用示例（参数以当前help为准，不是固定结果数）：

```powershell
opencli xiaohongshu search '主题' --limit 5 --window background -f json
opencli reddit search '主题' --limit 5 --window background -f json
opencli reddit read 'POST_ID_OR_URL' --limit 50 --depth 3 --replies 10 --max-length 5000 --expand-more true --window background -f json
opencli youtube transcript 'VIDEO_URL' --lang en --mode raw --window background -f json
```

小红书note/comments可能需要带页面签名的原始链接。仅使用现有工具/浏览器提供的授权链接完成调用，不在公开报告保存签名、token或账号信息；输出规范化来源链接。缺少必要凭据访问授权时先说明范围并询问。

## 完整内容与覆盖

“完整评论”应检查根评论、子回复、分页终点与返回截断。平台总数可能包含删除、折叠或不可见项；不能仅把limit设很大就宣称读全。保留实际读到的范围和缺项原因。

现成字幕为空时先看轨道、语言与适配器错误；必要时用另一个正常入口确认。额外转录/ASR按具体任务价值和授权决定，不将一个视频项目的规则套到所有平台。

## 连接异常

- Extension not connected：确认扩展启用、目标浏览器配置及桥接；不把重新网页登录当唯一修复。
- Navigation rejected：检查页面与桥接状态，有限重试或换浏览器；不复制作者旧版本补丁为通用修复。
- HTML代替结构化结果或登录页：确认目标材料尚未取得，恢复授权会话或选正常可访问入口。
- 不让多个Agent未经协调操纵同一浏览器会话；不关闭无关用户标签页。

Agent-Reach用于已接通渠道的辅助与管理，不是OpenCLI的别名。读对应help后使用；完整doctor/setup可能探测认证或安装额外程序，先核对行为与当前授权。本文不以Agent-Reach默认执行凭据提取、ASR或新增收费API。
