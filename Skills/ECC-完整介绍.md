# ECC (Everything Claude Code) 完整介绍

> **版本**: 2.0.0 | **作者**: [Affaan Mustafa](https://x.com/affaanmustafa) | **仓库**: [github.com/affaan-m/ECC](https://github.com/affaan-m/ECC)
> **官网**: [ecc.tools](https://ecc.tools) | **许可证**: MIT | **最后更新**: 2026-06-29
> **223K+ Stars** | **34.2K+ Forks** | **230+ 贡献者** | **12+ 语言生态**

---

## 一、概述

ECC（Everything Claude Code）是一个**面向 AI Agent 编码工作流的完整操作系统级插件**，也是目前 Claude Code 生态中规模最大、最全面的 Agent 工作流系统。它不是简单的配置集合，而是一套完整的：**专用 Agent 矩阵 + 技能工作流 + Hook 自动化 + 安全审计 + 持续学习 + 研究工具链 + 跨平台适配层**。

ECC 的核心定位是 **Agent Harness Operating System**（Agent 运行框架操作系统）—— 在 Claude Code、Codex、Cursor、OpenCode、Gemini CLI、Zed、GitHub Copilot、Trae、Kiro、Qwen 等多个 AI 编码平台之上，提供统一的工作流编排、质量保障和运维管控能力。

---

## 二、核心数据

| 维度 | 数量 |
|------|------|
| 专用 Agent | **67 个** |
| 技能（Skills） | **271 个** |
| 命令 / Legacy Command Shims | **92 个** |
| 支持的语言生态 | **12+**（TypeScript、Python、Go、Java、Kotlin、Rust、C++、F#、Swift、Dart/Flutter、PHP、Perl） |
| 支持的编码平台 | **8+**（Claude Code、Codex、Cursor、OpenCode、Gemini CLI、Zed、GitHub Copilot、Trae、Kiro、Qwen） |
| 内部测试 | **1300+ 个**（含 AgentShield 独立 1282 项测试 + 主套件持续扩展） |
| Git 提交 | **2188+ 个** |
| 社区 PR 合并 | **200+** |

---

## 三、架构层次

ECC 采用分层架构，从底层到顶层共 7 个维度：

### 3.1 Agents（专用 Agent 矩阵）

ECC 提供了 **67 个专用 Agent**，每个 Agent 聚焦于特定的专业领域，可被主会话按需调度。Agent 分为以下几大类：

#### 语言审查 Agent（Language Reviewers）

| Agent | 用途 |
|-------|------|
| `code-reviewer` | 通用代码审查：质量、安全、可维护性 |
| `typescript-reviewer` | TypeScript/JavaScript 专项审查 |
| `python-reviewer` | Python 专项审查（PEP 8、类型标注、Pythonic 惯用法） |
| `go-reviewer` | Go 专项审查（惯用法、并发、错误处理） |
| `java-reviewer` | Java/Spring Boot 专项审查 |
| `kotlin-reviewer` | Kotlin/Android/KMP 专项审查 |
| `rust-reviewer` | Rust 专项审查（所有权、生命周期、unsafe） |
| `cpp-reviewer` | C/C++ 专项审查（内存安全、现代 C++） |
| `fsharp-reviewer` | F# 函数式审查 |
| `swift-reviewer` | Swift 专项审查 |
| `django-reviewer` | Django ORM/DRF/迁移审查 |
| `fastapi-reviewer` | FastAPI 异步/DI/Pydantic 审查 |
| `database-reviewer` | PostgreSQL/Supabase 数据库审查 |
| `flutter-reviewer` | Flutter/Dart Widget 审查 |
| `vue-reviewer` | Vue.js Composition API 审查 |
| `react-reviewer` | React Hook/SSR/性能审查 |
| `php-reviewer` | PHP/PSR-12 审查 |
| `csharp-reviewer` | C# .NET 审查 |
| `healthcare-reviewer` | 医疗代码审查（PHI 合规、CDS 安全） |
| `mle-reviewer` | 生产 ML 管线审查 |
| `conversation-analyzer` | 对话/交互质量分析 |

#### 构建/错误修复 Agent（Build/Error Resolvers）

| Agent | 用途 |
|-------|------|
| `build-error-resolver` | 通用构建/TypeScript 错误修复 |
| `cpp-build-resolver` | C/C++ CMake/编译错误修复 |
| `go-build-resolver` | Go build/vet 错误修复 |
| `java-build-resolver` | Java/Maven/Gradle 错误修复 |
| `kotlin-build-resolver` | Kotlin/Gradle 错误修复 |
| `rust-build-resolver` | Rust cargo/borrow-checker 错误修复 |
| `react-build-resolver` | React/Vite/Webpack/Next.js 构建错误修复 |
| `django-build-resolver` | Django 迁移/依赖错误修复 |
| `pytorch-build-resolver` | PyTorch CUDA/训练错误修复 |
| `dart-build-resolver` | Dart/Flutter 分析错误修复 |
| `swift-build-resolver` | Swift/Xcode 构建错误修复 |
| `harmonyos-app-resolver` | 鸿蒙 HarmonyOS/ArkTS 应用开发 |
| `a11y-architect` | 无障碍架构设计 |

#### 流程与架构 Agent

| Agent | 用途 |
|-------|------|
| `architect` | 系统架构设计与技术决策 |
| `planner` | 复杂功能实现规划 |
| `tdd-guide` | TDD 测试驱动开发引导 |
| `code-explorer` | 代码库深度分析 |
| `code-architect` | 基于现有模式的架构设计 |
| `code-simplifier` | 代码简化与重构 |
| `spec-miner` | 棕地项目行为规范提取 |

#### 质量与安全 Agent

| Agent | 用途 |
|-------|------|
| `security-reviewer` | OWASP Top 10 安全漏洞检测 |
| `silent-failure-hunter` | 静默失败/错误吞噬检测 |
| `comment-analyzer` | 代码注释质量分析 |
| `type-design-analyzer` | 类型设计分析 |
| `performance-optimizer` | 性能瓶颈分析 |
| `refactor-cleaner` | 死代码清理 |

#### 运维与自动化 Agent

| Agent | 用途 |
|-------|------|
| `e2e-runner` | Playwright 端到端测试 |
| `doc-updater` | 文档与代码地图更新 |
| `docs-lookup` | Context7 API 文档查询 |
| `loop-operator` | 自主循环执行与监控 |
| `harness-optimizer` | Agent 配置调优 |
| `pr-test-analyzer` | PR 测试覆盖分析 |
| `agent-evaluator` | Agent 输出质量 5 轴评分 |

#### 领域专项 Agent

| Agent | 用途 |
|-------|------|
| `marketing-agent` | 营销策略与文案 |
| `chief-of-staff` | 邮件/Slack/LINE 通信分类 |
| `homelab-architect` | 家庭/小型网络设计 |
| `network-architect` | 企业网络架构设计 |
| `network-troubleshooter` | 网络故障诊断 |
| `network-config-reviewer` | 路由器/交换机配置审查 |
| `seo-specialist` | 技术 SEO 审计 |

#### GAN（生成对抗网络）Agent 三件套

| Agent | 用途 |
|-------|------|
| `gan-planner` | 将一句话 prompt 扩展为完整产品规格 |
| `gan-generator` | 按规格实现功能 |
| `gan-evaluator` | 通过 Playwright 测试评分、反馈给 Generator |

---

### 3.2 Skills（技能工作流）

ECC 包含 **271 个技能**，按功能领域分类如下：

#### 核心开发流程

| Skill | 功能 |
|-------|------|
| `ecc:code-review` | 代码审查（本地变更或 GitHub PR） |
| `ecc:python-review` | Python 专项审查 |
| `ecc:go-review` | Go 专项审查 |
| `ecc:rust-review` | Rust 专项审查 |
| `ecc:react-review` | React 专项审查 |
| `ecc:vue-review` | Vue.js 专项审查 |
| `ecc:cpp-review` | C++ 专项审查 |
| `ecc:cpp-test` | C++ TDD 工作流 |
| `ecc:security-review` | 安全漏洞审查 |
| `ecc:security-scan` | AgentShield 安全扫描 |
| `ecc:test-coverage` | 测试覆盖率分析与补充 |
| `ecc:build-fix` | 构建错误增量修复 |
| `ecc:refactor-clean` | 死代码识别与安全删除 |
| `ecc:tdd-workflow` | TDD 测试驱动开发流程 |

#### 规划与编排

| Skill | 功能 |
|-------|------|
| `ecc:plan` | 需求重述、风险评估、分步实施计划 |
| `ecc:plan-prd` | 问题驱动的 PRD 生成 |
| `ecc:feature-dev` | 有代码库理解的引导式功能开发 |
| `ecc:multi-plan` | 多模型实现计划（不修改生产代码） |
| `ecc:multi-execute` | 多模型计划执行 |
| `ecc:multi-workflow` | 完整多模型开发工作流 |
| `ecc:multi-frontend` | 前端多模型工作流 |
| `ecc:multi-backend` | 后端多模型工作流 |

#### 编排器（Orchestrator）系列

| Skill | 功能 |
|-------|------|
| `ecc:orch-add-feature` | 编排全新功能开发（研究→计划→TDD→审查→提交） |
| `ecc:orch-build-mvp` | 编排 MVP 搭建（规格→切割→脚手架→TDD→审查） |
| `ecc:orch-change-feature` | 编排功能修改（更新测试→改实现→审查） |
| `ecc:orch-fix-defect` | 编排缺陷修复（复现→回归测试→修复→审查） |
| `ecc:orch-refine-code` | 编排行为保留重构 |

#### 项目与配置管理

| Skill | 功能 |
|-------|------|
| `ecc:project-init` | 项目技术栈检测与 ECC 接入方案 |
| `ecc:configure-ecc` | ECC 交互式安装配置向导 |
| `ecc:harness-audit` | 仓库 Agent 配置健康审计 |
| `ecc:model-route` | 基于任务复杂度的模型选择建议 |
| `ecc:cost-report` | Claude Code 使用成本报告 |
| `ecc:pm2` | PM2 服务管理命令生成 |
| `ecc:quality-gate` | 代码格式化质量门禁 |
| `ecc:setup-pm` | 包管理器检测与配置 |

#### 会话管理

| Skill | 功能 |
|-------|------|
| `ecc:sessions` | 会话历史管理 |
| `ecc:save-session` | 保存当前会话状态 |
| `ecc:resume-session` | 从上次会话恢复工作 |
| `ecc:checkpoint` | 工作流检查点管理 |
| `ecc:loop-start` | 启动自主循环模式 |
| `ecc:loop-status` | 检查循环状态 |

#### 学习与进化

| Skill | 功能 |
|-------|------|
| `ecc:learn` | 从会话中提取可复用模式并保存为技能 |
| `ecc:learn-eval` | 提取模式 + 质量自评 + 确定保存位置 |
| `ecc:skill-create` | 分析 Git 历史生成 SKILL.md |
| `ecc:skill-health` | 技能组合健康仪表板 |
| `ecc:skill-stocktake` | 技能存量盘点 |
| `ecc:instinct-status` | 查看已学的直觉（项目+全局） |
| `ecc:instinct-export` | 导出直觉到文件 |
| `ecc:instinct-import` | 从文件/URL 导入直觉 |
| `ecc:prune` | 删除 30 天未提升的待处理直觉 |
| `ecc:promote` | 将项目级直觉提升到全局 |

#### Hook 与规则管理

| Skill | 功能 |
|-------|------|
| `ecc:hookify` | 从对话分析创建 Hook 规则 |
| `ecc:hookify-list` | 列出所有 Hook 规则 |
| `ecc:hookify-configure` | 交互式启停 Hook 规则 |
| `ecc:hookify-help` | Hook 系统帮助 |

#### Epic/Issue 管理（企业功能）

| Skill | 功能 |
|-------|------|
| `ecc:epic-claim` | 认领 Epic Issue |
| `ecc:epic-decompose` | 将 Epic 分解为子任务 |
| `ecc:epic-publish` | 发布已验证的 Epic 更新 |
| `ecc:epic-review` | 标记 Epic 审查状态 |
| `ecc:epic-sync` | 同步 Epic Issue 与本地状态 |
| `ecc:epic-unblock` | 扫描阻塞的 Epic |
| `ecc:epic-validate` | Epic 就绪性验证 |

#### PR 工作流

| Skill | 功能 |
|-------|------|
| `ecc:pr` | 从当前分支创建 GitHub PR |
| `ecc:review-pr` | 使用专用 Agent 进行综合 PR 审查 |
| `ecc:prp-pr` | 自然语言描述变更并创建 PR |
| `ecc:prp-commit` | 快速提交（自然语言选择文件） |
| `ecc:prp-plan` | 创建综合功能实施计划 |
| `ecc:prp-prd` | 交互式 PRD 生成器 |
| `ecc:prp-implement` | 执行实施计划与严格验证 |

#### 商业与运营

| Skill | 功能 |
|-------|------|
| `ecc:brand-discovery` | 品牌发现（7 维度分析） |
| `ecc:brand-voice` | 品牌语调定义 |
| `ecc:marketing-campaign` | 完整营销活动策划与执行 |
| `ecc:content-engine` | 内容引擎 |
| `ecc:market-research` | 市场研究 |
| `ecc:competitive-platform-analysis` | 竞争平台分析 |
| `ecc:investor-materials` | 投资人材料制作 |
| `ecc:investor-outreach` | 投资人外联 |
| `ecc:article-writing` | 文章写作 |
| `ecc:social-publisher` | 社交媒体发布 |
| `ecc:crosspost` | 跨平台内容发布 |
| `ecc:social-graph-ranker` | 社交图谱排名 |
| `ecc:connections-optimizer` | 社交连接优化 |
| `ecc:customer-billing-ops` | 客户计费运营 |
| `ecc:ecc-tools-cost-audit` | ECC Tools 成本审计 |
| `ecc:google-workspace-ops` | Google Workspace 运营 |
| `ecc:project-flow-ops` | 项目流程运营 |
| `ecc:workspace-surface-audit` | 工作空间面审计 |

#### 媒体与展示

| Skill | 功能 |
|-------|------|
| `ecc:frontend-slides` | 零依赖 HTML 演示文稿构建 |
| `ecc:frontend-design-direction` | 前端设计方向指导 |
| `ecc:manim-video` | Manim 数学动画视频 |
| `ecc:remotion-video-creation` | Remotion 视频创建 |
| `ecc:video-editing` | 视频编辑 |
| `ecc:liquid-glass-design` | Liquid Glass 设计风格 |
| `ecc:ios-icon-gen` | iOS 图标生成 |

#### 网络与基础设施

| Skill | 功能 |
|-------|------|
| `ecc:network-bgp-diagnostics` | BGP 诊断 |
| `ecc:network-config-validation` | 网络配置验证 |
| `ecc:network-interface-health` | 网络接口健康检查 |
| `ecc:netmiko-ssh-automation` | Netmiko SSH 自动化 |
| `ecc:cisco-ios-patterns` | Cisco IOS 配置模式 |
| `ecc:homelab-network-setup` | 家庭网络搭建 |
| `ecc:homelab-pihole-dns` | Pi-hole DNS 配置 |
| `ecc:homelab-vlan-segmentation` | VLAN 分割 |
| `ecc:homelab-wireguard-vpn` | WireGuard VPN 配置 |
| `ecc:homelab-network-readiness` | 家庭网络就绪检查 |

#### 数据与数据库

| Skill | 功能 |
|-------|------|
| `ecc:database-migrations` | 数据库迁移 |
| `ecc:mysql-patterns` | MySQL 模式 |
| `ecc:postgres-patterns` | PostgreSQL 模式 |
| `ecc:redis-patterns` | Redis 模式 |
| `ecc:prisma-patterns` | Prisma ORM 模式 |
| `ecc:jpa-patterns` | JPA/Hibernate 模式 |
| `ecc:kotlin-exposed-patterns` | Kotlin Exposed 框架模式 |
| `ecc:clickhouse-io` | ClickHouse 分析/查询/数据工程 |

#### 深度学习与 MLOps

| Skill | 功能 |
|-------|------|
| `ecc:mle-workflow` | MLE 完整工作流 |
| `ecc:pytorch-patterns` | PyTorch 深度学习模式 |
| `ecc:foundation-models-on-device` | 设备端基础模型 |
| `ecc:ml-adoption-playbook` | ML 采用策略手册 |
| `ecc:recsys-pipeline-architect` | 推荐系统管线架构 |
| `ecc:benchmark` | 基准测试 |
| `ecc:benchmark-methodology` | 基准方法论 |
| `ecc:benchmark-optimization-loop` | 基准优化循环 |

#### 优化与性能

| Skill | 功能 |
|-------|------|
| `ecc:parallel-execution-optimizer` | 并行执行优化 |
| `ecc:data-throughput-accelerator` | 数据吞吐加速 |
| `ecc:latency-critical-systems` | 延迟关键系统 |
| `ecc:recursive-decision-ledger` | 递归决策账本 |

#### 预测市场与交易

| Skill | 功能 |
|-------|------|
| `ecc:ito-market-intelligence` | Itô 市场情报 |
| `ecc:ito-basket-compare` | Itô 篮子比较 |
| `ecc:ito-trade-planner` | Itô 交易规划器 |
| `ecc:ito-data-atlas-agent` | Itô 数据地图 Agent |
| `ecc:prediction-market-oracle-research` | 预测市场 Oracle 研究 |
| `ecc:prediction-market-risk-review` | 预测市场风险评估 |

#### 安全与合规

| Skill | 功能 |
|-------|------|
| `ecc:security-bounty-hunter` | 安全赏金猎人模式 |
| `ecc:safety-guard` | 安全守卫 |
| `ecc:hipaa-compliance` | HIPAA 合规 |
| `ecc:defi-amm-security` | DeFi AMM 安全审查 |
| `ecc:opensource-pipeline` | 开源发布管线（fork→sanitizer→packager） |
| `ecc:opensource-forker` | 开源 Fork（脱敏处理） |
| `ecc:opensource-sanitizer` | 开源安全扫描（20+ 正则模式） |
| `ecc:opensource-packager` | 开源打包 |

#### 其他领域技能（速览）

- **前端框架**: `react-patterns`、`react-performance`、`react-testing`、`vue-patterns`、`angular-developer`、`nuxt4-patterns`、`nestjs-patterns`、`vite-patterns`、`nextjs-turbopack`、`bun-runtime`
- **后端框架**: `django-patterns`、`django-security`、`django-tdd`、`springboot-patterns`、`springboot-security`、`fastapi-patterns`、`laravel-patterns`、`laravel-security`、`laravel-tdd`、`quarkus-patterns`
- **移动开发**: `flutter-dart-code-review`、`swiftui-patterns`、`android-clean-architecture`、`compose-multiplatform-patterns`、`harmonyos-app-resolver`、`swift-concurrency-6-2`
- **DevOps**: `docker-patterns`、`kubernetes-patterns`、`deployment-patterns`、`flox-environments`
- **AI/Agent**: `agentic-engineering`、`agentic-os`、`ai-first-engineering`、`autonomous-agent-harness`、`autonomous-loops`、`mcp-server-patterns`、`agent-payment-x402`、`agent-sort`、`agent-self-evaluation`、`agent-harness-construction`、`agent-introspection-debugging`、`agent-eval`
- **物流/运营**: `logistics-exception-management`、`returns-reverse-logistics`、`inventory-demand-planning`、`production-scheduling`、`quality-nonconformance`、`carrier-relationship-management`
- **科学/研究**: `scientific-thinking-literature-review`、`scientific-thinking-scholar-evaluation`、`scientific-db-pubmed-database`、`scientific-db-uspto-database`
- **Google Workspace**: `google-workspace-ops`
- **软件开发哲学**: `ai-first-engineering`、`intent-driven-development`、`search-first`、`coding-standards`、`hexagonal-architecture`、`api-design`、`architecture-decision-records`、`code-tour`、`codebase-onboarding`、`blueprint`、`canary-watch`
- **无障碍**: `accessibility`
- **浏览器/QA**: `browser-qa`、`click-path-audit`
- **API/集成**: `api-connector-builder`、`x-api`

---

### 3.3 Hooks（自动化钩子系统）

ECC 的 Hook 系统支持 **6 种事件触发类型**，实现全生命周期的自动化执行：

| 事件类型 | 触发时机 | 典型用途 |
|---------|---------|---------|
| `PreToolUse` | 工具执行前 | 验证、安全提醒 |
| `PostToolUse` | 工具执行后 | 格式化、反馈循环 |
| `UserPromptSubmit` | 用户发送消息时 | 输入预处理 |
| `Stop` | Claude 完成响应时 | 会话摘要生成 |
| `PreCompact` | 上下文压缩前 | 关键状态保存 |
| `Notification` | 权限请求时 | 通知分发 |

**典型应用**：
- 在 `git commit` 前自动运行安全扫描
- 在长时间命令（npm/pytest）执行前提醒使用 tmux
- 会话停止时生成摘要
- 上下文压缩前保存关键状态

**运行时配置**：
```bash
ECC_HOOK_PROFILE=minimal|standard|strict   # 设置 Hook 配置等级
ECC_DISABLED_HOOKS=pre:bash:gate,...       # 禁用特定 Hook
ECC_SESSION_START_MAX_CHARS=4000            # 限制会话启动附加上下文（默认 8000）
ECC_SESSION_START_CONTEXT=off               # 完全禁用 SessionStart 上下文
ECC_SESSION_RETENTION_DAYS=14               # session-tmp 保留天数
```

---

### 3.4 Rules（编码规则系统）

ECC 提供按语言分组的模块化规则文件，覆盖 **12+ 语言生态**：

```
rules/
├── common/          # 通用编码最佳实践
├── typescript/      # TypeScript 规则
├── python/          # Python 规则
├── golang/          # Go 规则
├── java/            # Java 规则
├── kotlin/          # Kotlin 规则
├── rust/            # Rust 规则
├── cpp/             # C/C++ 规则
├── php/             # PHP 规则
├── perl/            # Perl 规则
└── ...
```

规则覆盖：代码风格、安全检查、测试要求、Git 工作流、Agent 调度等。

---

### 3.5 Instincts（持续学习/直觉系统）

ECC 的 Instinct 系统能从会话中**自动提取可复用模式**，形成"直觉"（Instinct），分为两层：

- **项目级（Project）**：针对特定项目的模式
- **全局级（Global）**：跨项目通用的模式

每个 Instinct 带有置信度评分，支持：
- 从会话自动提取（`/learn`）
- 导入/导出（`/instinct-import`、`/instinct-export`）
- 项目→全局提升（`/promote`）
- 过期清理（`/prune`，30 天未提升的直觉自动清理）

---

### 3.6 Commands（命令行入口）

ECC 提供 **92 个**简洁的斜杠命令（slash commands），作为技能系统的快捷入口：

```bash
# 代码审查
/code-review        # 审查当前 diff
/security-review    # 安全审查

# 开发流程
/plan               # 制定实施计划
/tdd                # TDD 工作流
/build-fix          # 修复构建错误

# 项目管理
/epic-sync          # 同步 Epic 状态
/pr                 # 创建 PR
/save-session       # 保存会话

# 分析与优化
/harness-audit      # Agent 配置审计
/model-route        # 模型选择建议
/cost-report        # 费用报告
```

---

### 3.7 ECC 2.0 控制面（Rust + Python）

ECC 2.0 在常规 CLI 之外新增了双控制面：

#### Rust 控制面（ecc2/）

一个独立的 Rust 控制面程序，提供：

- **TUI 仪表板**：终端 UI 管理所有 Agent 会话
- **SQLite 会话存储**：结构化会话状态管理
- **会话生命周期**：start / stop / resume / daemon
- **Worktree 集成**：隔离的并行开发环境
- **可观性与风险评分**：会话级别的监控与评估
- **Operator 状态快照**：`ecc status --markdown --write status.md`

```bash
cd ecc2
cargo run -- dashboard     # 启动仪表板
cargo run -- start --task "审计仓库" --agent claude --worktree
cargo run -- sessions       # 列出会话
cargo run -- status         # 状态快照
```

#### Python 桌面仪表板（ecc_dashboard.py）

基于 Tkinter 的桌面 GUI 应用：

```bash
npm run dashboard
# 或
python3 ./ecc_dashboard.py
```

**功能特点：**
- 标签式界面：Agents、Skills、Commands、Rules、Settings
- 深色/浅色主题切换
- 字体定制（家族和大小）
- 搜索和筛选所有组件
- 项目 Logo 在标题栏和任务栏

### 3.8 控制面子系统（v2.0.0 新增）

| 子系统 | 描述 |
|--------|------|
| **Session Adapters** (`ecc.session.v1`) | 跨 Harness 统一的会话适配层（Claude Code、Codex、OpenCode、dmux） |
| **MCP Inventory** (`ecc.mcp.v1`) | MCP 服务器配置归一化视图，含碎片检测、漂移检测和密钥脱敏 |
| **Worktree-Lifecycle Service** | 确定性的冲突预测和安全垃圾回收，用于并行 Agent Worktree |
| **Selective Install** | 清单驱动的安装管线，`install-plan.js` + `install-apply.js` 实现增量组件安装 |

---

## 四、安装方式

### 方式一：插件安装（推荐）

```bash
# 添加市场
/plugin marketplace add https://github.com/affaan-m/ECC

# 安装插件
/plugin install ecc@ecc
```

安装后手动复制规则文件：

```bash
mkdir -p ~/.claude/rules/ecc
cp -R rules/common ~/.claude/rules/ecc/
cp -R rules/typescript ~/.claude/rules/ecc/
# 按需复制其他语言规则
```

### 方式二：手动安装

```bash
git clone https://github.com/affaan-m/ECC.git
cd ECC
npm install
./install.sh --profile full
```

### 选择性安装

```bash
# 最小安装（无 Hooks）
./install.sh --profile minimal --target claude

# 按组件咨询
npx ecc consult "security reviews" --target claude
```

### 重要提示：不要混用安装方式

> ⚠️ **不要堆叠安装方法。** 最常见的安装损坏场景是：先 `/plugin install`，再执行 `install.sh --profile full`，两者叠加会导致技能和运行行为重复。
>
> 如果已混装，按以下顺序清理：
> 1. 移除 Claude Code 插件安装
> 2. 从仓库根目录运行 `node scripts/uninstall.js`
> 3. 删除额外手动复制的规则目录
> 4. 使用单一方式重新安装

---

## 五、新手指南

ECC 仓库提供三份深度指南：

| 指南 | 内容 | 链接 |
|------|------|------|
| **Shorthand Guide** | 安装、基础、哲学（先读这个） | [the-shortform-guide.md](https://github.com/affaan-m/ECC/blob/main/the-shortform-guide.md) |
| **Longform Guide** | 令牌优化、记忆持久化、评估、并行化 | [the-longform-guide.md](https://github.com/affaan-m/ECC/blob/main/the-longform-guide.md) |
| **Security Guide** | 攻击向量、沙箱、脱敏、CVE、AgentShield | [the-security-guide.md](https://github.com/affaan-m/ECC/blob/main/the-security-guide.md) |

---

## 六、设计哲学

1. **Agent-First**：优先委派给专业 Agent 处理领域任务
2. **Test-Driven**：测试先行，要求 80%+ 覆盖率
3. **Security-First**：永不妥协安全，所有输入必须验证
4. **Immutability**：始终创建新对象，永不修改现有对象
5. **Plan Before Execute**：复杂功能先规划再编码
6. **Cross-Harness**：同一套系统，多平台运行
7. **Operator-First**：以 Operator 体验为中心构建工作流

---

## 七、ECC Pro 与企业版

ECC 开源版永久 MIT 许可证。ECC Pro（$19/seat/mo）提供：

- 私有仓库支持
- GitHub App 安装（[github.com/apps/ecc-tools](https://github.com/apps/ecc-tools)）
- PR 自动审计
- AgentShield 高级安全扫描
- 团队配置同步
- 企业治理控制

---

## 八、关键术语

| 术语 | 说明 |
|------|------|
| **Agent** | 可被调度的专业 AI 子代理，具有限定的工具权限 |
| **Skill** | 可复用的工作流定义（SKILL.md），是 ECC 的核心单元 |
| **Command** | 斜杠命令，Skills 的快捷入口 |
| **Legacy Command Shim** | 旧式命令兼容层，平滑迁移到 Skills 体系 |
| **Hook** | 基于事件触发的自动化脚本 |
| **Rule** | 按语言分组的编码规范文件 |
| **Instinct** | 从会话中学习到的可复用模式 |
| **Harness** | AI 编码平台（Claude Code、Codex、Cursor 等） |
| **Worktree** | Git 工作树，用于隔离的并行开发 |
| **MCP** | Model Context Protocol，连接外部服务的协议 |
| **AgentShield** | ECC 内置的安全扫描引擎，1282 项测试，102 条规则 |
| **Hermes** | ECC 2.0 的 Operator 操作者身份/故事层 |
| **Orchestrator** | 端到端工作流编排器（`orch-*` 系列） |
| **Session Adapter** | 跨 Harness 的会话状态适配层 |

---

## 九、版本历史速览

| 版本 | 时间 | 亮点 |
|------|------|------|
| **v2.0.0** | 2026-06 | 稳定版：261 技能、Session Adapters、MCP Inventory、Worktree-Lifecycle、orch-* 编排器、Discord 社区 |
| **v2.0.0-rc.1** | 2026-04 | Dashboard GUI、66 Agents/268 Skills/84 Shims、Itô 预测市场包、ECC 2.0 Rust α、优化包、媒体技能 |
| **v1.9.0** | 2026-03 | 选择性安装、6 新 Agent、12 语言规则、SQLite 状态存储 |
| **v1.8.0** | 2026-03 | Hook 运行时控制、997 测试通过、NanoClaw v2 |
| **v1.7.0** | 2026-02 | Codex 支持、前端幻灯片技能、5 商业技能、992 测试 |
| **v1.6.0** | 2026-02 | Codex CLI、AgentShield（1282 测试）、GitHub Marketplace、978 测试 |
| **v1.4.0** | 2026-02 | 多语言规则、安装向导、PM2、中文翻译 |
| **v1.3.0** | 2026-02 | OpenCode 插件支持 |
| **v1.2.0** | 2026-02 | Python/Django/Spring Boot 技能、会话管理、持续学习 v2 |

---

## 十、总结

ECC 是目前 AI 编码领域最完整的 Agent 工作流系统。它将 12+ 个月的实战经验沉淀为 **67 个专业 Agent**、**271 个技能**、**92 个命令**和完整的自动化体系。无论是单人开发还是企业团队，ECC 都提供了一套从代码编写、审查、测试、安全扫描到持续学习的全生命周期解决方案。

> **社区**: [Discord](https://discord.gg/36yGMHGFbR) | **赞助**: [GitHub Sponsors](https://github.com/sponsors/affaan-m) | **商业支持**: [ECC Pro](https://ecc.tools/pricing)
