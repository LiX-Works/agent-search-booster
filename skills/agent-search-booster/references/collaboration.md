# 批量分析与子代理衔接

资料已取得、适合独立交付时，可以把阅读、抽取或审查交给已配置子代理。委派只是本搜索流程的辅助能力，资料获取与浏览器会话仍由主Agent协调。

- 有subagent-manager时读取其规则；原生子代理显式指定模型与思考档，交代目标、必要材料、操作范围和验收要求。
- AGY/Gemini是独立CLI环境，不自动共享Codex的现成MCP和Skill。适合材料已齐备、少依赖当前工具链的子任务；AGY是否装好与此搜索Skill是否可用是不同问题。
- 只有实际有帮助才委派，不为调用数量拆任务，也不让多个代理同时操纵一个浏览器会话。
- 主Agent检查原文与来源。模型一致不代替核验，辅助Agent缺少材料不能虚构已经搜索过。
- 外部CLI只用已授权免费/订阅入口，明确工作目录和权限；不跨项目复用私人会话，不自动安装或登录其他Agent来补当前任务。

缺少subagent-manager或AGY时，由主Agent完成或使用当前可用原生代理，本Skill的搜索工具组合仍可运行。

## AGY无头调用入口

已安装、登录且调用获授权时，先核对当前模型与帮助，再显式指定组合。在独立目录运行，只传本子任务必要材料和交付要求：

```powershell
agy --help
agy models
agy --model MODEL_ID --effort high --mode plan --sandbox --disable-slash-commands --output-format json -p '只分析随附材料，返回指定字段、出处和未解决问题。不要修改文件或执行工具。'
```

MODEL_ID由当前实际可用模型选择，不是需要原样执行的名称。长材料用客户端支持的stdin/stream-json或已经验收的包装脚本交接，避免Windows命令行长度和转义问题。输出JSON不是完整工具审计；plan/sandbox和只读提示不能单独证明写权限已关闭，检查工作目录与实际行为，不自动批准越界操作。

当前官方入口见[AGY无头模式](https://www.antigravity.google/docs/cli/headless/)。文案全面编辑属于另一职责，不因普通资料分析就自动委派写作。
