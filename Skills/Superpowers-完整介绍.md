# Superpowers 完整介绍

> **版本**: 6.0.3 | **作者**: [Jesse Vincent (obra)](https://github.com/obra) | **仓库**: [github.com/obra/superpowers](https://github.com/obra/superpowers)
> **许可证**: MIT | **零依赖** | **跨平台** | **最后更新**: 2026-06-29

---

## 一、概述

Superpowers 是一套**完整的软件开发方法论**，通过一组可组合的技能（Skills）和自动触发机制，赋予 AI 编码 Agent 系统化的工程能力。它不是简单的提示词集合，而是一套**Agent 行为塑造系统**（Agent Behavior-Shaping System），从根本上改变 AI 编码助手的工作方式。

**核心理念**：Agent 不应该"直接开始写代码"——而是先理解需求、头脑风暴、输出设计、制定计划、TDD 实现、代码审查、最后验证。Superpowers 让这个完整流程**自动触发、自动执行**。

Superpowers 由 [Jesse Vincent (obra)](https://github.com/obra) 创建，背后团队是 [Prime Radiant](https://primeradiant.com)。支持 **11 个 AI 编码平台**，安装后自动生效，无需手动调用。

> **社区**: [Discord](https://discord.gg/35wsABTejz) | **发布通知**: [订阅](https://primeradiant.com/superpowers/) | **招人**: [Superpowers 社区工程师](https://primeradiant.com/jobs/superpowers-community-engineer/)

---

## 二、与其他系统的区别

| 特征 | Superpowers | ECC |
|------|------------|-----|
| 定位 | 通用开发方法论 | Agent 工作流操作系统 |
| 规模 | 14 个核心技能 | 270+ 技能 + 67 Agent |
| 设计哲学 | 零依赖、极简、强约束 | 完备、全面、多语言覆盖 |
| 技能触发 | 全自动（根据上下文自动激活） | 主动调用 + 自动化 |
| 工作流 | 线性的 头脑风暴→计划→实现→审查→验证 | 网状的多维度工作流 |
| 适用场景 | 所有开发任务的标准化流程 | 特定语言/领域的深度覆盖 |

Superpowers 和 ECC 是**互补关系**而非竞争关系。Superpowers 定的是"怎么做"的方法论，ECC 提供的是"用什么工具做"的全面工具箱。

---

## 三、技能体系（14 个核心技能）

Superpowers 包含 **14 个技能**，分为两大类：

### 3.1 过程技能（Process Skills）—— 定义"如何工作"

这些技能是 Superpowers 的骨架，定义了开发任务的标准流程。

#### 1. `superpowers:brainstorming`（头脑风暴）

**触发条件**: 任何创造性的编码工作（创建功能、构建组件、添加功能、修改行为）

**核心规则**：
- 在写任何代码之前，必须理解项目上下文
- 一次只问一个问题，逐步明确需求
- 提出 2-3 种方案及权衡
- 输出设计文档并获得用户批准
- 保存设计文档到 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
- 设计文档通过后，自动调用 `writing-plans` 进入计划阶段

**硬性门禁**：在用户批准设计之前，**绝对不能**调用任何实现技能或写任何代码。这适用于所有项目——不管有多"简单"。

**工作流**：
```
探索项目上下文 → 提供可视化伴侣（如需要）
→ 逐步提问明确需求 → 提出 2-3 种方案
→ 分段展示设计 → 用户逐段审批
→ 写入设计文档 → 规范自审
→ 用户审核规范 → 过渡到实现阶段
```

**反模式警告**："这个太简单了，不需要设计"——所有项目都必须走这个流程。简单项目是最容易因未审视的假设而浪费时间的。

#### 2. `superpowers:using-git-worktrees`（使用 Git Worktrees）

**触发条件**: 设计批准后、编写计划前自动触发

**核心功能**：
- 在批准的设计方案通过后使用
- 创建隔离的工作空间到新分支
- 运行项目初始化设置
- 验证干净的测试基线（Green baseline）

**核心概念**：
- Git Worktree 允许同时检出多个分支到不同目录
- 每个 Worktree 是独立的开发空间
- 适合同时在多个功能上工作的场景

#### 3. `superpowers:writing-plans`（编写实现计划）

**触发条件**: brainstorming 完成后自动触发

**核心功能**：
- 将设计文档转化为可执行的实现计划
- 每个任务 2-5 分钟，独立、可验证
- 面向"热衷但缺乏判断力的初级工程师"编写——足够清晰，不需要任何背景知识也能执行
- 每个任务包含：精确文件路径、完整代码、验证步骤
- 强调真正的红/绿 TDD、YAGNI（你不会需要的）、DRY（不要重复自己）
- 计划文件保存到 `docs/superpowers/plans/YYYY-MM-DD-<topic>-plan.md`
- 计划经过规范审查器和文档审查器双重审核后才算完成

**设计原则**：
- 计划必须对任何工程师都可执行，不能依赖默认上下文
- 每个任务有明确的完成标准
- 任务之间解耦，便于并行执行

#### 4. `superpowers:subagent-driven-development`（子代理驱动开发）

**触发条件**: 有实现计划且任务相互独立时自动触发

**核心机制**：
- 每个任务派发一个**全新子代理**（零上下文污染）
- 子代理不继承主会话的上下文——只获得精心构造的必要信息
- 每个任务完成后执行**两阶段审查**：
  1. **规范合规审查**（spec-reviewer）：代码是否匹配规范？
  2. **代码质量审查**（code-quality-reviewer）：代码质量是否达标？
- 审查不通过 → 实现代理修复 → 重新审查
- **连续执行**：不会在任务之间停下来问"要继续吗？"

**为什么用子代理**：
- 保持主会话上下文干净，只用于协调
- 每个子代理专注单一任务，成功率高
- 两阶段审查确保质量和规范合规

**子代理提示模板**：
- `implementer-prompt.md` — 实现代理提示
- `spec-reviewer-prompt.md` — 规范审查代理提示
- `code-quality-reviewer-prompt.md` — 代码质量审查代理提示

#### 5. `superpowers:executing-plans`（执行计划）

**触发条件**: 需要并行会话执行计划时

**与 subagent-driven-development 的区别**：
- subagent-driven-development：同一个会话内执行
- executing-plans：启动**独立的并行会话**执行，含人工检查点

适用场景：任务之间紧密耦合、需要跨会话协调的情况。

#### 6. `superpowers:dispatching-parallel-agents`（调度并行代理）

**核心功能**：
- 将任务分解为可并行执行的独立单元
- 管理并行代理的调度和结果收集
- 自动处理代理失败和重试

#### 7. `superpowers:systematic-debugging`（系统化调试）

**触发条件**: 遇到任何 Bug、测试失败或非预期行为时，**在提出修复方案之前**必须使用

**核心方法论**：
- **根本原因追溯**（Root Cause Tracing）：向下钻取，找到根本逻辑限制，而非表面修复
- **基于条件的等待**（Condition-Based Waiting）：不要用固定等待时间，而是等待特定条件满足
- **纵深防御**（Defense in Depth）：从多个层面验证修复

**严格禁止**：
- 用 `try-catch` 掩盖错误
- 打临时补丁（Workaround）
- 不找到根本原因就修改代码

配套文档：
- `root-cause-tracing.md` — 根本原因追溯方法
- `condition-based-waiting.md` — 条件等待策略
- `defense-in-depth.md` — 纵深防御原则
- 测试压力案例文件用于验证调试能力

#### 8. `superpowers:test-driven-development`（测试驱动开发）

**触发条件**: 编写新功能、修复 Bug、重构代码时

**严格的 TDD 流程**：
1. **先写失败的测试**（Red）
2. **写最少的代码让测试通过**（Green）
3. **重构**（Refactor）

配套文档：`testing-anti-patterns.md` — 常见测试反模式及如何避免

#### 9. `superpowers:requesting-code-review`（请求代码审查）

**触发条件**: 代码完成、需要审查时

生成结构化的代码审查请求，包含：
- 变更摘要
- 关键决策说明
- 需要特别关注的风险点
- 审查者提示（`code-reviewer.md`）

#### 10. `superpowers:receiving-code-review`（接收代码审查）

**触发条件**: 收到代码审查反馈时

指导如何有效处理审查意见：
- 理解反馈的真实意图
- 区分必须修改和建议修改
- 逐条响应和处理
- 更新代码后重新提交审查

#### 11. `superpowers:verification-before-completion`（完成前验证）

**触发条件**: 所有任务完成后自动触发

**核心原则**：
- 在声称"完成"之前，必须验证修改是否真的生效
- 运行应用、检查行为、确认修复
- 不能只依赖测试通过——要在真实运行环境中验证

#### 12. `superpowers:finishing-a-development-branch`（完成开发分支）

**触发条件**: 功能开发完成、准备合并时

**核心流程**：
- 确保所有测试通过
- 代码审查已完成
- 呈现选项（合并/PR/保留/丢弃）
- 清理 Worktree
- 分支干净整洁

---

### 3.2 支撑技能（Support Skills）

#### 13. `superpowers:using-superpowers`（使用 Superpowers）

**触发条件**: 每次会话开始时自动加载

**这是 Superpowers 的引导技能**，定义了整个系统的运行规则：

- **指令优先级**：用户指令 > Superpowers 技能 > 默认系统提示
- **技能查找规则**：即使只有 1% 可能的技能也需要检查触发
- **Red Flags 表**：列出 12 种自我合理化的想法及其真相
- **技能优先级**：过程技能先于实现技能

**关键机制**：自动触发。用户不需要手动调用任何技能——Agent 根据上下文自动判断应该激活哪个技能。

**Red Flags 示例**：
| 想法（错误） | 真相（正确） |
|------------|------------|
| "这只是个简单的问题" | 问题是任务，检查技能 |
| "我需要先了解更多上下文" | 技能检查应该在提问之前 |
| "让我先探索代码库" | 技能告诉你如何探索 |
| "我可以通过 git 快速检查" | 文件缺少对话上下文 |
| "这不需要正式技能" | 如果有技能存在，就使用它 |
| "我记得这个技能" | 技能会进化，阅读当前版本 |
| "我先做这一件事" | 在任何操作之前先检查技能 |
| "技能太过分了" | 简单的事情会变复杂，使用它 |
| "这不算任务" | 行动 = 任务，检查技能 |
| "这感觉很有生产力" | 无纪律的行动浪费时间 |
| "我知道那是什么意思" | 知道概念 ≠ 使用技能，调用它 |

#### 14. `superpowers:writing-skills`（编写技能）

**触发条件**: 需要创建或修改技能时

提供技能编写的完整方法论：
- 技能不是散文，是**塑造 Agent 行为的代码**
- 包含 `anthropic-best-practices.md`：Anthropic 的技能编写最佳实践
- 包含 `persuasion-principles.md`：说服原则（如何让 Agent 遵循技能）
- 包含 `testing-skills-with-subagents.md`：使用子代理测试技能的方法
- 压力测试方法论：跨多个会话进行对抗性测试
- 评估要求：修改技能内容必须提供 before/after 的 eval 数据
- EVAL 工具链：[superpowers-evals](https://github.com/prime-radiant-inc/superpowers-evals/)（Drill 框架）

---

## 四、完整工作流

Superpowers 的完整开发流程是一条**线性推进的管道**：

```
用户提出需求
    │
    ▼
┌─────────────────────────────┐
│  brainstorming              │  ← 自动触发
│  理解意图 → 设计方案 → 审批 │
│  输出: 设计文档              │
└─────────────┬───────────────┘
              │ 自动过渡
              ▼
┌─────────────────────────────┐
│  using-git-worktrees        │  ← 自动触发
│  创建隔离工作空间 → 新分支   │
│  运行项目初始化 → 验证基线   │
└─────────────┬───────────────┘
              │ 自动过渡
              ▼
┌─────────────────────────────┐
│  writing-plans              │  ← 自动触发
│  设计 → 任务分解 → 实施计划  │
│  输出: 实现计划（2-5分钟/任务）│
└─────────────┬───────────────┘
              │ 自动过渡
              ▼
┌─────────────────────────────┐
│  subagent-driven-development│  ← 自动触发
│  每个任务:                   │
│  实现 → 规范审查 → 质量审查   │
│  审查不通过 → 修复 → 再审    │
└─────────────┬───────────────┘
              │ 所有任务完成
              ▼
┌─────────────────────────────┐
│  requesting-code-review     │  ← 代码审查
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  verification-before-       │
│  completion                 │  ← 验证功能
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│  finishing-a-development-   │
│  branch                     │  ← 收尾、准备 PR
└─────────────────────────────┘
```

当遇到 Bug 时：
```
发现 Bug
    │
    ▼
┌─────────────────────────────┐
│  systematic-debugging       │  ← 必须先追溯根本原因
│  禁止临时补丁和 try-catch    │
└─────────────┬───────────────┘
              │ 找到根本原因后
              ▼
┌─────────────────────────────┐
│  test-driven-development    │  ← 先写失败的回归测试
└─────────────┬───────────────┘
              │
              ▼
         修复 → 验证
```

---

## 五、设计哲学

### 5.1 零依赖原则

Superpowers 是零依赖插件。不接受任何添加第三方依赖的 PR。这保证了它在任何环境下的可靠性和可移植性。

### 5.2 高拒绝率文化

Superpowers 仓库有 **94% 的 PR 拒绝率**。几乎所有被拒绝的 PR 都是 Agent 生成的，原因是：
- 没有阅读贡献指南
- 没有真正理解问题
- 发明了不存在的问题
- 批量提交低质量 PR
- 修改了精心调校的技能内容而没有 eval 证据

**Agent 贡献者必须**：
1. 完整阅读 PR 模板并填写每项内容
2. 搜索现有已开和已关闭的 PR，确认没有重复
3. 确认这是一个真实的问题（不是理论推测）
4. 明确披露使用的模型、AI 编码平台、平台版本和所有已安装插件
5. 让人类合作伙伴审查完整 diff 后提交

### 5.3 不是散文，是代码

技能文件（SKILL.md）不是给人读的散文——它们是**塑造 Agent 行为的可执行代码**。对其内容的任何修改必须有评估证据证明改进效果。

**不接受**：
- 仅仅为了"符合 Anthropic 官方文档"而进行的合规性修改
- 没有 before/after eval 证据的技能内容修改
- 批量或"扫射式" PR

### 5.4 通用性优先

Superpowers 核心只包含对所有项目都有益的通用技能。领域特定的技能（如投资组合构建、预测市场、游戏开发）应该作为独立插件发布，不进入核心。

### 5.5 自动触发

Superpowers 的核心机制是自动触发。`using-superpowers` 引导技能在会话开始时加载，使所有技能能够根据上下文自动激活。用户不需要知道技能名称——Agent 会自动判断应该使用哪个技能。

**验收测试**：在一个全新的会话中发送：
> Let's make a react todo list

如果集成正确，Agent 应该**在写任何代码之前**自动触发 `brainstorming` 技能。

---

## 六、跨平台支持

Superpowers 支持 **11 个 AI 编码平台**：

| 平台 | 安装方式 |
|------|---------|
| **Claude Code**（官方市场） | `/plugin install superpowers@claude-plugins-official` |
| **Claude Code**（Superpowers 市场） | `/plugin marketplace add obra/superpowers-marketplace` → `/plugin install superpowers@superpowers-marketplace` |
| **Antigravity** | `agy plugin install https://github.com/obra/superpowers` |
| **Codex App** | Plugins → Coding → Superpowers → `+` |
| **Codex CLI** | `/plugins` → 搜索 "superpowers" → `Install Plugin` |
| **Cursor** | `/add-plugin superpowers` 或从市场搜索 |
| **Factory Droid** | `droid plugin marketplace add https://github.com/obra/superpowers` → `droid plugin install superpowers@superpowers` |
| **Gemini CLI** | `gemini extensions install https://github.com/obra/superpowers` |
| **GitHub Copilot CLI** | `copilot plugin marketplace add obra/superpowers-marketplace` → `copilot plugin install superpowers@superpowers-marketplace` |
| **Kimi Code** | `/plugins` → `Marketplace` > `Superpowers` |
| **OpenCode** | 执行指令：`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md` |
| **Pi** | `pi install git:github.com/obra/superpowers` |

---

## 七、技能触发机制

Superpowers 的技能使用**声明式触发条件**（description frontmatter），Agent 在收到用户消息后自动匹配：

```
用户消息 → 匹配技能的 description 字段 → 触发相关技能
```

当前的上下文窗口在 Cursor 等编辑器中可以支持同时加载所有 14 个技能的触发条件，实现真正的全自动触发。

---

## 八、视觉伴侣（Visual Companion）与 Telemetry

brainstorming 技能包含可选的视觉伴侣功能，在设计过程中使用 Prime Radiant Logo 生成可视化展示。该 Logo 从官方网站加载，**仅包含 Superpowers 版本号**，不包含任何项目详情、提示词或 Agent 信息。

**完全可选**。可通过以下方式禁用：
- 设置环境变量 `SUPERPOWERS_DISABLE_TELEMETRY` 为任意 true 值
- Superpowers 也自动遵循 `DISABLE_TELEMETRY` 和 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`

---

## 九、文件结构

```
superpowers/
├── skills/
│   ├── brainstorming/
│   │   ├── SKILL.md
│   │   ├── spec-document-reviewer-prompt.md
│   │   └── visual-companion.md
│   ├── writing-plans/
│   │   ├── SKILL.md
│   │   └── plan-document-reviewer-prompt.md
│   ├── subagent-driven-development/
│   │   ├── SKILL.md
│   │   ├── implementer-prompt.md
│   │   ├── spec-reviewer-prompt.md
│   │   └── code-quality-reviewer-prompt.md
│   ├── systematic-debugging/
│   │   ├── SKILL.md
│   │   ├── root-cause-tracing.md
│   │   ├── condition-based-waiting.md
│   │   └── defense-in-depth.md
│   ├── test-driven-development/
│   │   ├── SKILL.md
│   │   └── testing-anti-patterns.md
│   ├── executing-plans/SKILL.md
│   ├── dispatching-parallel-agents/SKILL.md
│   ├── finishing-a-development-branch/SKILL.md
│   ├── receiving-code-review/SKILL.md
│   ├── requesting-code-review/
│   │   ├── SKILL.md
│   │   └── code-reviewer.md
│   ├── using-git-worktrees/SKILL.md
│   ├── using-superpowers/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── verification-before-completion/SKILL.md
│   └── writing-skills/
│       ├── SKILL.md
│       ├── anthropic-best-practices.md
│       ├── persuasion-principles.md
│       └── testing-skills-with-subagents.md
├── docs/
│   ├── superpowers/
│   │   ├── specs/     # 设计文档
│   │   └── plans/     # 实现计划
│   ├── porting-to-a-new-harness.md
│   ├── testing.md
│   └── windows/
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .opencode/
│   ├── INSTALL.md
│   └── plugins/
├── .pi/
│   └── extensions/
├── CLAUDE.md      # 贡献者指南
└── README.md
```

---

## 十、Design Doc 和 Plan 的存放约定

Superpowers 有严格的文档存放约定：

- **设计文档**写入 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
- **实现计划**写入 `docs/superpowers/plans/YYYY-MM-DD-<topic>-plan.md`

设计文档生成后进行**规范自审**（spec self-review）：检查占位符、矛盾、歧义和范围问题。然后要求用户审核规范文件本身，而不仅仅是对话中的摘要。这确保用户真正看到并理解了完整的设计。

---

## 十一、总结

Superpowers 是一套**经过实战验证的 Agent 行为塑造系统**，通过 14 个精心调校的技能，将 AI 编码 Agent 从"直接写代码的助手"转变为"遵循完整工程方法的专业开发者"。它的核心价值在于：

1. **自动触发**：用户不需要学习任何命令，Agent 自动判断何时使用哪个技能
2. **防跳步**：强制的 brainstorm → worktree → plan → implement → review → verify 流程防止 Agent 草率行事
3. **零依赖**：纯 Markdown 技能文件，不需要任何外部工具
4. **跨平台**：同一套技能，11 个平台通用
5. **经过验证**：94% 的 PR 拒绝率意味着只有真正经过充分测试的改动才能进入核心

Superpowers 和 ECC 的最佳实践是**同时使用**：Superpowers 定义标准化的工作流方法论，ECC 提供特定领域的深度工具覆盖。

> **社区**: [Discord](https://discord.gg/35wsABTejz) | **发布通知**: [订阅](https://primeradiant.com/superpowers/) | **公司**: [Prime Radiant](https://primeradiant.com) | **商业支持**: sales@primeradiant.com