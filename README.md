# MATLAB / Simulink MCP + Skills 安装、更新与禁用指南

一份面向 AI Agent 的中文操作手册（runbook），用于审计、下载、更新、配置和排查 MATLAB MCP Server、MATLAB Agentic Toolkit 与 Simulink Agentic Toolkit。

本文档强调可追溯更新和明确的启用边界。默认流程只更新文件并保持禁用；只有用户明确指定客户端、MATLAB 安装和所需能力后，Agent 才进入安装与启用流程。

> 作者：离子怪 · MIT License · 最近核验：2026-09-12

[阅读完整指南与可复制提示词](./matlabmcp+skills安装提示词Y.md) · [查看版本与摘要记录](./versions.json) · [查看许可证](./LICENSE)

## 这份文档解决什么问题

MATLAB 的 Agent 集成由多个独立环节组成：MCP 二进制、MATLAB 工具箱、Agentic Toolkit、客户端注册、skills 发现入口，以及 MATLAB 会话初始化。

仅仅下载文件，不代表已经安装；写入客户端配置，不代表已经启用；能输出版本号，也不代表 MATLAB 会话连接或 Simulink 功能已经通过验证。

完整指南把这些状态分开处理，主要解决以下问题：

- 核对官方 Release、固定版本和下载来源，避免使用过时名称或不明安装包。
- 在更新前识别已有配置、skills 链接和 `startup.m` 自动加载逻辑。
- 先备份再修改，并为变更提供清晰的回滚位置。
- 分别处理 Codex、Claude Code、GitHub Copilot 等客户端的配置差异。
- 用可核验的结果报告区分“已下载”“已注册”“已启用”和“已运行验证”。

## 它是什么，不是什么

[`matlabmcp+skills安装提示词Y.md`](./matlabmcp+skills安装提示词Y.md) 是交给 Agent 阅读和执行的操作说明，不是可执行安装器。打开或阅读该文件不会自动下载软件、修改配置或启动 MATLAB。

仓库也不包含 MathWorks 二进制、`.mltbx` 工具箱或第三方 skills。执行时需要由 Agent 从文档记录的官方仓库重新获取，并按实际 Release 信息完成来源和摘要检查。

## 术语与数量说明

| 术语 | 本仓库中的含义 |
| --- | --- |
| MCP Server | 向 Agent 暴露 MATLAB 工具能力的服务程序 |
| Agentic Toolkit | MathWorks 发布的工具、模板、目录和说明集合 |
| Skill | 一份独立的 `SKILL.md` Agent 操作说明 |
| skills catalog | 按领域组织多份 Skill 的目录，不等于已启用列表 |
| 插件或发现入口 | 客户端加载一组能力的注册方式，数量不等于 Skill 数量 |

在本次固定版本中，MATLAB Toolkit 含 152 份独立 `SKILL.md`，Simulink Toolkit 含 31 份。这个数字表示上游源码快照中的 Skill 文件数量，不表示本机安装了 183 个插件，也不表示这些 Skill 已被客户端发现或启用。

## 推荐使用方式

### 1. 准备输入

向 Agent 提供完整指南，并说明本次目标：只审计、只更新并禁用，或者安装并启用。若要求启用，还应给出目标客户端、MATLAB 版本或安装路径，以及需要的 skills 范围。

不要只说“全部装上”。不同 Toolkit 对 MATLAB 版本、产品许可证和会话模式有不同要求，批量启用也会扩大 Agent 的指令发现范围。

### 2. 默认执行：更新文件但保持禁用

首次使用或尚未确定集成范围时，推荐发送：

```text
请按附件执行 MATLAB / Simulink MCP 与 skills 的审计和更新。
本次使用“模式 A：只更新文件，保持禁用”。

请重新核对官方 Release，检查现有 MCP 注册、skills 发现目录和 MATLAB startup.m。
修改前创建备份；不要启动 MATLAB，不要安装 .mltbx，不要连接会话，也不要启用任何 MCP、插件或 Skill。

完成后报告：固定版本、来源、SHA-256、备份位置、实际下载内容、各客户端禁用状态，以及没有执行的运行验证。
```

该模式适合预先准备文件、检查旧配置或评估升级影响。它不应产生“安装成功”或“运行验证通过”的结论。

### 3. 按需执行：安装或更新并启用

确认环境和目标后，可以明确要求模式 B：

```text
请按附件执行“模式 B：完整安装或更新并启用”。
目标客户端：<Codex / Claude Code / GitHub Copilot / 其他>
MATLAB 安装或版本：<实际路径或版本>
需要启用的范围：<具体 Toolkit、skills 分组或能力>

先输出只读检查和变更计划，再备份现有配置。
只启用上面指定的范围；逐项验证 MCP、MATLAB 会话和所需工具箱，并把未验证项目明确列出。
```

尖括号内容必须替换为真实环境信息。文档不授权 Agent 自行猜测 MATLAB 路径、客户端配置键、许可证状态或启用范围。

### 4. 检查交付结果

一次可信的执行至少应说明：

- 使用的官方仓库、Release tag、下载地址和校验结果。
- 修改了哪些文件，备份位于哪里，如何回滚。
- MCP、插件、Skill 发现入口和 `startup.m` 当前是否启用。
- 是否真正启动 MATLAB、连接会话并完成运行验证。
- 因版本、许可证、缺少产品或信息不足而跳过的项目。

如果报告只写“全部安装成功”，却没有区分上述状态，不应将它视为完成验收。

## 执行模式

| 模式 | 默认 | 允许的操作 | 不执行的操作 |
| --- | --- | --- | --- |
| A：文件更新并禁用 | 是 | 只读检查、官方下载、摘要核验、备份、更新本地文件、停用自动发现入口 | 安装工具箱、启用 MCP 或插件、启动 MATLAB、连接会话 |
| B：安装或更新并启用 | 否 | 在用户明确范围内注册客户端、安装所需组件、初始化会话并验证 | 未授权的全量启用、猜测配置、跳过备份和验证 |

## 已核验版本快照

| 组件 | 固定版本 | 发布日期（UTC） |
| --- | --- | --- |
| MATLAB MCP Server | [v0.13.0](https://github.com/matlab/matlab-mcp-server/releases/tag/v0.13.0) | 2026-09-03 |
| MATLAB Agentic Toolkit | [MATK-2026.09.a](https://github.com/matlab/matlab-agentic-toolkit/releases/tag/MATK-2026.09.a) | 2026-09-10 |
| Simulink Agentic Toolkit | [SATK-2026.09.b](https://github.com/matlab/simulink-agentic-toolkit/releases/tag/SATK-2026.09.b) | 2026-09-09 |

这些版本是 2026-09-12 的核验快照，不是永久最新版。执行指南时应重新读取官方 Release；发现新版本后，需要重新检查兼容性、资源名称、摘要和配置方式，不能只替换版本号。

官方 Release asset 提供 digest 时，可以核对官方 SHA-256。源码归档若只有本地计算值，必须标为本地记录，不能写成“官方摘要验证通过”。

## 兼容性边界

指南当前记录的基础门槛为 MATLAB R2021a 或更高版本；连接现有 MATLAB 会话和 Simulink Agentic Toolkit 需要 R2023a 或更高版本。

具体 Skill 还可能依赖额外的 MATLAB 产品、Simulink 产品或许可证。Skill 文件存在只表示上游仓库提供了该说明，不证明当前机器具备对应产品，也不证明运行成功。

## 仓库内容

```text
.
├── README.md                         # 项目介绍与使用入口
├── matlabmcp+skills安装提示词Y.md     # 完整操作手册和可复制提示词
├── versions.json                     # 固定版本、Release 资源与摘要记录
└── LICENSE                           # 本仓库原创内容的 MIT License
```

`versions.json` 用于审计本次核验依据。它不是依赖锁文件，也不会自动阻止上游 Release、资源名称或仓库结构发生变化。

## 安全与可恢复性原则

- 先做只读检查，再确定唯一目标和变更范围。
- 修改配置和启动文件前创建可恢复备份。
- 不打印 token、Cookie、完整认证对象或无关配置。
- 不把下载完成、配置存在和运行成功混为同一状态。
- 结构、路径、版本或目标不明确时停止写入并报告缺失信息。
- 不覆盖来源不明的同名自定义 Skill。

## 许可证与项目关系

Copyright © 2026 离子怪。本仓库原创文档、提示词和版本记录采用 [MIT License](./LICENSE)。

MATLAB、Simulink、MathWorks MCP、Agentic Toolkit 及其内容遵循各自许可证和使用条款，不受本仓库 MIT License 覆盖。本项目由社区维护，与 MathWorks、OpenAI 或 GitHub 没有官方隶属关系。

## 相关链接

- [博客版指南](https://ice11123.github.io/blog_test2/blog/ai-agent%E5%8D%8F%E4%BD%9C%E4%B8%8E%E5%BC%80%E5%8F%91/matlab-simulink-mcp-skills-install/)
- [Codex 切换 API 或账号后的旧任务 Provider 修复指南](https://github.com/ice11123/codex-old-conversation-fix-after-api-account-switch)
- [离子怪的博客](https://ice11123.github.io/blog_test2/)
