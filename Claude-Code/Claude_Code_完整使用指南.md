# Claude Code 完整使用指南

> **一句话概述**：Claude Code 是一个运行在终端里的 AI 编程助手——你可以用自然语言让它帮你写代码、调试 Bug、管理项目，就像和一个资深程序员结对编程。

---

## 阅读说明

> ⚠️ **关于标注**：本文档使用 `⚠️ Claude 官方模型` 标签标注了**仅在使用 Anthropic 官方模型/订阅（Claude Pro/Max/Team/Enterprise 或 Console API）时才可用的功能**。如果你使用的是第三方模型 API（如 DeepSeek、OpenAI 等），这些功能不可用。
>
> 本文档作者当前使用 **DeepSeek API** 接入 Claude Code，因此大部分 Claude 官方模型专属功能仅作简要介绍。
>
> **最后更新**: 2026-06-29 | 基于官方文档 [code.claude.com/docs](https://code.claude.com/docs/en/overview)

---

## 目录

- [零、Claude Code 能力地图](#零claude-code-能力地图)
- [一、核心概念与安装](#一核心概念与安装)
- [二、界面与快捷键](#二界面与快捷键)
- [三、核心命令详解](#三核心命令详解)
- [四、CLI 命令行参考](#四cli-命令行参考)
- [五、工作流与进阶功能](#五工作流与进阶功能)
- [六、常见问题与最佳实践](#六常见问题与最佳实践)
- [七、快速参考手册](#七快速参考手册)

---

## 零、Claude Code 能力地图

```
┌──────────────────────────────────────────────────────────┐
│                   Claude Code 能力地图                     │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐         │
│  │ 项目管理    │  │  记忆系统   │  │  Skill 技能 │         │
│  │ /init      │  │ /memory    │  │ /plan      │         │
│  │ /clear     │  │ CLAUDE.md  │  │ /simplify  │         │
│  │ /compact   │  │ Auto-memory│  │ /debug     │         │
│  │ /resume    │  │            │  │ /batch     │         │
│  └────────────┘  └────────────┘  └────────────┘         │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐         │
│  │ 子代理系统  │  │  后台代理   │  │  Workflow  │         │
│  │ /fork      │  │ /bg        │  │ /deep-res..│ ⚠️      │
│  │ /agents    │  │ /background│  │ /ultraplan │ ⚠️      │
│  │ /batch     │  │ claude ag. │  │ /workflows │ ⚠️      │
│  └────────────┘  └────────────┘  └────────────┘         │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐         │
│  │  Hooks/MCP │  │  Plugins   │  │ 权限与配置  │         │
│  │ 钩子自动化  │  │ 插件扩展    │  │ 权限模式    │         │
│  │ MCP 外部服务│  │ 市场安装    │  │ /permissions│         │
│  │ 数据库/API │  │ 自定义     │  │ /effort    │         │
│  └────────────┘  └────────────┘  └────────────┘         │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐         │
│  │ 代码审查    │  │  远程协作⚠️  │  │  桌面/Web⚠️ │         │
│  │ /code-review│  │ /teleport │  │ Desktop   │         │
│  │ /diff      │  │ /remote-  │  │ Web(claude│         │
│  │ /security  │  │   control │  │ .ai/code) │         │
│  └────────────┘  └────────────┘  └────────────┘         │
│                                                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐         │
│  │ 定时任务⚠️   │  │  IDE 集成  │  │ 计划模式   │         │
│  │ /schedule  │  │ VS Code   │  │ /plan     │         │
│  │ Routines   │  │ JetBrains │  │ 结构化方案  │         │
│  │ 日程自动化  │  │ Cursor    │  │ 分步实施   │         │
│  └────────────┘  └────────────┘  └────────────┘         │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 一、核心概念与安装

### 1.1 什么是 Claude Code？

Claude Code 是 Anthropic 开发的一款**命令行 AI 编程助手**。它运行在你的终端中，可以读取你的代码库、编辑文件、运行命令，并与你的开发工具集成。

**典型场景**：
- "帮我创建一个 React 登录页面" → 自动生成完整代码
- "这段代码为什么报错？" → 分析错误并给出修复方案
- "把项目中所有 `var` 改成 `let`" → 批量修改文件
- "帮我写这个函数的单元测试" → 生成测试代码

### 1.2 支持的使用方式

| 方式 | 说明 | 第三方 API 可用 |
|------|------|:--------:|
| **Terminal CLI** | 终端命令行，功能最完整 | ✅ |
| **VS Code 扩展** | 编辑器内联 diffs、@-mentions | ✅ |
| **Cursor** | Cursor 编辑器集成 | ✅ |
| **JetBrains IDE** | IntelliJ/PyCharm/WebStorm 等插件 | ✅ |
| **Desktop App** ⚠️ | 独立桌面应用，多会话并行 | ❌ 需订阅 |
| **Web (claude.ai/code)** ⚠️ | 浏览器中运行，无本地环境 | ❌ 需订阅 |
| **Slack** ⚠️ | 在 Slack 中 @Claude 路由 Bug 到 PR | ❌ 需订阅 |
| **CI/CD** ⚠️ | GitHub Actions / GitLab CI/CD | ❌ 需订阅 |
| **iOS App** ⚠️ | Claude iOS 应用中的 Code 功能 | ❌ 需订阅 |

### 1.3 安装方式

#### 方式一：原生安装（推荐，自动更新）

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash
```

```powershell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

```cmd
:: Windows CMD
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

> **注意**：Windows 推荐安装 **Git for Windows**，以便 Claude Code 使用 Bash 工具。未安装时回退为 PowerShell。

#### 方式二：Homebrew（macOS）

```bash
brew install --cask claude-code        # 稳定版（约晚一周）
brew install --cask claude-code@latest # 最新版
```

#### 方式三：WinGet（Windows）

```powershell
winget install Anthropic.ClaudeCode
```

#### 方式四：包管理器（Linux）

```bash
# apt (Debian/Ubuntu)
sudo apt install claude-code

# dnf (Fedora/RHEL)
sudo dnf install claude-code

# apk (Alpine)
sudo apk add claude-code
```

### 1.4 第三方 API 接入

使用 DeepSeek 等第三方 API 时，通过环境变量配置：

```bash
# 设置 API 端点和密钥
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_API_KEY="your-deepseek-api-key"
```

### 1.5 核心概念

| 概念 | 通俗解释 |
|------|----------|
| **会话（Session）** | 你和 AI 的一次"对话"。每次启动 Claude Code，就是一个新会话。 |
| **上下文（Context）** | AI 能"记住"的对话内容。越长越了解你在做什么，但消耗更多费用。 |
| **Token** | AI 处理文本的计量单位。中文约 1 汉字 = 1~2 token。 |
| **CLAUDE.md** | 项目的"自我介绍"文件。你写项目规则和偏好，AI 每次对话都会读。 |
| **Skill** | 可复用的工作流定义（SKILL.md），封装特定任务的最佳实践。 |
| **Hook** | 在特定事件发生时自动执行的脚本（如保存文件后自动格式化）。 |
| **MCP** | 模型上下文协议，连接外部数据源（数据库、API、文件系统等）。 |
| **权限模式** | 控制 AI 执行操作时的审批策略（默认、自动、仅计划等）。 |

---

## 二、界面与快捷键

### 2.1 启动

```bash
cd your-project
claude
```

首次使用会提示登录。第三方 API 用户通过环境变量配置后直接进入。

### 2.2 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + C` | 中断当前操作（按一次暂停，连按两次退出） |
| `Ctrl + D` | 退出 Claude Code |
| `Ctrl + R` | 搜索历史命令 |
| `↑ / ↓` | 浏览历史输入 |
| `Tab` | 自动补全文件路径和命令 |
| `Shift + Tab` | 循环切换权限模式 |
| `Ctrl + T` | 切换 TUI 全屏模式 |
| `双击 Esc` | 返回上级菜单 |

### 2.3 权限模式

按 `Shift + Tab` 循环切换：

| 模式 | 说明 |
|------|------|
| **default** | 每次操作都询问 |
| **acceptEdits** | 自动接受文件编辑，命令仍需确认 |
| **plan** | 仅计划模式，不能执行任何操作 |
| **auto** | AI 自动判断低风险操作并执行，关键操作仍询问 |
| **dontAsk** | ⚠️ 不询问直接执行（谨慎使用） |
| **bypassPermissions** | ⚠️ 完全跳过权限提示（最危险） |

---

## 三、核心命令详解

### 3.1 项目与会话管理

#### `/init` — 初始化项目

- **作用**：扫描代码库，生成项目配置文件 `CLAUDE.md`
- **什么时候用**：首次在项目中使用 Claude Code；项目结构发生重大变化后
- **环境变量**：设置 `CLAUDE_CODE_NEW_INIT=1` 启用交互式流程（同时引导配置 Skills、Hooks、记忆文件）

#### `/clear [名称]` — 清空上下文

- **作用**：清空当前对话历史，相当于"重开一局"
- **别名**：`/reset`, `/new`
- **可选参数**：传一个名称标记上一个会话，方便在 `/resume` 中识别

#### `/compact [指令]` — 压缩对话

- **作用**：将长对话压缩成摘要，释放上下文空间
- **可选参数**：指定压缩重点（如 `/compact 重点关注用户认证`）

#### `/resume [会话]` — 恢复会话

- **作用**：按 ID 或名称恢复历史会话，或打开会话选择器
- **别名**：`/continue`

#### `/rename "新名称"` — 重命名当前会话

- **作用**：给当前会话起一个有描述性的名字

#### `/export [文件名]` — 导出对话

- **作用**：将整个对话记录导出为文本文件或复制到剪贴板

#### `/rewind` — 回退

- **作用**：回退对话和/或代码到之前的检查点
- **别名**：`/checkpoint`, `/undo`

#### `/branch [名称]` — 分支对话

- **作用**：将当前对话"分叉"出一个新会话，可并行尝试不同方案。原会话保留，可通过 `/resume` 回到原会话

#### `/cd <路径>` — 切换工作目录

- **作用**：将会话移动到新的工作目录，保留对话缓存
- **注意**：需要 v2.1.169+

#### `/add-dir <路径>` — 添加工作目录

- **作用**：为当前会话添加额外的文件访问目录

#### `/focus` — 专注模式

- **作用**：切换专注视图，仅显示最近一条提示、工具调用摘要和最终回复

#### `/goal [条件|clear]` — 设置目标

- **作用**：设置一个目标，AI 会持续工作直到条件满足。`/goal clear` 清除

#### `/btw <问题>` — 顺便问一下

- **作用**：问一个快速问题，不计入对话历史（不膨胀上下文）

#### `/recap` — 会话摘要

- **作用**：生成当前会话的一行摘要。离开后返回时也有自动摘要

### 3.2 Memory 模块（记忆系统）

三层记忆系统：

```
┌─────────────────────────────────────────────────────┐
│                三层记忆系统                            │
│                                                     │
│  Layer 1: 用户记忆 ~/.claude/CLAUDE.md               │
│          全局生效，所有项目共享 — 个人偏好、代码风格      │
│                                                     │
│  Layer 2: 项目记忆 ./CLAUDE.md                        │
│          当前项目生效 — 项目架构、技术栈、代码规范        │
│                                                     │
│  Layer 3: 自动记忆 (Auto-memory)                     │
│          AI 自动记录重要信息到 ~/.claude/projects/     │
│          <项目哈希>/memory/                          │
│          四种类型：user / project / feedback / reference │
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### `/memory` 命令

- **作用**：查看和编辑当前项目的记忆文件，管理自动记忆和 CLAUDE.md
- **最佳实践**：
  - 用户记忆写个人偏好："用 TypeScript"、"中文注释"
  - 项目记忆写项目规范："测试命令是 npm test"
  - AI 自动记录：在对话中说"记住这个"，AI 会保存到自动记忆

### 3.3 Skill 系统（技能模块）

#### 内置 Skills

| 命令 | 功能 | 第三方 API |
|:-----|:-----|:----------:|
| `/init` | 项目初始化 | ✅ |
| `/plan [描述]` | 直接进入计划模式 | ✅ |
| `/simplify [目标]` | 代码审查与清理（4 个并行审查代理） | ✅ |
| `/debug [描述]` | 调试诊断 | ✅ |
| `/loop [间隔] [提示]` | 循环执行任务 | ✅ |
| `/batch <指令>` | 大规模并行变更（5~30 独立单元+worktree） | ✅ |
| `/run` | 启动项目并验证效果 | ✅ |
| `/verify` | 验证代码修改是否达到预期 | ✅ |
| `/code-review [级别]` | 审查当前 diff 的正确性/清理机会 | ✅ |
| `/security-review` | 安全漏洞审查 | ✅ |
| `/review [PR]` | 审查 GitHub PR | ✅ |
| `/fewer-permission-prompts` | 分析常用操作并创建允许列表 | ✅ |
| `/claude-api` | Claude API 开发助手 | ⚠️ |
| `/deep-research <问题>` | ⚠️ 多角度网络搜索+交叉验证报告 | ❌ 需订阅 |
| `/ultraplan <提示>` | ⚠️ Cloud 深度规划 | ❌ 需订阅 |
| `/ultrareview [PR]` | ⚠️ Cloud 多代理深度审查 | ❌ 需订阅 |

**Skill 的结构**：每个 Skill 由一个 `SKILL.md` 文件定义，包含 frontmatter（name、description、when_to_use）和工作流程指令。

**Skill 存放位置**：
- `~/.claude/skills/` — 全局可用
- `.claude/skills/` — 仅当前项目
- `.claude/workflows/` — ⚠️ 动态工作流脚本

#### ⚠️ 动态工作流（Dynamic Workflows）

需要 Claude Code v2.1.154+ 和 Claude 订阅。动态工作流将编排逻辑编码为 JavaScript 脚本，可并行运行数十到数百个子代理。通过 `/effort ultracode` 开启或直接在提示中包含 `ultracode` 关键词触发。

### 3.4 子代理与并行

| 命令 | 功能 | 第三方 API |
|:-----|:-----|:----------:|
| `/fork <指令>` | 创建一个子代理处理侧线任务（继承完整对话） | ✅ |
| `/agents` | 管理代理配置 | ✅ |
| `/bg [提示]` / `/background` | 当前会话转为后台代理，释放终端 | ✅ |
| `/tasks` | 查看后台所有运行中的任务 | ✅ |
| `/batch <指令>` | 将大规模变更分解为独立单元并行执行 | ✅ |
| `/workflows` | ⚠️ 查看动态工作流进度 | ❌ 需订阅 |

**对比**：

| 方式 | 谁决定下一步 | 规模 | 中断影响 |
|------|:-----------:|:----:|:--------:|
| **子代理**（`/fork`） | Claude 逐轮决定 | 少量 | 重启本轮 |
| **后台代理**（`/bg`） | 独立会话自行运行 | 少量 | 保留 |
| **Agent Teams** ⚠️ | 主代理监督 | 少量并行 | 同伴继续 |
| **Workflows** ⚠️ | JavaScript 脚本 | 数百代理 | 可恢复 |

### 3.5 Hooks 模块（钩子系统）

#### 事件类型

| 事件 | 触发时机 | 典型用途 |
|------|----------|----------|
| `PreToolUse` | 工具执行前 | 安全检查 |
| `PostToolUse` | 工具执行后 | 自动格式化 |
| `UserPromptSubmit` | 用户按下 Enter | 输入预处理 |
| `SessionStart` | 会话开始/恢复 | 注入上下文 |
| `Stop` | Ctrl+C | 防止意外中断 |
| `PreCompact` | 压缩前 | 保存关键信息 |
| `PostCompact` | 压缩后 | 补充上下文 |
| `Notification` | 系统通知 | 桌面提醒 |

#### 配置示例 `.claude/settings.json`

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [{ "type": "command", "command": "bash .claude/hooks/format.sh" }]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash(rm *)",
        "hooks": [{ "type": "command", "command": "bash .claude/hooks/safety-check.sh" }]
      }
    ]
  }
}
```

#### Hook 返回值

| 退出码 | 含义 |
|:------:|------|
| `exit 0` | 成功，允许继续 |
| `exit 1` | 阻止并显示错误 |
| `exit 2` | 静默阻止 |

### 3.6 MCP 服务模块（模型上下文协议）

#### 管理 MCP

```bash
# 添加 HTTP MCP 服务
claude mcp add --transport http my-server https://example.com/mcp

# 添加本地命令 MCP
claude mcp add my-server -- npx my-mcp-package

# 添加时设置环境变量
claude mcp add my-server -e API_KEY=xxx -- node server.js

# 项目级别（团队共享）
claude mcp add my-server -s project -- node server.js

# 列出所有服务
claude mcp list

# 查看服务详情
claude mcp get my-server

# 移除
claude mcp remove my-server

# ⚠️ OAuth 登录/登出（v2.1.186+）
claude mcp login sentry
claude mcp logout sentry

# 从 Claude Desktop 导入
claude mcp add-from-claude-desktop
```

#### 对话中管理

输入 `/mcp` 可以重新连接或管理 MCP 服务。支持 `enable`/`disable` 单个或全部。

#### 常用 MCP

| 服务 | 添加命令 |
|------|----------|
| **GitHub** | `claude mcp add --transport http github https://api.githubcopilot.com/mcp/` |
| **Playwright** | `claude mcp add playwright -- npx @anthropic-ai/mcp-server-playwright@latest` |
| **Filesystem** | `claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /path` |
| **PostgreSQL** | `claude mcp add pg -- npx -y @anthropic/mcp-postgres` |

### 3.7 Plugins 模块（插件系统）

```bash
claude plugin list          # 列出已安装
claude plugin install <name> # 安装
claude plugin enable/disable # 启用/禁用
claude plugin update <name>  # 更新
claude plugin uninstall <name> # 卸载
claude plugin details <name> # 详情
claude plugin marketplace    # 管理市场
```

**注意**：插件和 Skill 不同——Skill 是轻量级 SKILL.md，Plugin 是完整包（可包含多个 Skill + Hook + MCP 配置）。

### 3.8 代码审查

| 命令 | 功能 | 第三方 API |
|:-----|:-----|:----------:|
| `/code-review [级别]` | 审查当前 diff 的正确性/清理。级别: low/medium/high/xhigh/max/ultra | ✅ |
| `/code-review --fix` | 应用审查结果 | ✅ |
| `/code-review --comment` | 以 GitHub PR 内联评论形式发布 | ✅ |
| `/code-review ultra` | ⚠️ Cloud 多代理深度审查（需要订阅） | ❌ |
| `/simplify [目标]` | 仅代码清理（不找 Bug），4 个审查代理并行 | ✅ |
| `/security-review` | 安全漏洞审查 | ✅ |
| `/review [PR]` | 审查 GitHub PR | ✅ |
| `/diff` | 交互式 diff 查看器（未提交变更+逐轮 diff） | ✅ |
| `/ultrareview [PR]` | ⚠️ /code-review ultra 的别名 | ❌ |

### 3.9 ⚠️ 远程与定时任务（需 Claude 订阅）

| 命令 | 功能 |
|:-----|:------|
| `/teleport` | 将 Web 会话拉到本地终端 |
| `/remote-control` / `/rc` | 使当前会话可从其他设备远程控制 |
| `/schedule [描述]` / `/routines` | 创建定时任务（Anthropic 云端执行） |
| `/desktop` | 将会话转移到 Desktop 桌面应用 |
| `/autofix-pr [提示]` | 创建云端会话，监控当前 PR，自动修复 CI 失败 |
| `/web-setup` | 连接 GitHub 账号到 Claude Code Web |

### 3.10 模型与配置

| 命令 | 功能 | 第三方 API |
|:-----|:-----|:----------:|
| `/model [模型]` | ⚠️ 切换 Claude 官方模型 | ❌ |
| `/effort [级别]` | 设置推理努力级别（low/medium/high/xhigh/max/ultracode） | ⚠️ 部分支持 |
| `/config [key=value]` | 打开设置界面（v2.1.181+ 支持直接设置，如 `/config model=sonnet`） | ✅ |
| `/permissions` | 管理工具权限规则 | ✅ |
| `/status` | 查看版本、模型、账户、连接状态 | ✅ |
| `/usage` | 显示成本、用量限制、活动统计 | ✅ |
| `/plugins` | 管理插件 | ✅ |
| `/hooks` | 查看 Hook 配置 | ✅ |
| `/skills` | 列出可用 Skill | ✅ |
| `/safe-mode` | ⚠️ 启动时禁用所有自定义配置以排查问题 | ✅ |

### 3.11 其他实用命令

| 命令 | 功能 |
|:-----|:------|
| `/help` | 显示帮助和可用命令 |
| `/exit` / `/quit` | 退出。后台会话中仅分离，会话继续运行 |
| `/doctor` | 诊断安装和配置问题 |
| `/copy [N]` | 复制上次响应到剪贴板；数字指定第 N 次之前的 |
| `/color [颜色]` | 设置提示栏颜色（red/blue/green/yellow/purple/orange/pink/cyan） |
| `/theme` | 更改颜色主题（含色盲友好主题和自定义主题） |
| `/keybindings` | 打开快捷键配置文件 |
| `/terminal-setup` | 配置终端快捷键（VS Code/Cursor 等需配置 Shift+Enter） |
| `/tui [default\|fullscreen]` | 切换终端 UI 渲染模式 |
| `/voice` | ⚠️ 语音输入（需 Claude.ai 账号） |
| `/feedback` | 提交反馈或 Bug 报告 |
| `/powerup` | 交互式的 Claude Code 功能学习教程 |
| `/insights` | ⚠️ 分析你的 Claude Code 使用报告 |
| `/team-onboarding` | ⚠️ 生成团队接入指南 |
| `/release-notes` | 查看更新日志 |
| `/sandbox` | ⚠️ 切换沙箱模式（仅支持平台） |
| `/statusline` | 配置 Claude Code 状态栏 |

---

## 四、CLI 命令行参考

### 4.1 常用 CLI 命令

```bash
claude                          # 启动交互式会话
claude "查询"                   # 启动并发送初始提示
claude -p "查询"                # 非交互模式，执行后退出
cat file | claude -p "处理"     # 管道输入
claude -c                       # 继续最近会话
claude -r "<会话>" "查询"       # 恢复指定会话
claude -v                       # 查看版本
claude update                   # 更新到最新版
claude install [版本]            # 安装特定版本
```

### 4.2 身份认证

```bash
claude auth login                # 登录 Anthropic 账号
claude auth login --console      # 使用 Console API 计费
claude auth login --sso          # SSO 登录
claude auth logout               # 退出登录
claude auth status               # 查看登录状态
claude setup-token               # 生成 CI 长期令牌（需订阅）
```

### 4.3 后台代理管理

```bash
claude agents                    # 查看和管理后台代理会话
claude agents --json             # JSON 格式输出
claude attach <id>               # 附加到后台会话
claude logs <id>                 # 查看后台会话输出
claude stop <id>                 # 停止后台会话
claude respawn <id>              # 重启后台会话（保留对话）
claude rm <id>                   # 移除后台会话记录
claude daemon status             # 查看后台会话守护进程状态
claude daemon stop --any         # 停止守护进程
```

### 4.4 MCP 管理

```bash
claude mcp list                  # 列出所有 MCP 服务
claude mcp add ...               # 添加服务
claude mcp get <name>            # 查看服务详情
claude mcp remove <name>         # 移除
claude mcp login <name>          # ⚠️ OAuth 登录（v2.1.186+）
claude mcp logout <name>         # ⚠️ OAuth 登出
```

### 4.5 其他 CLI 命令

```bash
claude project purge [路径]      # 清除项目本地缓存
claude remote-control --name ""  # ⚠️ 启动远程控制服务
claude ultrareview [目标]        # ⚠️ 非交互式深度审查
claude --teleport                # ⚠️ 拉取 Web 会话到本地
```

### 4.6 重要 CLI 标志

| 标志 | 说明 | 第三方 API |
|:-----|:-----|:----------:|
| `-p "查询"` | 非交互模式 | ✅ |
| `-c` | 继续最近会话 | ✅ |
| `-r <会话>` | 恢复指定会话 | ✅ |
| `-n "名称"` | 设置会话显示名称 | ✅ |
| `--model` | ⚠️ 指定模型 | ❌ |
| `--effort [级别]` | 设置努力级别 | ⚠️ 部分支持 |
| `--permission-mode [模式]` | 设置权限模式 | ✅ |
| `--bg` | 后台启动 | ✅ |
| `--agent <名称>` | 使用自定义代理 | ✅ |
| `--agents '{"name":{...}}'` | 动态定义子代理 | ✅ |
| `--allowed-tools` | 设置免审批工具 | ✅ |
| `--disallowed-tools` | 设置禁止工具 | ✅ |
| `--tools` | 限制可用工具 | ✅ |
| `--worktree / -w` | 在隔离的 Git Worktree 中启动 | ✅ |
| `--bare` | 极简模式（跳过所有自发现） | ✅ |
| `--safe-mode` | 安全模式（禁用自定义） | ✅ |
| `--settings <path>` | 指定设置文件 | ✅ |
| `--mcp-config <path>` | 加载 MCP 配置文件 | ✅ |
| `--strict-mcp-config` | 仅使用 --mcp-config 中的服务 | ✅ |
| `--append-system-prompt "文本"` | 附加系统提示 | ✅ |
| `--system-prompt "文本"` | 替换整个系统提示 | ✅ |
| `--system-prompt-file <路径>` | 从文件加载系统提示 | ✅ |
| `--max-budget-usd <金额>` | 设置最大花费 | ✅ |
| `--max-turns <数字>` | 限制最大轮次 | ✅ |
| `--json-schema '{schema}'` | ⚠️ 结构化 JSON 输出 | ✅ |
| `--output-format json` | JSON 输出格式 | ✅ |
| `--verbose` | 详细日志输出 | ✅ |
| `--debug [分类]` | 调试模式 | ✅ |
| `--chrome` | ⚠️ Chrome 浏览器集成 | ❌ |
| `--remote` | ⚠️ 创建 Web 会话 | ❌ |
| `--remote-control` | ⚠️ 启用远程控制 | ❌ |
| `--teleport` | ⚠️ 拉取 Web 会话 | ❌ |
| `--teammate-mode` | ⚠️ 代理团队显示模式 | ❌ |
| `--channels` | ⚠️ 通道通知 | ❌ |
| `--fork-session` | 恢复时创建新会话 ID | ✅ |
| `--from-pr <PR>` | 恢复链接到 PR 的会话 | ✅ |
| `--tmux` | 在 tmux 中启动 worktree | ✅ |
| `--ax-screen-reader` | 屏幕阅读器友好输出 | ✅ |
| `--fallback-model <模型>` | ⚠️ 备用模型链 | ❌ |
| `--init` / `--maintenance` | 运行 Setup hooks | ✅ |
| `--replay-user-messages` | 回显用户消息 | ✅ |
| `--exclude-dynamic-system-prompt-sections` | 优化提示缓存复用 | ✅ |
| `--no-session-persistence` | 不持久化会话 | ✅ |
| `--plugin-dir <路径>` | 加载本地插件目录 | ✅ |
| `--plugin-url <URL>` | 从 URL 加载插件 | ✅ |
| `--session-id <UUID>` | 指定会话 ID | ✅ |
| `--input-format <格式>` | 指定输入格式 | ✅ |
| `--prompt-suggestions` | 预测下一条用户提示 | ✅ |
| `--dangerously-skip-permissions` | 跳过所有权限提示 | ✅ |
| `--allow-dangerously-skip-permissions` | 允许在模式切换中启用 | ✅ |
| `--enable-auto-mode` | v2.1.111 已移除，auto 模式默认在 Shift+Tab 中 | — |
| `--disable-slash-commands` | 禁用所有命令和技能 | ✅ |
| `--init-only` | 仅运行 Setup hooks 后退出 | ✅ |

---

## 五、工作流与进阶功能

### 5.1 ⚠️ 定时任务（Routines）

需 Claude 订阅。Routines 在 Anthropic 管理的基础设施上执行，即使你的计算机关闭也能运行：

```bash
# CLI 中创建
/schedule 每天早上9点审查我的 PR
```

Routines 也可以通过 API 调用或 GitHub 事件触发。Desktop 应用支持本地定时任务。CLI 中的 `/loop` 用于会话内快速轮询。

### 5.2 ⚠️ 远程控制与 Teleport

需 Claude 订阅。

```bash
# 从手机/浏览器继续本地会话
claude --remote-control "项目名称"

# 将 Web 会话拉到本地终端
claude --teleport

# CLI 中启用远程控制
/remote-control
```

### 5.3 ⚠️ 动态工作流

需 Claude Code v2.1.154+ 和 Claude 订阅。

```bash
# 方式一：在提示中包含 ultracode
ultracode: 审查 src/routes/ 下所有 API 端点的认证检查

# 方式二：设置 ultracode 推理级别
/effort ultracode

# 查看运行进度
/workflows
```

### 5.4 计划模式

```bash
# 直接进入计划模式
/plan 修复用户认证模块的性能问题
```

计划模式下，AI 只能读取文件和制定方案，不能执行任何操作。适合在开始大改之前先确认方案。

### 5.5 完整开发工作流

```
开始 → /init（首次）→ /plan（制定方案）
  → AI 实现/修改代码 → /code-review（审查）
  → /batch（大规模并行变更）
  → /diff（查看变更）→ git commit
  → /verify（验证效果）
```

遇到 Bug 时：
```
发现 Bug → /debug（诊断）→ AI 追溯根因
  → 修复 → /verify（验证修复）
```

---

## 六、常见问题与最佳实践

### 6.1 第三方 API 注意事项

- `/model` 命令不可用——模型由 API 提供商决定
- `/effort` 的部分级别可能不可用
- `/fast` 模式不可用
- Routines、Remote Control、Teleport、Workflows 等需 Claude 订阅的功能不可用
- Desktop/Web/Slack/CI 集成不可用
- 其他核心功能（编辑、调试、MCP、Hooks、Skills）正常工作

### 6.2 最佳实践

1. **明确具体**：不要说"修复 Bug"，说"修复用户登录后空白页的 Bug"
2. **分步描述**：复杂任务分解为步骤
3. **让 AI 先探索**：大改动前让 AI 先理解代码库
4. **善用 Skills**：`/plan` 先出方案再执行
5. **定期压缩**：长对话用 `/compact` 释放上下文
6. **使用记忆**：写 CLAUDE.md 和自动记忆减少重复说明
7. **权限模式**：信任的任务用 `acceptEdits` 模式提高效率

### 6.3 常用保存路径

| 文件 | 作用范围 | 用途 |
|------|----------|------|
| `~/.claude/CLAUDE.md` | 全局 | 个人偏好 |
| `./CLAUDE.md` | 项目 | 项目规范 |
| `~/.claude/settings.json` | 全局 | 全局设置 |
| `.claude/settings.json` | 项目 | 项目设置 |
| `~/.claude/skills/` | 全局 | 全局 Skills |
| `.claude/skills/` | 项目 | 项目 Skills |
| `.claude/hooks/` | 项目 | 项目 Hooks |

---

## 七、快速参考手册

### 7.1 最常用命令速查

```bash
claude            # 启动
claude -p "查询"  # 一次查询
claude -c         # 继续上次

# 会话内
/init             # 初始化项目
/clear            # 清空上下文
/compact          # 压缩对话
/plan 描述        # 计划模式
/code-review      # 代码审查
/debug            # 调试
/diff             # 查看变更
/batch 指令       # 并行批量变更
```

### 7.2 后台代理速查

```bash
claude agents           # 查看后台代理
claude attach <id>      # 附加到代理
claude stop <id>        # 停止
/bg                     # 当前会话转后台
/tasks                  # 查看后台任务
```

### 7.3 配置速查

```bash
~/.claude/settings.json        # 全局设置
.claude/settings.json          # 项目设置
./CLAUDE.md                    # 项目记忆
~/.claude/CLAUDE.md            # 全局记忆
```

---

> **文档版本**：基于 Claude Code 官方文档 [code.claude.com/docs](https://code.claude.com/docs/en/overview)
> **最后更新**：2026-06-29
> **适用后端**：本文档面向所有 Claude Code 用户。标注 `⚠️ Claude 官方模型` 的功能仅在使用官方 API 或订阅时可用，使用 DeepSeek 等第三方 API 的用户可略过。
> **使用建议**：需要查找某个功能时，用 Ctrl+F 搜索关键词。