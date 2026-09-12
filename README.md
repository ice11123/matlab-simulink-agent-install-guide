# MATLAB / Simulink MCP + Skills 安装与更新提示词

**作者：离子怪 · 文档许可证：MIT · 核验日期：2026-09-12**

给 AI Agent 使用的中文安装、更新和排错指令，覆盖 Codex、Claude Code 与 GitHub Copilot。支持只下载更新、保持禁用，也支持用户明确要求后的完整启用。

- [完整中文文档与可复制提示词](./matlabmcp+skills安装提示词Y.md)
- [核验版本与下载摘要](./versions.json)
- [MIT 许可证](./LICENSE)

## 本次纠正了什么

旧文档中的 MCP 名称、R2020b 门槛和固定 skills 清单已经过时。当前 MCP 使用 `matlab-mcp-server` 名称；MATLAB 基础功能要求 R2021a 或更高版本，连接现有会话与 Simulink Toolkit 要求 R2023a 或更高版本。MATLAB 与 Simulink 的独立 marketplace 也不应继续套用旧的聚合 marketplace 名称。

本版把下载、注册、启用和运行验证拆开，默认只更新文件。尤其检查遗留 skills 链接和 `startup.m`，避免“没有启动 MCP”却仍在自动加载工具。

## 已核验版本

| 组件 | 固定版本 | 发布日期（UTC） |
| --- | --- | --- |
| MATLAB MCP Server | [v0.13.0](https://github.com/matlab/matlab-mcp-server/releases/tag/v0.13.0) | 2026-09-03 |
| MATLAB Agentic Toolkit | [MATK-2026.09.a](https://github.com/matlab/matlab-agentic-toolkit/releases/tag/MATK-2026.09.a) | 2026-09-10 |
| Simulink Agentic Toolkit | [SATK-2026.09.b](https://github.com/matlab/simulink-agentic-toolkit/releases/tag/SATK-2026.09.b) | 2026-09-09 |

这些版本是核验时的快照，不是永久的“最新版”。以后使用时应先重新检查官方 Release，再决定是否更新固定版本。

## 快速使用

把完整文档附给 Agent，并发送：

```text
请按附件核对当前官方版本并更新我的 MATLAB / Simulink MCP 与 skills。
本次只更新文件，保持未启用；不要启动 MATLAB、安装会自动配置客户端的工具箱、注册新插件或连接会话。
请检查已有 MCP 注册、skills 自动发现目录和 MATLAB startup.m；先备份，再禁用相关自动加载入口。
完成后报告版本、SHA-256、备份位置、禁用状态与尚未进行的运行验证。
```

需要启用时，请另行明确指定客户端、MATLAB 安装路径和需要的 skills 分组。

## 许可边界

Copyright (c) 2026 离子怪。本仓库原创文档和提示词使用 MIT License。本仓库不分发 MATLAB、MathWorks MCP 二进制、工具箱或第三方 skills；这些组件适用其各自许可证与 MathWorks 使用条款，MIT 不覆盖它们。本项目不是 MathWorks 或 OpenAI 的官方产品。

## 相关文档

- [Codex 切换 API/账号后旧对话无法操作修复](https://github.com/ice11123/codex-old-conversation-fix-after-api-account-switch)
- [离子怪的博客](https://ice11123.github.io/blog_test2/)
