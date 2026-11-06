---
name: control-chrome
description: "Control web pages in the user's real Google Chrome or Edge profile through Playwright MCP and the Playwright Extension from either Codex or Claude Code. Use for browser navigation, page or tab inspection, clicking, typing, screenshots, uploads, and any request to operate a browser. Also use when a user has just installed this skill, explicitly invokes $control-chrome in Codex or /control-chrome in Claude Code, asks how to use it or what to do next, needs to check whether Playwright MCP and the Playwright Extension are configured, or needs guided installation and extension-token setup. In Codex, do not default to the GPT/ChatGPT Chrome Extension, the Codex in-app browser, Computer Use, or a separate automated browser context."
---

# Control Chrome through Playwright MCP

## Welcome new users

When the user invokes this skill without a concrete browser task, says they just installed it, or asks how to use it, give concise onboarding:

- Tell them they can directly ask for browser work, for example: “打开这个链接并总结页面”, “用我已登录的 Chrome 填写这个表单”, or “检查我的 Chrome 是否已准备好”.
- Explain that the skill uses Playwright MCP plus the Playwright Extension to operate their real Chrome or Edge profile.
- In Codex, show `$control-chrome` when an explicit invocation example is useful. In Claude Code, show `/control-chrome`.
- Offer to check the connection now. If they agree, continue with the setup flow below.
- Keep the first response short. Do not front-load installation commands or ask for an extension token unless the checks show that setup is missing.

## Detect the host

Identify whether the current agent host is Codex or Claude Code before running host-specific commands. Prefer the current product and runtime context; do not choose a host merely because both `codex` and `claude` executables are installed on the machine.

- In Codex, use the Codex MCP commands and Codex restart instructions below.
- In Claude Code, use the Claude MCP commands and Claude Code reload or restart instructions below.
- If the current host is genuinely unclear, ask the user whether they are using Codex or Claude Code before changing configuration.

Never run one host's configuration command against the other host.

## Use the required browser route

Use Playwright MCP browser tools backed by the Playwright Extension. Prefer an already configured, working Playwright MCP server; its tool prefix is commonly `mcp__playwright__browser_*`. This route operates the user's real Chrome or Edge profile and preserves their selected tabs and signed-in state.

In Codex, do not initialize the bundled `browser-client.mjs`, the Node REPL browser runtime, the GPT/ChatGPT Chrome Extension, the Codex in-app browser, standalone Playwright, Computer Use, or another browser automation context for the default path. In Claude Code, do not substitute a separately launched Playwright browser or another browser context for the extension-backed real-browser route.

Honor an explicit user choice of another surface. Saying only “Chrome”, “browser”, “open this page”, or “use my logged-in session” does not select another browser route.

## Set up Playwright when needed

Follow this short setup flow:

1. Check whether Playwright MCP browser tools are available. If a CLI check is needed, use only the current host's commands:
   - Codex: `codex mcp get playwright --json` or `codex mcp list`.
   - Claude Code: `claude mcp get playwright` or `claude mcp list`; `/mcp` is also useful inside an interactive Claude Code session.
2. If Playwright MCP is not installed, tell the user it is required and recommended for controlling their real browser, then ask whether they want you to install it. Do not install until they agree.
3. After approval, confirm the current CLI syntax with the matching help command, then install at user scope:
   - Codex:

     ```bash
     codex mcp add playwright -- npx -y @playwright/mcp@latest --extension
     ```

   - Claude Code:

     ```bash
     claude mcp add -s user playwright -- npx -y @playwright/mcp@latest --extension
     ```

4. If Playwright MCP is installed, verify the extension connection with `browser_tabs` or `browser_snapshot`.
5. If the extension is missing or not configured, send one clear setup message that asks the user to install the official Microsoft Playwright Extension:

   `https://chromewebstore.google.com/detail/playwright-extension/mmlmfjhmonkocbjadbfplnigmagldckm`

   In the same message, tell the user: after installation, open the extension, copy `PLAYWRIGHT_MCP_EXTENSION_TOKEN`, and send the token to you so you can complete the configuration. Do not replace this with a vague instruction such as “connect the extension.”
6. When the user sends the token, use it for one local configuration change and do not echo it:
   - In Codex, update the Playwright MCP environment configuration under `[mcp_servers.playwright.env]`, ensuring the command includes `--extension`.
   - In Claude Code, update the user-scoped `playwright` entry created by `claude mcp add` so its `env` contains `PLAYWRIGHT_MCP_EXTENSION_TOKEN`, preserving all other Claude configuration and the existing `npx -y @playwright/mcp@latest --extension` arguments. Respect `CLAUDE_CONFIG_DIR` when set; otherwise use Claude Code's normal user configuration. Validate with `claude mcp get playwright`.
7. Finish with the current host's refresh instructions, then verify with `browser_tabs` or `browser_snapshot`:
   - Codex: ask the user to refresh or restart Codex and begin a new task if required.
   - Claude Code: ask the user to restart the Claude Code session after changing MCP environment configuration, then check `/mcp`. If the skill itself was installed or updated as a Claude Code plugin, `/reload-plugins` reloads plugin components.

If installation fails because Node.js or `npx` is unavailable, explain that specific problem and ask before installing another prerequisite. Keep other troubleshooting proportional to the actual error.

Never silently fall back to another browser surface. If setup cannot be completed, explain the limitation and ask before using an alternative.

## Operate the page

1. Use `browser_tabs` to inspect, select, or create tabs without closing unrelated tabs.
2. Use `browser_navigate` when a target URL is known.
3. Use `browser_snapshot` or `browser_find` to inspect interactive page state. Prefer snapshots over screenshots for locating elements.
4. Use exact targets from the latest snapshot for `browser_click`, `browser_type`, `browser_fill_form`, `browser_select_option`, and other interactions.
5. Use `browser_take_screenshot` only when visual appearance matters.
6. Re-snapshot after navigation or substantial DOM changes before taking another action.
7. Avoid `browser_run_code_unsafe` unless normal Playwright MCP tools cannot complete a necessary, authorized operation.

When authentication blocks navigation, ask the user to sign in in the same browser profile and tell you when it is ready. Do not bypass authentication by substituting search results or another site.

## Protect sensitive actions and the extension token

Proceed with read-only navigation and inspection when they are within the user's request. Confirm immediately before sending or publishing messages, deleting data, making payments or purchases, changing accounts, permissions, or production, uploading or disclosing local files, or taking another irreversible or externally consequential action.

Request the extension token only during the setup flow above. Treat it as a secret: use it only for the requested local MCP configuration, never quote or repeat it, never include it in progress or final responses, and never store it in memory. After configuration, refer to it only as “the extension token” or say that it is configured.

Do not inspect or expose cookies, local storage, passwords, browser profiles, or session stores. Preserve the user's existing tabs and browser state unless changing them is necessary for the requested task.
