# multi-source-search

一份用于**多源搜索、读取原文和核验来源**的 Codex Skill。它描述怎样按信息缺口选渠道、遇到工具问题怎样继续，以及回答时怎样交代证据范围。第三方搜索、浏览器和平台工具都是可选入口；Skill 本身不安装或登录这些服务。

核心文件在 [`skills/multi-source-search/SKILL.md`](skills/multi-source-search/SKILL.md)。`references/` 分别说明网页/文档工具和平台帖子/评论的读取边界，需要时再看。各人应根据本机实际可用的工具、账户授权和任务要求适配。

本次更新：修改部分指令，加强提示词注入防御，并让判断更精准。正常教程步骤仍可用于研究与用户授权的任务；对明显企图劫持任务的外部指令进行隔离。

## 安装与使用

将 `skills/multi-source-search/` 整个文件夹复制到个人的 `~/.agents/skills/` 下；若目标已有同名 Skill，先自行比较，不直接覆盖。也可以放入特定项目的 `.agents/skills/`，仅供该项目使用。Codex 通常会发现新 Skill；若当前会话未显示，重新打开任务后再检查。[Codex Skill 文档](https://learn.chatgpt.com/docs/build-skills)

需要时在任务中显式提到 `$multi-source-search`，或让 Codex 根据请求自动选用。可先用一个很小的公开资料问题验证搜索、打开原文和引用链路。缺少 Exa、Tavily、OpenCLI 等可选服务时，仍可使用当前环境中已授权的来源。

这是独立 Skill，与 `subagent-manager` 可配合，但无需同时安装。仓库不包含账号、API Key、Cookie 或浏览器登录状态。内容按 [MIT 许可证](LICENSE)开放。
