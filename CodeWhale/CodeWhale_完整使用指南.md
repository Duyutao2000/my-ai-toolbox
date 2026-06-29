# CodeWhale 完整使用指南

> **一句话概述**：CodeWhale 是一个运行在终端里的 AI 编程助手，支持 25+ 模型提供商（DeepSeek、Anthropic、OpenAI 等），开放模型优先——你可以用自然语言让它帮你写代码、调试 Bug、管理项目，就像和一个资深程序员结对编程。

---

## 阅读说明

> 本文档基于 CodeWhale **v0.8.65** 编写，面向所有 CodeWhale 用户。CodeWhale 以 **Brother Whale** 为创始原则，内置 Constitution（宪法）治理体系，通过代码强制执行的层级化法律框架（全局宪法 → 项目法律 → 当前请求 → 实时证据），确保 AI 行为可预测、可审计、可信任。
>
> CodeWhale 默认使用 **DeepSeek V4** 系列模型（pro / flash），同时支持 Anthropic、OpenAI、GLM、Kimi 等 25+ 提供商，支持 1M token 上下文窗口，内置提示缓存（Prefix Cache）优化，支持并行工具调用、子代理系统和持久化 Fleet 编排。

---

## 目录

- [零、CodeWhale 能力地图](#零codewhale-能力地图)
- [一、欢迎页](#一欢迎页)
  - [CodeWhale 的独特之处：Constitution 治理体系](#codewhale-的独特之处constitution-治理体系)
- [二、第一次使用（手把手教学）](#二第一次使用手把手教学)
- [三、核心命令详解](#三核心命令详解)
  - [3.1 项目与上下文管理模块](#31-项目与上下文管理模块)
  - [3.2 任务与进度管理模块](#32-任务与进度管理模块)
  - [3.3 Sub-agents 模块（子代理系统）](#33-sub-agents-模块子代理系统)
  - [3.4 Fleet 模块（持久化编排系统）](#34-fleet-模块持久化编排系统)
  - [3.5 RLM 模块（递归语言模型）](#35-rlm-模块递归语言模型)
  - [3.6 Skill 系统（技能模块）](#36-skill-系统技能模块)
  - [3.7 Memory 与项目指令模块](#37-memory-与项目指令模块)
  - [3.8 权限与安全模块](#38-权限与安全模块)
  - [3.9 模型与配置模块](#39-模型与配置模块)
  - [3.10 状态与成本控制模块](#310-状态与成本控制模块)
  - [3.11 Git 与版本控制模块](#311-git-与版本控制模块)
  - [3.12 MCP 服务模块（模型上下文协议）](#312-mcp-服务模块模型上下文协议)
  - [3.13 Hooks 模块（钩子系统）](#313-hooks-模块钩子系统)
  - [3.14 Plugins 模块（插件系统）](#314-plugins-模块插件系统)
- [四、完整工作流示例](#四完整工作流示例)
- [五、常见问题与解决方案](#五常见问题与解决方案)
- [六、最佳实践建议](#六最佳实践建议)
- [七、进阶功能](#七进阶功能)
- [八、快速参考手册](#八快速参考手册)

---

## 零、CodeWhale 能力地图

在深入学习之前，先通过这张"能力地图"了解 CodeWhale 所有功能模块的全貌：

```
┌──────────────────────────────────────────────────────────────────────┐
│                        CodeWhale 能力地图                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │  项目管理     │  │  任务系统     │  │  Sub-agents  │                │
│  │  /goal       │  │  /task       │  │  子代理系统   │                │
│  │  update_plan │  │  checklist   │  │  并行协作     │                │
│  │  /compact    │  │  Fleet 编排  │  │  worktree 隔离│                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │  Fleet 系统  │  │  Skill 技能  │  │  项目法律     │                │
│  │  持久化编排   │  │  专业技能包   │  │  constitution│                │
│  │  多 worker   │  │  一键调用     │  │  AGENTS.md   │                │
│  │  跨会话恢复   │  │  社区市场     │  │  项目指令     │                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │  MCP 服务    │  │  Hooks 钩子  │  │  Plugins 插件│                │
│  │  外部工具连接 │  │  事件触发     │  │  扩展生态     │                │
│  │  数据库操作   │  │  自动化脚本   │  │  功能打包     │                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                │
│  │  权限安全     │  │  模型配置     │  │  Git 集成    │                │
│  │  多级权限模式 │  │  DeepSeek V4 │  │  git_status  │                │
│  │  审批门控     │  │  Prefix Cache│  │  git_diff    │                │
│  │  白名单机制   │  │  并行调用     │  │  git_log     │                │
│  └──────────────┘  └──────────────┘  └──────────────┘                │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────┐        │
│  │              Constitution 治理体系（v0.8.65）              │        │
│  │  4 层嵌套：全局宪法 → 项目法律 → 当前请求 → 实时证据       │        │
│  │  核心原则：用户意图至上 · 仓库法律显式 · 证据高于叙述 · 记忆在后 │        │
│  └──────────────────────────────────────────────────────────┘        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

> **阅读建议**：如果你是第一次使用，建议按顺序从第一章读到第三章。如果你已经有一定基础，可以直接跳到第八章"快速参考手册"查找你需要的命令。

---

## 一、欢迎页

### CodeWhale 能帮你做什么？

CodeWhale 是一个**终端 AI 编程助手**，基于 DeepSeek V4 架构。把它想象成一个坐在你旁边的资深程序员——你只需要用中文（或英文）告诉它你想做什么，它就会帮你完成。

**典型场景**：
- "帮我创建一个 React 登录页面" → 它自动生成完整代码
- "这段代码为什么报错？" → 它分析错误并给出修复方案
- "把项目中所有 `var` 改成 `let`" → 它批量修改所有文件
- "帮我写这个函数的单元测试" → 它生成测试代码
- "分析这个 Python 项目的依赖关系" → 它搜索、读取、生成分析报告

### CodeWhale 的独特之处：Constitution 治理体系

CodeWhale 的核心特色是内置了一套 **Constitution（宪法）** 治理体系。它不是简单的提示词工程，而是一个**代码强制执行的层级化法律框架**——法律住在运行框架里，而非模型提示词中，因此换模型法律不变。

```
┌─────────────────────────────────────────────────────────────┐
│                  Constitution 法律层级（v0.8.65）              │
│                                                             │
│  1. 全局宪法（Global Constitution）                          │
│     编译进每个二进制的基础法律，不可被任何下层覆盖               │
│     权威次序条款固定了冲突时的裁决规则                         │
│                                                             │
│  2. 项目法律（Project Law / Local Law）                     │
│     .codewhale/constitution.json 声明持久权威：              │
│     受保护不变量 · 分支策略 · 验证策略 · 升级条件              │
│     （也读取 AGENTS.md / .codewhale/instructions.md 等）     │
│                                                             │
│  3. 当前请求（Current Request）                             │
│     你当前这轮的指令——用户意图至上                           │
│     高于过期的仓库指引、记忆和先前的交接                       │
│                                                             │
│  4. 实时证据（Live Evidence）                                │
│     工具实际返回的内容——证据高于叙述                         │
│     测试失败就如实报告失败，绝不被总结成乐观措辞               │
│                                                             │
│  ─── 法律不生活在模型中，模型可以随时更换 ───                │
│  运行时框架承载宪法，模型提供推理。                            │
│  换掉模型，法律不变。                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**这对你意味着什么**：
- **用户意图至上**：你当前的请求高于过期的仓库指引、记忆和交接文档
- **仓库法律必须显式**：通过 `.codewhale/constitution.json` 声明项目的持久权威
- **证据高于叙述**：工具输出胜过自信的猜测，验证是任务的一部分
- **记忆排在最后**：有用但永不具备权威
- **换模型法律不变**：法律在运行框架中，不在模型提示词里

### 核心概念速览

| 概念 | 通俗解释 |
|------|----------|
| **会话（Session）** | 你和 AI 的一次"对话"。每次启动 CodeWhale，就是一个新会话。 |
| **上下文（Context）** | AI 能"记住"的对话内容。CodeWhale 支持 1M token 上下文窗口。 |
| **Token** | AI 处理文本的计量单位。中文约 1 个汉字 = 1~2 个 token。 |
| **Constitution** | CodeWhale 内置的宪法治理体系，代码强制执行而非提示词。 |
| **Prefix Cache** | DeepSeek V4 的提示缓存机制，缓存命中时成本降低约 90%。 |
| **Sub-agent（子代理）** | AI 可以"派遣"多个子 AI 同时处理不同任务，最多 20 个并发。 |
| **Fleet（舰队）** | 持久化多 worker 控制面，跨会话/跨进程管理批量任务。 |
| **Route（路由）** | 模型 + provider 的解析结果（含端点、协议、上下文限制、价格）。 |
| **Checklist（清单）** | AI 用清单跟踪多步骤任务的进度，确保不遗漏任何环节。 |

---

## 二、第一次使用（手把手教学）

### 2.1 启动 CodeWhale

#### 第一步：打开终端

- **Windows**：按 `Win + R`，输入 `powershell`，回车
- **macOS**：按 `Cmd + 空格`，输入 `Terminal`，回车
- **Linux**：按 `Ctrl + Alt + T`

#### 第二步：进入你的项目文件夹

```bash
cd /你的项目路径/
```

例如：
```bash
cd ~/my-website/
```

#### 第三步：配置 API Key 并启动

首次使用需要为默认的 DeepSeek provider 配置 API Key：

```bash
codewhale auth set --provider deepseek
codewhale auth status
codewhale doctor
```

或者通过环境变量配置：

```bash
export DEEPSEEK_API_KEY="your-key"
```

然后启动 CodeWhale：

```bash
codewhale
```

#### 更多安装方式

除了 npm，CodeWhale 还支持以下安装方式：

```bash
# Docker
docker pull ghcr.io/hmbown/codewhale:latest
docker run --rm -it -v "$PWD:/workspace" ghcr.io/hmbown/codewhale:latest

# Nix
nix run github:Hmbown/CodeWhale

# Windows Scoop
scoop install codewhale

# CNB 镜像（中国大陆用户推荐）
cargo install --git https://cnb.cool/codewhale.net/codewhale --tag v0.8.65 codewhale-cli --locked --force

# 从源码构建（Rust 1.88+）
cargo install codewhale-cli --locked
cargo install codewhale-tui --locked
```

> **注意**：npm wrapper（Node 18+）会从 GitHub Releases 下载经 SHA-256 校验的二进制文件，安装 `codewhale`、`codew` 和 `codewhale-tui` 三个命令。Cargo 构建 Linux 用户需先安装：`sudo apt-get install -y build-essential pkg-config libdbus-1-dev`。

#### 启动后会看到什么界面？

启动后，你会看到类似这样的界面：

```
┌─────────────────────────────────────────────────────────┐
│  CodeWhale  v0.8.53                                      │
│                                                         │
│  Model: deepseek-v4-pro                                  │
│  Platform: windows                                       │
│                                                         │
│  > _                                                     │
│                                                         │
│  Press Enter to send, /help for commands                 │
└─────────────────────────────────────────────────────────┘
```

顶部显示版本号、当前使用的模型和平台信息，底部的 `>` 是你输入消息的地方。

#### 如何退出？

- 输入 `/exit` 或 `/quit` 然后回车
- 或者按 `Ctrl + C`（连按两次）

#### 快捷键介绍

| 快捷键 | 功能 | 说明 |
|--------|------|------|
| `Ctrl + C` | 中断当前操作 | AI 正在执行任务时，按一次暂停，连按两次退出 |
| `Ctrl + L` | 压缩上下文 | 清理对话历史，保留关键信息（等同于 `/compact`） |
| `Ctrl + R` | 搜索历史命令 | 在之前的输入中搜索 |
| `Ctrl + S` | 立即发送 | 当有排队消息时强制发送 |
| `Tab` | 模式切换 / 自动补全 | 编辑器空闲时切换 Plan/Agent/YOLO 模式；编辑时补全路径 |
| `↑ / ↓` | 浏览历史输入 | 上下翻看之前发过的消息 |
| `Enter` | 发送消息 | 提交当前输入 |

### 2.2 启动后第一件事

CodeWhale 启动后会自动读取项目中的指令文件，包括：
- `AGENTS.md` — 项目级 AI 行为指南
- `CLAUDE.md` — 兼容 Claude Code 的项目配置
- `.codewhale/instructions.md` — CodeWhale 专用项目指令
- `.deepseek/instructions.md` — DeepSeek 项目指令

这些文件告诉 AI 你的项目是什么样的、用什么技术、有什么规矩。如果项目中没有这些文件，你可以直接告诉 AI 你的项目信息，或者让它帮你创建一个。

#### 快速创建项目指令文件

在你的项目中新建 `.codewhale/instructions.md`：

```markdown
# 项目概述
这是一个 React 个人网站项目，使用 Vite 构建。

## 技术栈
- 前端框架: React 18
- 构建工具: Vite
- CSS 方案: Tailwind CSS

## 代码规范
- 使用函数组件 + Hooks
- 组件文件使用 PascalCase 命名
- CSS 优先使用 Tailwind 类名

## 常用命令
- npm run dev: 启动开发服务器
- npm run build: 构建生产版本
```

之后每次启动 CodeWhale，AI 都会先自动读取这些指令。

---

## 三、核心命令详解

### 3.1 项目与上下文管理模块

#### `/compact` — 压缩对话历史

- **作用**：将长对话压缩成摘要，释放上下文空间
- **为什么需要**：虽然 CodeWhale 支持 1M token 上下文，但对话太长时响应变慢、成本升高。压缩就像把长篇对话整理成会议纪要。
- **什么时候用**：
  - 上下文使用超过 60% 时（系统会提示）
  - 完成了一个大任务，准备开始新任务
  - 对话超过 30 轮
- **怎么用**：
  - `/compact` — AI 自动判断哪些内容需要保留
  - `Ctrl + L` — 快捷键触发压缩

#### `/goal` — 持久化目标循环

- **作用**：设定一个持久目标，AI 会在多轮中持续工作——读取、编辑、运行、检查——直到目标完成或受阻
- **什么时候用**：大型任务（"重构整个 auth 模块"）需要多轮才能完成
- **怎么用**：`/goal 将 user 模块从 JS 迁移到 TS`

#### `/mode` — 切换工作模式

- **作用**：在 Plan（只读审查）、Agent（审批执行）、YOLO（自动执行）三种模式间切换
- **什么时候用**：
  - **Plan**：不熟悉项目时先探索，不做修改
  - **Agent**：日常开发（默认）
  - **YOLO**：信任的沙箱项目，不需要每次审批
- **怎么用**：
  - `/mode` — 打开模式选择器
  - `/mode plan` / `/mode agent` / `/mode yolo` — 直接切换

#### `/restore` — 回滚操作

- **作用**：从 side-git 快照回滚之前某一轮的变更——存放在仓库 `.git` 之外，不触动你的提交历史
- **什么时候用**：AI 改错了文件，想撤销当前会话中的某次变更
- **怎么用**：`/restore`

#### `/review` — 结构化工单审查

- **作用**：触发一个结构化的审查工作流，AI 会按照审查清单逐项检查
- **什么时候用**：完成一个功能后想要系统性的代码审查
- **怎么用**：`/review`

#### `/provider` 和 `/model` — 路由和模型管理

- **作用**：`/provider` 展示所有 provider 的认证状态、默认路由和费用信息；`/model` 选择模型和推理深度
- **什么时候用**：需要切换提供商或调整模型时
- **怎么用**：
  - `/provider` — 查看 provider 就绪面板
  - `/provider deepseek` — 切换到 DeepSeek
  - `/model` — 选择模型
  - `/model auto` — 让 AI 自动选择
  - `/model pro` / `/model flash` — 快速选择 DeepSeek 档位

#### `/skills` — 技能管理系统

- **作用**：列出和加载已安装的 Skill 技能包（等价于 `/skill` 但带有管理界面）
- **怎么用**：`/skills`

#### `/statusline` — 自定义状态栏

- **作用**：选择底栏显示哪些状态信息（模式、模型、费用、缓存状态、Git 分支等）
- **怎么用**：`/statusline`

#### `/modeldb` — 模型参考浏览器

- **作用**：打开内置模型目录的分页器——查看每个模型的上下文窗口、最大输出、模态和价格
- **怎么用**：`/modeldb`

#### `/memory` — 记忆管理

- **作用**：查看或管理持久化记忆（当记忆功能启用时）
- **怎么用**：`/memory`

#### `/config` — 运行时设置编辑

- **作用**：编辑运行时设置，包括 presets 等
- **怎么用**：
  - `/config` — 打开配置编辑器
  - `/config preset calm` — 应用"平静"预设
  - `/config preset calm --save` — 保存为默认
  - `/config subagents status` — 查看子代理配置

#### `checklist_write` — 创建任务清单

- **作用**：AI 将复杂任务拆解为步骤清单，逐项跟踪进度
- **什么时候用**：你给了 AI 一个多步骤的复杂任务时，AI 会自动创建
- **你会看到什么**：AI 会先列出所有步骤，标注状态（pending / in_progress / completed），然后逐步执行
- **好处**：
  - 你一眼就能看到 AI 在做什么、做到哪了
  - AI 不会遗漏步骤
  - 如果中途改变方向，可以要求 AI 更新清单

#### `update_plan` — 更新高阶策略

- **作用**：为复杂项目提供阶段级的策略视图（和 checklist 互补：checklist 管步骤，plan 管大方向）
- **什么时候用**：多阶段大型任务（通常 5+ 步骤、3+ 阶段）
- **和 checklist 的区别**：
  - Checklist：具体可执行的叶子任务
  - Plan：高层次的阶段和方向

#### 上下文管理最佳实践

1. **一个会话专注一个主题**：不要把前端、后端、数据库全部塞进一个会话
2. **大任务用 Sub-agents**：并行处理更快、更省 token
3. **及时 `/compact`**：当上下文压力提示出现时，不要等到卡顿才压缩
4. **善用 Prefix Cache**：DeepSeek V4 缓存共享前缀，同一个会话中追加新内容比修改旧内容更省钱

---

### 3.2 任务与进度管理模块

CodeWhale 提供了**持久化任务系统（Task）**，支持创建、跟踪、恢复长时间运行的工作。

#### `task_create` — 创建后台任务

- **作用**：创建一个持久化的后台任务，即使会话中断也能恢复
- **什么时候用**：
  - 需要跨会话的长期工作（如"重构整个项目的类型系统"）
  - 需要安排多个独立任务并行推进
- **怎么用**：告诉 AI "创建一个任务：重构 user 模块的类型定义"，AI 会调用 `task_create`

#### `task_list` — 查看任务列表

- **作用**：列出所有持久化任务及其状态
- **怎么用**：输入 `/tasks` 或让 AI 调用 `task_list`

#### `task_read` — 查看任务详情

- **作用**：查看某个任务的完整时间线、清单进度、验证结果
- **怎么用**：告诉 AI "查看任务 xxx 的进度"

#### 任务系统的实际应用

> 你："帮我创建一个任务：把整个项目从 JavaScript 迁移到 TypeScript"

AI 的处理流程：
1. 创建持久化任务，写入任务描述
2. 用 `checklist_write` 拆分为：分析现有文件 → 配置 tsconfig → 逐个文件迁移 → 类型检查 → 修复错误
3. 即使你关闭终端、明天再回来，任务仍然存在
4. 通过 `task_read` 可以查看当前的进度和已完成的工作

---

### 3.3 Sub-agents 模块（子代理系统）

#### 什么是 Sub-agents？

**Sub-agents（子代理）** 是 CodeWhale 的一种核心工作模式——主 AI 可以"派遣"多个子 AI 同时处理不同的子任务。把它想象成**项目经理给多个开发人员分配任务**。

#### 子代理的类型

| 角色 | 职能 | 是否可写 | 典型用途 |
|-------|-------|----------|----------|
| **general** | 灵活，按父代理指令执行 | 是 | 多步骤通用任务（默认） |
| **explore** | 只读代码映射和搜索 | 否 | "查找所有 `Foo` 的调用点" |
| **plan** | 分析和生成策略方案 | 最小化 | "设计迁移方案，不要执行" |
| **review** | 阅读和评分，带严重等级 | 否 | "审查这个 PR 的 Bug" |
| **implementer** | 精确落地指定的变更 | 是 | "把 `bar.rs::Foo::bar` 重写为 X" |
| **verifier** | 运行测试/验证，报告结果 | 否 | "运行 `cargo test`，报告结果" |
| **custom** | 显式窄工具白名单 | 视配置而定 | 锁定分发的受限执行 |

#### 什么时候使用子代理？

- **并行任务**：同时修改前端和后端代码
- **独立研究**：让一个子代理研究某个技术方案，另一个修改代码
- **大规模重构**：让多个子代理分别处理不同的文件
- **代码审查**：主代理写代码，子代理审查代码质量

#### 子代理的实际应用

> 你："帮我同时做三件事：给 Login 组件写测试、给 API 模块加错误处理、审查 Dashboard 页面的性能问题"

AI 的处理流程：
1. 分析三个任务相互独立
2. 创建 **3 个子代理**，分别处理测试、错误处理、性能审查
3. 三个子代理**并行**工作
4. 各子代理完成后，AI 汇总结果报告给你

#### Fork Context（上下文分叉）

子代理支持 `fork_context` 模式：当多个子代理需要相同的项目上下文时，它们共享父代理的上下文前缀，利用 DeepSeek V4 的 Prefix Cache 大幅降低成本。

#### Worktree 隔离

子代理支持 `worktree: true` 参数，CodeWhale 会创建一个独立的 Git worktree 和分支供该子代理使用：

- 默认分支名：`codex/agent-<name>-<id>`
- 默认路径：`<项目根>/.codewhale-worktrees/`
- 父仓库保持干净，子代理的修改被隔离在独立空间中

#### 子代理的并发上限

默认最多 **20** 个并发子代理（可通过 `[subagents].max_concurrent` 在 `~/.codewhale/config.toml` 中配置）。会话可准入最多 **200** 个排队中的子代理。

可通过 provider 级别配置微调并发策略：

```toml
[subagents]
# 全局默认
max_concurrent = 20
launch_concurrency = 20
max_admitted = 200
token_budget = 100000

[subagents.providers.deepseek]
max_concurrent = 20
launch_concurrency = 20

[subagents.providers.glm]
max_concurrent = 4
launch_concurrency = 3
max_admitted = 12
api_timeout_secs = 180
```

通过 `/config subagents status` 查看当前全局值和活跃 provider 的解析配置。

#### Token 预算

设置 `[subagents].token_budget` 为每个根 `agent` 分配一个聚合 Token 上限（由该子代理及其所有后代共享）。超出后子代理以 `budget_exhausted` 状态停止，而非持续运行。

---

### 3.4 Fleet 模块（持久化编排系统）

#### 什么是 Fleet？

**Fleet（舰队）** 是 CodeWhale 的持久化控制平面，用于管理和监控多 worker 批量运行。一个 Fleet worker 是一个无头的 `codewhale exec` 进程，但 Fleet 会持久化追踪它——任务记录在追加式账本（`.codewhale/fleet.jsonl`）中，即使管理器退出、系统休眠或运行时重启，任务也不会丢失。

**简单理解**：Sub-agents 是会话内的临时帮手，Fleet 是跨会话的持久化工人。

#### Fleet 的核心概念

| 概念 | 说明 |
|------|------|
| **Worker** | 一个无头执行单元（`codewhale exec` 运行） |
| **Run** | 一次 Fleet 任务的完整生命周期 |
| **Role** | worker 的行为模板（strong / balanced / fast） |
| **Profile** | 角色 + 模型的组合配置 |
| **Loadout** | 模型意图类别（`strong` / `balanced` / `fast`） |

#### Fleet 的基本用法

```bash
# 从任务 JSON 文件启动 Fleet 运行
codewhale fleet run tasks.json --max-workers 4

# 查看运行状态
codewhale fleet status

# 恢复中断的运行（幂等，可安全重复执行）
codewhale fleet resume <run-id>
```

#### Fleet 的实际应用

> 你："把这个大项目拆成 8 个独立的任务，让 Fleet 并行执行"

AI 的处理流程：
1. 分析项目中可以独立执行的任务（如测试各个模块）
2. 生成 `tasks.json`
3. 调用 `codewhale fleet run tasks.json --max-workers 4`
4. 4 个 worker 并行工作，每个 worker 输出结构化回执（pass / fail / partial / skip / timeout）
5. 实时查看 `fleet status`
6. 完成后汇总所有 worker 的结果

---

### 3.5 RLM 模块（递归语言模型）

> **当前状态**：RLM 仍可用，但在 v0.8.65 中已非核心推荐路径。大多数大数据处理任务可通过 Sub-agents 和 Fork Context 更高效地完成。本节仅供参考。

#### 什么是 RLM？

**RLM（Recursive Language Model，递归语言模型）** 是为超出普通上下文窗口的内容（大文件、长文档、批量数据）提供的 **Python REPL 分析环境**。

**简单理解**：RLM 就像一个"外接分析引擎"。当任务太大不适合直接在聊天中处理时，AI 会把数据加载到 RLM，在那里用 Python 代码和子 LLM 调用完成分析，只把结论返回给你。

#### RLM 的四大工作模式

| 模式 | 适用场景 | 核心工具 |
|------|----------|----------|
| **CHUNK（分块）** | 单个超大文件（>50K token） | `chunk()` 切分，逐块处理 |
| **BATCH（批量）** | 大量独立条目需要 LLM 处理 | `sub_query_batch()` 并行执行 |
| **SEQUENCE（顺序）** | 数据依赖链，A → B → C | `sub_query_sequence()` 或 Python for 循环 |
| **RECURSE（递归）** | 需要分解 + 批评的复杂问题 | `sub_query()` / `sub_rlm()` |

#### 什么情况下会用到 RLM？

- 分析一个 2 万行的日志文件，找出所有异常
- 对 100 个数据集条目进行分类
- 从一份 500 页的 PDF 中提取结构化数据
- 让子 LLM 审查你的推理，找出盲点

#### RLM 使用的核心原则

1. **确定性问题用 Python**：计数、正则、解析——在 REPL 中直接算，不浪费 LLM 调用
2. **语义问题用 sub_query**：分类、打分、情感分析——交给子 LLM
3. **独立任务走 Batch**：`dependency_mode="independent"`，一次完成多个
4. **依赖任务走 Sequence**：不要用 Batch 跑有先后顺序的任务

---

### 3.6 Skill 系统（技能模块）

#### 什么是 Skill？

**Skill** 是一套预定义的"专业技能包"——它包含了特定任务的知识、工作流程和指令。当 AI 加载一个 Skill 时，相当于临时请了一位该领域的专家。

**Skill 和普通提示词的区别**：
- 普通提示词：你每次都要手动告诉 AI 怎么做
- Skill：把一套经过验证的工作流程打包，一键调用，AI 自动按照最佳实践执行

#### 内置 Skill 介绍

CodeWhale 内置了以下常用 Skill：

| Skill | 功能 | 典型用法 |
|-------|------|----------|
| **documents** | Word 文档创建与编辑 | "帮我生成一份项目周报" |
| **spreadsheets** | Excel/CSV 表格处理 | "分析这个销售数据表" |
| **pdf** | PDF 读写与提取 | "从这份 PDF 中提取所有表格" |
| **presentations** | PPT 创建与编辑 | "根据这份报告生成演示文稿" |
| **skill-creator** | 创建自定义 Skill | "帮我创建一个代码格式化 Skill" |
| **skill-installer** | 安装社区 Skill | "安装 GitHub 上的某个 Skill" |
| **mcp-builder** | MCP 服务器开发 | "帮我搭建一个天气查询 MCP" |
| **plugin-creator** | 插件脚手架 | "创建一个自动部署插件" |
| **feishu** | 飞书/Lark 集成 | "把结果发到飞书文档" |
| **delegate** | 战略委托 | "帮我分派复杂任务给子代理" |
| **ods-dql-generator** | Oracle ODS 取数 DQL 生成 | "根据 DDL 生成 Kettle 取数脚本" |
| **v4-best-practices** | V4 最佳实践 | 多步骤任务时的参考指南 |

#### 如何使用 Skill

**方式一：主动调用**
```
/skill documents
```

**方式二：自动触发**
某些 Skill 会根据你的描述自动激活。比如你说"帮我创建一份 PPT"，AI 可能自动加载 `presentations` Skill。

**方式三：自然语言触发**
```
"帮我用 ods-dql-generator 生成取数脚本"
```

#### Skill 的存放位置

| 位置 | 作用范围 |
|------|----------|
| `C:\Users\<用户名>\.codewhale\skills\` | 全局——所有项目可用 |
| `C:\Users\<用户名>\.claude\skills\` | 兼容 Claude Code 全局 Skill |
| 项目 `.codewhale/skills/` | 项目——仅当前项目可用 |

#### 如何创建自己的 Skill

参见第七章"进阶功能"中的 Skill 创建指南。

---

### 3.7 Memory 与项目指令模块

#### CodeWhale 如何"记住"信息？

CodeWhale 使用**项目指令文件**作为项目级法律（Project Law——Constitution 层级中的第 2 层），并通过 `/memory` 管理持久化记忆。

#### 项目法律文件

CodeWhale 在启动时自动加载以下项目法律文件（按优先级从高到低）：

| 文件 | 作用 | 是否可提交 Git |
|------|------|---------------|
| `.codewhale/constitution.json` | 项目宪法——声明受保护不变量、分支策略、验证策略 | 是 |
| `.codewhale/instructions.md` | CodeWhale 专用项目指令 | 是 |
| `AGENTS.md` | 项目级 AI 行为指南 | 是 |
| `CLAUDE.md` | 兼容 Claude Code 格式的项目配置 | 是 |
| `.deepseek/instructions.md` | DeepSeek 项目指令 | 是 |

> **重要**：项目法律文件位于 Constitution 第 2 层，优先级高于记忆（第 4 层）和你的当前请求（第 3 层）。即项目法律对 AI 有强制约束力。如需声明项目的持久权威规则，使用 `.codewhale/constitution.json`：

```json
{
  "protected_invariants": ["不应修改 data/ 目录下的原始数据"],
  "branch_policy": "不允许直接推送到 main 分支",
  "verification_policy": "所有修改必须通过 `pytest` 验证",
  "escalate_when": ["检测到 API 密钥泄露"]
}
```

#### EngineConfig.instructions

如果你的 CodeWhale 配置中设置了 `EngineConfig.instructions`，指向的文件也会作为 Local Law 加载。这类指令由系统嵌入者声明，具有和项目指令相同甚至更高的效力。

#### `note` 工具 — 会话记忆

AI 可以在会话中使用 `note` 保存重要信息，这些信息在 `/compact` 压缩后会保留。

#### 记忆的实际应用场景

**场景 1：在项目指令中定义代码规范**
> 在 `.codewhale/instructions.md` 中写入：
> ```markdown
> ## 代码规范
> - 所有注释使用中文
> - 函数必须有类型注解
> - 使用 f-string 而非 .format()
> ```
> AI 之后生成的代码会自动遵守这些规范。

**场景 2：让 AI 记住重要决策**
> 你："记住，我们的 API 基础 URL 是 `https://api.example.com/v2`"
> AI：会将这个信息保存为 note。

**场景 3：项目关键约定**
> 在 `AGENTS.md` 中写入：
> ```markdown
> ## 关键约定
> - 不要修改 chapter1 和 chapter2 目录下的任何文件
> - 数据库迁移必须先经过 review
> ```
> AI 会严格遵守这些约定。

---

### 3.8 权限与安全模块

#### 权限模式

CodeWhale 有不同的权限模式来平衡**安全性**和**便利性**：

| 权限模式 | 说明 | 适用场景 |
|----------|------|----------|
| **Plan（只读）** | 不允许任何修改，只读调查 | 审查不熟悉的代码库 |
| **Agent（建议审批）** | 工具使用需审批门控 | 日常开发推荐 |
| **YOLO（自动执行）** | 自动批准所有操作 | 沙箱/信任环境 |

#### CodeWhale 的审批机制

在 Agent 模式下，当你要求 AI 执行以下操作时，需要你审批：
- 文件写入/编辑
- Shell 命令执行
- Sub-agent 创建
- 批量操作

审批时你会看到 AI 的详细计划（通常伴随 checklist），一目了然。

#### 操作系统级沙箱

CodeWhale 不仅依赖审批门，还提供了操作系统级的沙箱隔离：

| 平台 | 沙箱机制 | 说明 |
|------|----------|------|
| macOS | **Seatbelt** | 基于 macOS 沙箱扩展（Sandbox Profile）限制进程能力 |
| Linux | **Landlock + seccomp** | Landlock 限制文件系统访问，seccomp 限制系统调用 |
| Linux | **Bubblewrap (bwrap)** | 可选，提供用户命名空间隔离 |
| Windows | 审批门控 + 白名单 | Windows 上主要通过审批门和路径白名单实现 |

#### Side-git 快照与回滚

CodeWhale 会在运行过程中自动创建 **side-git 快照**：
- 快照存放在仓库 `.git` **之外的**独立存储中
- 通过 `/restore` 可以撤销之前某一轮的变更
- 不触动你的真实 Git 提交历史——回滚和恢复完全独立

#### Hooks v2

从 v0.8.58 开始，CodeWhale 提供了升级版钩子系统（Hooks v2）。配置文件为 `.codewhale/hooks.toml`：

```toml
[tool_call_before]
# 在工具执行前触发，返回 allow / deny / ask 决策
"edit_file" = "ask"
"shell" = { policy = "deny", glob = "rm -rf *" }
```

钩子决策有三个返回值：**allow**（允许）、**deny**（拒绝）、**ask**（请求审批）。deny 优先于 allow。

#### 安全最佳实践

1. **不要在对话中粘贴敏感信息**（密码、API 密钥、数据库连接字符串）
2. **环境变量存放密钥**：用 `.env` 文件或环境变量，或使用 `codewhale auth set`
3. **审查敏感操作**：删除文件、执行危险命令时仔细看 AI 的说明
4. **使用 `.gitignore`**：确保 `.env` 和敏感配置不会提交到 Git
5. **项目指令中写明安全边界**：哪些目录不可动、哪些命令不可执行

---

### 3.9 模型与配置模块

#### CodeWhale 使用的模型

CodeWhale 支持 **25+ 模型提供商**，共用同一套运行框架和同一组工具。DeepSeek 是默认提供商，但你可以随时切换：

**开放模型，托管服务：**
`deepseek`（默认）、`openrouter`、`huggingface`（Inference Providers）、`moonshot`（Kimi）、`zai`（GLM）、`minimax`、`volcengine`（火山方舟）、`nvidia-nim`、`together`、`fireworks`、`novita`、`siliconflow` / `siliconflow-CN`、`arcee`、`xiaomi-mimo`、`openmodel`、`deepinfra`、`stepfun`、`atlascloud`、`qianfan`、`wanjie-ark`，外加通用 `openai` 兼容路由

**开放模型，自托管：**
`vllm`、`sglang`、`ollama`——直连 localhost，无需 API Key

**闭源提供商，原生直连：**
`anthropic`（原生 Messages 适配器，支持自适应思考、prompt-cache 断点）、`openai-codex`（实验性，复用 ChatGPT/Codex CLI 登录）

#### RouteResolver（路由解析）

CodeWhale 不只是替换 API Base URL，而是通过 **RouteResolver** 解析出完整的"真实路由"：

```
提供商 + 模型 → RouteResolver → {
  端点（endpoint）,
  协议（wire protocol: Chat Completions / Messages API / Responses API）,
  模型 ID,
  上下文窗口（context limit）,
  价格（per-token / subscription / local）,
  推理能力（reasoning readiness）
}
```

路由解析的好处：
- **上下文预算感知**：压缩阈值和使用窗口来自真实路由的上下文限制，而非硬编码猜测
- **诚实费用显示**：每条路由报告确切费用状态，CodeWhale 从不编造价格
- **显式协议**：协议由路由携带，而非从提示词推断
- **推理力度翻译**：provider 间自动翻译 reasoning 力度方言

#### 提示缓存（Prefix Cache）

DeepSeek V4 的一个重要特性是 **Prefix Cache**：
- 共享前缀以 128 token 粒度缓存
- 缓存命中时成本降低约 **90%**
- 追加新内容比修改旧内容更省钱

#### 提示缓存（Prefix Cache）

DeepSeek V4 的一个重要特性是 **Prefix Cache**：
- 共享前缀以 128 token 粒度缓存
- 缓存命中时成本降低约 **90%**
- 追加新内容比修改旧内容更省钱

**实用建议**：
- 在同一个会话中继续工作比新开会话更省 token
- 不要频繁删除和重写早期消息
- 子代理使用 `fork_context: true` 可以共享父代理的缓存

#### 配置文件

CodeWhale 的配置文件位于 `~/.codewhale/config.toml`（旧的 `~/.deepseek/config.toml` 仍被读取以保持兼容）。可配置项包括：

```toml
[subagents]
max_concurrent = 20  # 最大并发子代理数（硬上限 20）
launch_concurrency = 20
token_budget = 100000

[subagents.models]
worker_model = "deepseek-v4-pro"
explorer_model = "deepseek-v4-flash"

[search]
provider = "duckduckgo"  # 搜索引擎后端

[fleet]         # Fleet 配置
max_workers = 4

[tui]
status_items = ["mode", "model", "cost", "cache", "git_branch"]
```

#### `/config` 和 `codewhale doctor`

- `codewhale doctor` — 诊断安装和配置问题
- `codewhale auth status` — 查看认证状态
- `codewhale --version` — 查看版本信息

---

### 3.10 状态与成本控制模块

#### 上下文使用监控

CodeWhale 会在界面中显示上下文使用情况。当使用率接近 60% 时会提示，建议使用 `/compact` 或 `Ctrl+L` 压缩。

#### 成本优化建议

1. **及时 `/compact`**：对话太长时及时压缩，避免 Token 浪费
2. **短会话**：完成一个主题后压缩或新开会话
3. **善用 Sub-agents**：子代理使用 flash 模型，比 pro 便宜很多
4. **Prefix Cache 友好**：追加而非修改，让缓存持续命中
5. **合理控制思考深度**：简单查询不需要深度推理

#### 思考深度（Thinking Budget）

CodeWhale 会根据任务复杂度自动调整思考深度：

| 任务类型 | 思考深度 | 原因 |
|----------|----------|------|
| 简单查找/读取 | 跳过 | 答案立即可得 |
| 工具输出解读 | 轻度 | 确认结果匹配意图 |
| 单函数代码生成 | 中度 | 需要考虑规范、边界情况 |
| 多文件重构 | 中度 | 跨文件依赖分析 |
| 调试（错误到根因） | 深度 | 需要假设-验证 |
| 架构设计 | 深度 | 需要权衡取舍 |
| 安全审查 | 深度 | 需要对抗性思维 |

---

### 3.11 Git 与版本控制模块

CodeWhale 内置了丰富的 Git 操作工具，AI 可以直接查看和控制版本。

#### 内置 Git 工具

| 工具 | 功能 | 用途 |
|------|------|------|
| `git_status` | 查看工作区状态 | 了解哪些文件被修改 |
| `git_diff` | 查看未提交的更改 | 审查 AI 或你的代码变更 |
| `git_show` | 查看某次提交详情 | 回顾历史变更 |
| `git_log` | 查看提交历史 | 了解项目演进 |
| `git_blame` | 逐行归属 | 找出某行代码是谁何时写的 |

#### 实际场景

> 你："看看我改了哪些文件？"

AI 调用 `git_status`，告诉你修改列表。

> 你："对比一下当前代码和上次提交的差异"

AI 调用 `git_diff`，展示具体的代码变更。

---

### 3.12 MCP 服务模块（模型上下文协议）

#### 什么是 MCP？

**MCP（Model Context Protocol，模型上下文协议）** 是一个标准化协议，让 CodeWhale 能够连接外部工具和数据源。CodeWhale 支持**双向 MCP 集成**：
- **MCP 客户端**：消费外部 MCP 服务器提供的工具（数据库、API、文件系统等）
- **MCP 服务器**：通过 `codewhale mcp` 将 CodeWhale 自身暴露为 MCP 服务器，供其他应用调用

**简单理解**：MCP 就是 AI 的"外接设备接口"。就像 USB 可以连接鼠标、键盘、U 盘，MCP 让 AI 能连接数据库、API、文件系统——同时 CodeWhale 也可以作为"USB 设备"被其他程序调用。

#### MCP 的典型用途

- 连接数据库（PostgreSQL、MySQL 等）
- 调用外部 API（天气、新闻、企业系统）
- 浏览器自动化（Playwright）
- GitHub/GitLab 操作
- 文件系统增强访问

#### MCP 的实际应用示例

> 你："帮我查一下数据库里有哪些用户昨天注册的？"

AI 通过 MCP 连接你的数据库，执行 SQL 查询，然后回复你结果。

---

### 3.13 Hooks 模块（钩子系统）

#### 什么是 Hooks？

**Hooks（钩子）** 是在特定事件发生时自动执行的脚本。你可以把它理解为"触发器"——当某件事发生时，自动做另一件事。

#### Hook 事件类型

CodeWhale 支持在多种事件上挂载钩子：
- **工具执行前/后**：如文件写入前检查、写入后格式化
- **会话事件**：会话开始、结束、压缩
- **用户交互事件**：消息提交、审批

#### Hook 的实际应用

- 每次 AI 写入文件后自动运行代码格式化工具
- Git 提交前自动运行单元测试
- 检测到 `.env` 文件被读取时发出警告

---

### 3.14 Plugins 模块（插件系统）

#### 什么是 Plugins？

**Plugins（插件）** 是 CodeWhale 的扩展包，可以从社区安装。一个插件可以包含多个 Skill、Hook、甚至是 MCP 服务配置。

**插件和 Skill 的区别**：
- **Skill**：轻量级，通常是一个 `SKILL.md` 文件，定义工作流程
- **Plugin**：重量级，是一个完整的包，可以包含多个 Skill + Hook + MCP + 其他资源

#### 插件管理

CodeWhale 支持通过 `plugin-creator` Skill 创建插件脚手架，通过 `skill-installer` Skill 安装社区资源。

---

## 四、完整工作流示例

### 工作流示例 1：从零开始创建一个 Python CLI 工具

**场景**：你想用 CodeWhale 创建一个命令行待办事项管理工具。

#### 第 1 步：准备环境

```bash
mkdir ~/todo-cli
cd ~/todo-cli
codewhale
```

#### 第 2 步：告诉 AI 你的需求

> 你："我要创建一个 Python CLI 待办事项管理工具。支持添加、查看、删除、标记完成。数据存为 JSON 文件。使用 click 库做命令行界面。"

AI 会先阅读项目目录（确认是空项目），然后用 `checklist_write` 列出所有步骤：

```
📋 任务清单：
[in_progress] 创建项目结构和依赖配置
[pending] 实现数据模型层
[pending] 实现添加待办功能
[pending] 实现查看待办列表功能
[pending] 实现删除和标记完成功能
[pending] 实现 CLI 入口
[pending] 写 README 文档
```

#### 第 3 步：逐步实现

AI 按清单逐步实现，每完成一步标记为 `completed`，自动进入下一步。你会看到：

```
📋 任务清单：
[completed] 创建项目结构和依赖配置
[in_progress] 实现数据模型层
[pending] 实现添加待办功能
...
```

#### 第 4 步：代码审查

> 你："检查一下代码有没有问题"

AI 调用子代理（reviewer 类型），审查代码质量、错误处理、边界情况，然后给出建议。

#### 第 5 步：验证

AI 会写一个简单的测试脚本验证功能，确认添加、查看、删除、标记都正常工作。

#### 涉及的模块总结

| 步骤 | 使用的模块 |
|------|-----------|
| 规划 | Checklist（`checklist_write`） |
| 生成代码 | 基础对话 + 工具调用 |
| 审查 | Sub-agents（`reviewer` 类型） |
| 验证 | 基础对话 + Shell 执行 |

---

### 工作流示例 2：修复现有项目的 Bug

**场景**：你的 Python 项目中数据加载模块报错 `KeyError: 'dataset_name'`，你不知道哪里出了问题。

#### 第 1 步：启动并描述问题

```bash
cd ~/my-project
codewhale
```

> 你："运行 load_data.py 时总是报 KeyError: 'dataset_name'，帮我找到原因"

#### 第 2 步：AI 系统排查

AI 的处理流程：
1. 读取 `load_data.py`，找到报错行
2. 用 `grep_files` 搜索项目中所有使用 `dataset_name` 的地方
3. 追踪数据流：`dataset_name` 从哪里来、在哪里被定义
4. 发现上游的配置文件版本不一致，新版本把 `dataset_name` 改成了 `name`

#### 第 3 步：修复

AI 定位到问题后：
- 展示错误原因和修复方案
- 征求你同意后修改代码
- 用 `edit_file` 精确替换

#### 第 4 步：验证

AI 执行修复后的脚本，确认错误已消除，然后用 `git_diff` 展示变更内容。

---

### 工作流示例 3：多文件批量重构

**场景**：你的项目 chapter4 目录下有 10+ 个 Python 文件，需要统一添加类型注解。

#### 第 1 步：描述任务

> 你："给 chapter4 目录下所有 Python 函数添加类型注解"

#### 第 2 步：AI 用 Sub-agents 并行处理

1. 主 AI 先用 `explorer` 子代理扫描 chapter4 目录，列出所有需要修改的函数
2. 根据文件数量创建 3-4 个 `implementer` 子代理，每个负责几个文件
3. 所有子代理**同时**工作
4. 完成后主 AI 汇总结果

#### 第 3 步：质量检查

AI 创建 `verifier` 子代理，运行 `mypy` 类型检查，确认所有注解正确。

---

## 五、常见问题与解决方案

### Q1："AI 不理解我的需求怎么办？"

**原因分析**：AI 对需求的理解依赖于你描述的质量和它掌握的上下文。

**解决方法**：
1. **提供更多上下文**：告诉 AI 项目背景、技术栈、你的目标
2. **举例说明**："我想要类似 pip 那样的命令行界面"
3. **分步描述**：复杂需求拆成 2-3 个小步骤
4. **让 AI 先问再动手**：说"先分析一下我的需求，有疑问再问我"

### Q2："对话历史太长怎么办？"

**解决方法**：
1. 使用 `/compact` 或 `Ctrl+L`：压缩对话历史，保留关键信息
2. 完成一个主题后新开会话
3. 使用 Sub-agents：让并行子代理分担工作，减少主会话轮次

### Q3："如何让 AI 记住项目规则？"

**解决方法**：
1. 写到 `.codewhale/instructions.md`：项目级规则，优先级最高
2. 写到 `AGENTS.md`：通用项目规则
3. 在对话中告诉 AI："记住，这个项目不用 async/await"

### Q4："AI 修改文件后怎么确认改了什么？"

**解决方法**：
1. 让 AI 调用 `git_diff`：查看所有变更
2. 让 AI 调用 `git_status`：看哪些文件被修改
3. 直接说"给我看看你都改了什么"

### Q5："能不能让 AI 同时做多件事？"

**解决方法**：
直接告诉 AI "请并行处理"，CodeWhale 的 Sub-agents 系统会自动分派。注意：只有**相互独立**的任务才能并行。

### Q6："权限提示太多怎么办？"

**解决方法**：
1. 如果信任当前项目，在安全的项目中使用 YOLO 模式
2. 大多数情况下保持 Agent（建议审批）模式，这是安全性和便利性的平衡点

---

## 六、最佳实践建议

### 给新手的 10 个实用技巧

1. **从项目指令开始**：花 5 分钟写好 `.codewhale/instructions.md`，之后每次对话 AI 都自动了解项目

2. **小步快跑**：每次只让 AI 做一件事，验证后再做下一件。比一次性给 10 个要求效率高得多

3. **善用 Checklist**：AI 自动列出清单后，你可以说"跳过第 3 步，先做第 5 步"，灵活控制流程

4. **给 AI 看代码**：不要只描述问题，把报错信息、相关文件路径告诉 AI。信息越多，分析越准

5. **学会压缩**：对话超过 30 轮后，用 `/compact` 整理。即使 1M 上下文也有限

6. **并行处理独立任务**：遇到可以独立拆分的任务时，告诉 AI 可以并行处理

7. **写清楚项目规则**：在 `.codewhale/instructions.md` 中写明"哪些目录不能动""用什么代码风格"

8. **定期回顾**：用 `git_diff` 查看 AI 的修改，确认质量

9. **合理使用子代理**：大任务让 AI 自动分派，小任务直接对话就够了

10. **利用 Prefix Cache**：不要频繁新开会话，同一个项目尽量在一个会话中持续工作

### 避免的常见错误

| 错误 | 为什么不对 | 正确做法 |
|------|-----------|----------|
| 描述太模糊 | AI 只能猜测你的意图 | 具体说明：文件路径、期望行为、限制条件 |
| 一次给太多任务 | AI 容易遗漏或搞混 | 一个一个来，验证后再接下一个 |
| 不检查 AI 的修改 | AI 也会犯错 | 用 `git_diff` 查看改动，让 AI 验证 |
| 在敏感项目中用 YOLO 模式 | 安全风险 | 日常使用保持 Agent 模式 |
| 忽略项目指令文件 | AI 缺少项目上下文 | 花时间写好 `.codewhale/instructions.md` |

### 如何逐步提升使用效率

```
Level 1 (新手):   基本对话 → 让 AI 写代码、改代码、读文件
Level 2 (入门):   使用 checklist、compact、让 AI 多步骤执行
Level 3 (进阶):   配置项目指令文件、使用 Sub-agents 并行
Level 4 (熟练):   创建自定义 Skill、配置 MCP 连接外部服务
Level 5 (专家):   使用 RLM 处理大数据、搭建团队工作流、编写 Plugin
```

### 不同场景下的模块选择建议

| 场景 | 推荐使用的模块 |
|------|---------------|
| 新项目启动 | 项目指令 + checklist + Sub-agents |
| 日常开发 | 基础对话 + git_diff/git_status |
| Bug 修复 | AI 自动排查 + 精确编辑（`edit_file`） |
| 代码审查 | Sub-agents（reviewer）+ git_diff |
| 大规模重构 | checklist + 多个 Sub-agents 并行 |
| 大数据分析 | RLM（chunk / batch 模式） |
| 团队协作 | 共享项目指令文件 + .codewhale/settings |

---

## 七、进阶功能

### 自定义 Skill 开发

创建一个"代码格式化" Skill：

**第 1 步**：创建 Skill 目录

```bash
mkdir -p .codewhale/skills/code-formatter
```

**第 2 步**：编写 `SKILL.md`

```markdown
---
name: code-formatter
description: 使用 black 格式化 Python 代码
when_to_use: 用户要求格式化代码、统一代码风格
---

# 代码格式化工作流

1. 运行 `black --check .` 检查格式问题
2. 运行 `black .` 自动修复
3. 报告修改了哪些文件
```

**第 3 步**：在对话中使用

```
/skill code-formatter
```

### RLM 进阶用法

#### 批量分类示例

面对 100 条客户反馈需要情感分类时：

1. AI 用 `rlm_open` 打开一个 RLM 会话
2. 在 RLM 中用 `sub_query_batch(dependency_mode="independent")` 并行处理 100 条
3. 返回统计结果：正面 xx 条、负面 xx 条、中性 xx 条

#### 大文件分块处理

面对一个 50K+ token 的日志文件：

1. AI 用 `rlm_open` 加载文件
2. 用 `chunk()` 切成若干个块
3. 逐块用 `sub_query()` 提取关键信息
4. 汇总得出结论

### 无头模式（Headless / CI/CD 中使用）

v0.8.58+ 支持 `codewhale exec` 无头模式，适用于脚本和 CI 环境：

```bash
# 让 AI 审查代码并输出结果
codewhale exec --allowed-tools read_file,exec_shell --max-turns 10 "fix the failing test"

# 限制禁止工具
codewhale exec --disallowed-tools write_file,edit_file --max-turns 20 "分析项目结构"

# 查看版本
codewhale --version

# 诊断配置
codewhale doctor
codewhale doctor --json   # JSON 格式输出，适用于自动诊断
```

### 自定义 Sub-agents 策略

在复杂任务中，你可以明确告诉 AI 子代理策略：

> 你："先用一个 explorer 子代理了解项目结构，然后给每个模块创建一个 implementer 子代理，最后用一个 verifier 子代理统一验证"

---

## 八、快速参考手册

### 对话中可用命令速查

#### 上下文管理

| 命令/快捷键 | 用途 |
|-------------|------|
| `/compact` | 压缩对话历史，释放上下文空间 |
| `Ctrl + L` | 同上，快捷键 |
| `/exit` / `/quit` | 退出 CodeWhale |
| `Ctrl + C` | 中断当前操作（按两次退出） |

#### AI 自动调用的核心工具

以下工具由 AI 在需要时自动调用，你不需要手动输入：

**文件操作**
| 工具 | 功能 |
|------|------|
| `read_file` | 读取文件内容（支持 PDF 自动提取） |
| `write_file` | 写入文件 |
| `edit_file` | 精确替换文件中的文本 |
| `apply_patch` | 应用 diff 补丁（多文件/多区块） |
| `file_search` | 按文件名模糊查找 |
| `list_dir` | 列出目录内容 |

**搜索**
| 工具 | 功能 |
|------|------|
| `grep_files` | 正则搜索文件内容 |
| `web_search` | 搜索引擎查询 |
| `fetch_url` | 直接获取 URL 内容 |

**Git**
| 工具 | 功能 |
|------|------|
| `git_status` | 查看工作区状态 |
| `git_diff` | 查看未提交的变更 |
| `git_show` | 查看某次提交详情 |
| `git_log` | 查看提交历史 |
| `git_blame` | 逐行归属查询 |

**任务与代理**
| 工具 | 功能 |
|------|------|
| `checklist_write` | 创建/更新任务清单 |
| `update_plan` | 更新高阶策略 |
| `task_create` | 创建持久化后台任务 |
| `task_list` | 查看任务列表 |
| `task_read` | 查看任务详情 |
| `agent_open` | 创建子代理 |
| `agent_eval` | 等待/获取子代理结果 |
| `agent_close` | 关闭子代理 |

**RLM**
| 工具 | 功能 |
|------|------|
| `rlm_open` | 打开 RLM 会话 |
| `rlm_eval` | 在 RLM 中执行代码 |
| `rlm_configure` | 配置 RLM 参数 |
| `rlm_close` | 关闭 RLM 会话 |

**验证**
| 工具 | 功能 |
|------|------|
| `run_tests` | 运行 `cargo test` |
| `run_verifiers` | 多语言验证器（Rust/Node/Python/Go） |
| `diagnostics` | 诊断工具 |

#### 子代理类型速查

| 类型 | 用途 | 是否可写 |
|------|------|----------|
| `general` | 通用编程、调试、修改（默认） | 是 |
| `explore` | 只读代码库搜索、信息收集 | 否 |
| `plan` | 架构设计、方案规划 | 最小化 |
| `review` | 代码审查、质量检查 | 否 |
| `implementer` | 专注编码实现 | 是 |
| `verifier` | 验证、测试、确认 | 否 |
| `custom` | 显式窄工具白名单 | 视配置 |

#### RLM 模式速查

| 模式 | 适用场景 | 核心函数 |
|------|----------|----------|
| CHUNK | 超大文件分块处理 | `chunk()` |
| BATCH | 大量独立条目并行处理 | `sub_query_batch(..., dependency_mode="independent")` |
| SEQUENCE | 数据依赖链 | `sub_query_sequence()` |
| RECURSE | 分解 + 审查 | `sub_query()` / `sub_rlm()` |

### 快捷键列表

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + C` | 中断当前操作（按两次退出） |
| `Ctrl + L` | 压缩上下文 |
| `Ctrl + R` | 搜索历史输入 |
| `↑ / ↓` | 浏览历史输入 |
| `Tab` | 自动补全路径和命令 |
| `Enter` | 发送消息 |

### 项目指令文件模板

#### `.codewhale/instructions.md` 模板

```markdown
# 项目概述
[简述项目做什么]

## 技术栈
- 语言: [Python/JavaScript/...]
- 框架: [React/FastAPI/...]
- 数据库: [PostgreSQL/...]

## 项目结构
```
src/
├── models/      # 数据模型
├── services/    # 业务逻辑
├── utils/       # 工具函数
└── tests/       # 测试
```

## 代码规范
- 使用 [black/ESLint] 格式化
- 函数必须写文档字符串
- 优先使用 [TypeScript/类型注解]

## 常用命令
- `npm run dev` — 启动开发服务器
- `pytest` — 运行测试
- `npm run build` — 构建

## 注意事项
- 不要修改 data/ 目录下的原始数据
- API 调用需要错误处理
```

### Constitution 核心原则速记（v0.8.65）

| 原则 | 含义 |
|------|------|
| **用户意图至上** | 你当前的请求高于过期的仓库指引、记忆和交接文档 |
| **仓库法律必须显式** | 通过 `.codewhale/constitution.json` 声明项目的持久权威 |
| **证据高于叙述** | 工具输出胜过自信的猜测，验证是任务的一部分 |
| **记忆排在最后** | 有用，但永不具备权威 |
| **换模型法律不变** | 法律在运行框架中，不在模型提示词里 |

---

> **文档版本**：基于 CodeWhale **v0.8.65**（2026-06-24 发布）
> **最后更新**：2026 年 6 月 24 日
> **使用建议**：这份文档可以当作参考手册，不需要一次读完。遇到某个功能不知道怎么做时，用浏览器的搜索功能（Ctrl+F）查找相关关键词即可。
