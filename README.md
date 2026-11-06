# Playwright Chrome

让 Codex 或 Claude Code 通过 **Playwright MCP + Playwright Extension** 操作你正在使用的 Chrome / Edge，复用现有标签页与登录状态。

本仓库包含 `control-chrome` Agent Skill，以及 Codex 插件元数据。Skill 提供操作流程与配置指引；执行浏览器操作需要另外配置 MCP 服务和浏览器扩展。

## 能做什么

- 打开网页，读取、总结或提取页面内容。
- 查看和切换标签页，点击按钮，填写表单。
- 使用浏览器中已有的登录状态完成任务。
- 检查 Playwright MCP 与扩展的连接，并引导首次配置。

## 安装 Skill

克隆或下载本仓库：

```bash
git clone https://github.com/ShengWang1017/playwright-chrome.git
```

将 `skills/control-chrome` 整个目录复制到相应位置：

| 使用的 Agent | 技能目录 |
| --- | --- |
| Codex | `~/.codex/skills/control-chrome/` |
| Claude Code | `~/.claude/skills/control-chrome/` |

Windows 中 `~` 对应当前用户目录（`%USERPROFILE%`）。安装后新建任务或重启会话，以加载技能。

## 连接浏览器

可以直接让 Agent 检查并引导配置：

**Codex：**

```text
使用 $control-chrome 检查我的 Chrome 是否已连接；未配置时请按最简流程帮我完成设置。
```

**Claude Code：**

```text
/control-chrome 检查我的 Chrome 是否已连接；未配置时请按最简流程帮我完成设置。
```

手动配置时，需要 Node.js / npm（提供 `npx`），并添加对应的 MCP 服务：

```bash
# Codex
codex mcp add playwright -- npx -y @playwright/mcp@latest --extension

# Claude Code
claude mcp add -s user playwright -- npx -y @playwright/mcp@latest --extension
```

安装 [Microsoft Playwright Extension](https://chromewebstore.google.com/detail/playwright-extension/mmlmfjhmonkocbjadbfplnigmagldckm)，并按技能指引在本机配置扩展令牌。令牌和个人 MCP 配置应保留在本机。

## 使用示例

```text
打开这个链接并总结页面。
用我已登录的 Chrome 填写这个表单。
查看当前浏览器的标签页。
```

## 仓库结构

```text
.codex-plugin/plugin.json          Codex 插件元数据
skills/control-chrome/SKILL.md     核心技能说明
skills/control-chrome/README.md    中文使用与配置说明
skills/control-chrome/agents/      Agent 展示信息
```

此版本整理自本地安装的 `playwright-chrome` 插件。核心 `SKILL.md` 保持原样，安装说明已改为从本仓库获取。
