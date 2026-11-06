# Playwright 浏览器控制

通过 Playwright MCP 与 Playwright Extension，让 Codex 或 Claude Code 操作用户正在使用的真实 Chrome 或 Edge 浏览器，并复用现有标签页和登录状态。

## 适用场景

- 打开网页并读取、总结或提取信息。
- 查看和切换当前浏览器标签页。
- 点击按钮、填写表单、选择选项或上传文件。
- 使用浏览器中已有的登录状态完成操作。
- 检查 Playwright MCP 和 Playwright Extension 是否已配置好。
- 在缺少配置时，引导 Codex 或 Claude Code 用户完成 MCP 和浏览器扩展的安装与连接。

安装并配置完成后，可以直接说：

- “打开这个链接并总结页面内容。”
- “用我已经登录的 Chrome 填写这个表单。”
- “检查我的 Chrome 是否已经可以被 Playwright 控制。”

## 安装

从 [playwright-chrome 仓库](https://github.com/ShengWang1017/playwright-chrome) 克隆或下载文件，将本目录 `control-chrome`（包含 `SKILL.md` 和 `agents/`）复制到使用中的 Agent 技能目录：

- Codex：`~/.codex/skills/control-chrome/`
- Claude Code：`~/.claude/skills/control-chrome/`

Windows 中 `~` 指当前用户目录，例如 `%USERPROFILE%`。

安装后新建一个 Codex 任务或 Claude Code 会话，以加载技能。浏览器控制还需要 Playwright MCP 和 Playwright Extension，可按下面的首次配置流程完成。

## 首次配置流程

Codex 用户可以说：

```text
使用 $control-chrome 检查我的 Chrome 是否已连接；如果还没配置，请按最简流程帮我完成设置。
```

Claude Code 用户可以执行：

```text
/control-chrome 检查我的 Chrome 是否已连接；如果还没配置，请按最简流程帮我完成设置。
```

Agent 会按以下流程处理：

1. 检查 Playwright MCP 是否已经安装。
2. 如果未安装，说明用途并推荐安装，询问用户是否同意；得到同意后再执行对应平台的安装命令：
   - Codex：`codex mcp add playwright -- npx -y @playwright/mcp@latest --extension`
   - Claude Code：`claude mcp add -s user playwright -- npx -y @playwright/mcp@latest --extension`
3. 如果已安装，检查 Playwright Extension 是否已经连接。
4. 如果扩展尚未安装或配置，提示用户安装官方 Playwright Extension：
   <https://chromewebstore.google.com/detail/playwright-extension/mmlmfjhmonkocbjadbfplnigmagldckm>
5. 安装扩展后，在扩展中复制 `PLAYWRIGHT_MCP_EXTENSION_TOKEN` 并发送给当前 Agent。Agent 会直接完成本机 MCP 配置，不在回复中回显该 Token。
6. 配置完成后：
   - Codex 用户刷新或重启 Codex，并新建任务验证连接。
   - Claude Code 用户重新启动当前会话，并通过 `/mcp` 验证连接；如果以 Claude Code 插件方式更新了 Skill，可以运行 `/reload-plugins`。

通常不需要手动运行 MCP 安装命令。直接让当前 Agent 检查连接即可；缺少配置时，它会先询问是否安装。
