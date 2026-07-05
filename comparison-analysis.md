# 长征工程方法论 vs GitHub 同类 Skill 对比分析

> 调研时间：2026-07-03
> 调研范围：GitHub 上 6 个与"工程方法论/架构迁移/流程框架"相关的 Claude Code Skill

---

## 一、调研对象总览

| Skill | 作者 | Stars | 核心定位 | 方法论来源 |
|-------|------|-------|---------|----------|
| **refactor-architecture** | AMOSKILL45 | 待查 | 8-phase 安全重构大型 TS/React 单体代码库 | 真实项目 87 个 PR 的经验沉淀 |
| **skill-evolve** | OrangeViolin | 待查 | 演进式 Skill 改进（观察→提炼→迭代） | OTF + JIT + Bootstrap 方法论 |
| **c3-skill** | lagz0ne | 待查 | C3 架构设计方法论（Context-Container-Component） | 架构即代码，LLM 可导航的架构模型 |
| **skill-creator** | daymade | 高 | Skill 开发完整方法论 | Anthropic 官方最佳实践 + 社区经验 |
| **migrate-to-codex** | OpenAI | 高 | Claude Code → Codex 配置迁移 | OpenAI 官方迁移指南 |
| **prompt-architect** | ckelsoe | 高 | 27 个研究支持框架的提示工程方法论 | 学术论文 + 研究框架 |
| **长征工程方法论** | 本 Skill | 新建 | 将长征战略映射到工程迁移流程 | 长征历史事件 → 工程隐喻 |

---

## 二、核心方法论对比

### 2.1 阶段模型对比

| Skill | 阶段数 | 阶段命名风格 | 阶段特征 |
|-------|--------|-------------|--------|
| **refactor-architecture** | 8 phases + Pre-Flight | 航空术语（Pre-Flight, Phase 0/1/2/3/4/5） | 渐进式加载，每阶段有明确 skip 条件、验证门、checkpoint tag、回滚指令 |
| **长征工程方法论** | 9 phases | 历史事件隐喻（危机识别→封锁线→遵义→赤水→泸定桥→雪山→草地→分裂→会师→根据地） | 历史叙事驱动，每阶段有"长征事件"+"工程映射"+"关键动作" |
| **skill-evolve** | 5 步循环 | 抽象动词（冷启动→观察→提炼→改写→验证） | 循环迭代，无固定终点，每轮产出保存在 evolve/ 目录 |
| **c3-skill** | 3 Acts | 戏剧术语（Act 1: Shape/Freeze, Act 2: Change-units, Act 3: Raise canvas） | 架构冻结 + 变更单元驱动，强调"完整性永不放松" |
| **skill-creator** | 9 Phases | 数字编号（Phase 1-9） | 从 0 到 1 创建 skill，包含并行调研、测试迭代、量化对比 |
| **migrate-to-codex** | 6 步迁移 | 技术动词（Scan→Plan→Doctor→Dry-run→Migrate→Validate） | 工具链驱动，强调 self-healing loop |

**关键差异**：
- **refactor-architecture** 和 **长征** 都是"线性阶段+检查点"模型，但前者用航空术语，后者用历史隐喻
- **skill-evolve** 是唯一的"循环迭代"模型，没有终点，持续演进
- **c3-skill** 是"冻结-变更-升级"的架构治理模型，强调不可变性

---

### 2.2 决策机制对比

| Skill | 决策风格 | 纠错机制 | 反模式识别 |
|-------|---------|---------|----------|
| **refactor-architecture** | 决策树（Decision Tree）推荐运行/跳过哪些 phase | 每 phase 有 validation gate，失败则回滚 | 7 个 codebase 特征判断是否应该使用本 skill |
| **长征工程方法论** | 历史事件类比（"召开遵义会议"=架构评审） | 定期复盘（每两周一次），允许推翻之前决策 | 4 种"左倾错误"：教条主义、冒险主义、逃跑主义、分裂主义 |
| **skill-evolve** | 观察驱动（跑真实 prompt → 记录 → 提炼模式） | 每轮验证判断是否收敛，不收敛继续迭代 | 无显式反模式，但通过"竞争旧 skill 删除"避免干扰 |
| **c3-skill** | 变更单元（change-unit）原子提交，destruction gate 自动拒绝 | eval 检查事实 vs 外部状态一致性，drift 检测 | 冻结事实禁止手工编辑，变更必须通过 change-unit |
| **skill-creator** | 8 渠道 Prior Art 调研 + Adopt/Extend/Build 决策矩阵 | 量化迭代对比（工具调用次数、Token 消耗、错误数） | 删除竞争旧 skill、脱敏按目的地不按内容 |
| **migrate-to-codex** | CLI 工具链驱动（--scan-only → --plan → --doctor → --dry-run） | Self-Healing Loop：运行→检查→修复→重运行 | 保留无关现有配置，不编辑源文件 |

**关键差异**：
- **长征** 是唯一使用"历史政治隐喻"来命名反模式的（左倾错误工程化）
- **c3-skill** 是唯一使用"机器门控"（destruction gate）自动拒绝危险操作的
- **skill-creator** 强调"量化指标"（工具调用次数、Token 消耗）来评估迭代效果

---

### 2.3 团队/组织维度对比

| Skill | 团队角色定义 | 协作模式 | 知识沉淀 |
|-------|-------------|---------|--------|
| **refactor-architecture** | 3 种团队规模（solo / active team / large team）对应不同分支策略 | 渐进式加载，每阶段只加载当前 phase 到上下文 | references/ 目录：audits.md, pitfalls.md, team-coordination.md, db-migrations.md |
| **长征工程方法论** | "突击队"（2-3 人攻坚核心模块）、"牵制部队"（维护旧系统） | "大会师"=多团队协作验收，"休整窗口"=团队恢复 | history.md 完整历史参考 + 3 个快速模板（遵义会议/泸定桥/长征总结） |
| **skill-evolve** | 无显式角色定义 | 单人循环：观察→提炼→改写→验证 | evolve/ 目录：evolution-log.md + round-N/ 观察记录 |
| **c3-skill** | 无显式角色，强调"Agent 可导航" | 变更单元原子提交，多人协作通过 change-unit 串行化 | .c3/ 目录：sealed markdown tree，Git 可 review |
| **skill-creator** | 3+ 个研究 agent 并行（工具调研/API 调研/约束调研） | Agent Team 并行调研，独立验证 | references/ 目录：skill-development-methodology.md + 实战案例库 |
| **migrate-to-codex** | 无显式角色 | 自动化迁移，人工确认环节 | .codex/migrate-to-codex-report.txt 迁移报告 |

**关键差异**：
- **长征** 是唯一明确定义"团队角色"（突击队、牵制部队）和"团队休整"概念的方法论
- **skill-creator** 是唯一引入"多 Agent 并行调研"协作模式的
- **refactor-architecture** 根据团队规模提供不同分支策略，非常实用

---

## 三、独特优势对比

### 3.1 各 Skill 的独有特性

| Skill | 独有特性 | 价值 |
|-------|---------|------|
| **refactor-architecture** | **渐进式上下文加载**（每 phase 只加载当前阶段到上下文，避免 token 爆炸） | 大型项目重构时，AI 上下文不会溢出 |
| | **明确的 skip 条件**（每 phase 都有"何时跳过"的判断标准） | 避免过度工程，按需执行 |
| | **Rollback 指令**（每 phase 都有回滚方案） | 安全网，降低重构风险 |
| **长征工程方法论** | **历史叙事驱动**（用长征故事作为隐喻框架） | 极强的记忆性和传播性，团队容易理解和记住 |
| | **反模式政治隐喻**（左倾错误工程化） | 将技术问题与历史教训绑定，增强警示效果 |
| | **生存优先哲学**（系统能跑 > 架构优雅） | 直面工程现实，避免技术自嗨 |
| | **团队休整概念**（大会师后必须休整） | 关注团队可持续性，防止 burnout |
| **skill-evolve** | **知识自增殖**（Bootstrap：每轮笔记是下一轮的燃料） | 无需人工持续投入，skill 自我进化 |
| | **压缩记忆**（evolution-log.md 跨轮次索引） | 长周期改进项目不会丢失上下文 |
| | **从 1 到 N**（专门改进现有 skill，而非创建） | 填补 skill-creator 的空白 |
| **c3-skill** | **架构冻结**（facts 一旦冻结，禁止手工编辑） | 保证架构文档与代码的一致性 |
| | **Destruction Gate**（自动拒绝会导致孤儿或悬空引用的退休操作） | 机器保证图结构完整性 |
| | **Eval 绑定**（.c3/eval/*.yaml 绑定事实到代码/文件/命令） | 架构文档可自动验证是否 drift |
| | **Sweep 反向图遍历**（"改 payments 会 break 什么？"） | 影响分析自动化 |
| **skill-creator** | **并行调研模式**（3+ agent 同时跑不同方向） | 5-20 分钟获得串行需要 3+ 小时的信息 |
| | **量化迭代对比**（工具调用次数、Token 消耗、错误数） | 用数据证明 skill 改进效果 |
| | **实战案例库**（每条规则背后的事故） | 规则不是凭空制定，而是有真实事故支撑 |
| **migrate-to-codex** | **Self-Healing Loop**（自动运行→检查→修复→重运行） | 迁移过程自动化，减少人工干预 |
| | **多目标支持**（global ~/.claude/ + project ./.claude/） | 同时处理个人配置和项目配置 |
| | **状态报告**（Added / Check before using / Not Added） | 清晰的迁移结果分类 |
| **prompt-architect** | **27 个研究支持框架**（CO-STAR, RISEN, ReAct, Tree of Thought 等） | 覆盖几乎所有提示工程场景 |
| | **质量评分系统**（5 维度：Clarity/Specificity/Context/Completeness/Structure） | 可量化的提示词质量评估 |
| | **框架选择决策树**（交互式选择最佳框架） | 降低用户选择成本 |

---

## 四、可借鉴的改进点

### 4.1 长征工程方法论可以向其他 Skill 学习什么

| 来源 Skill | 可借鉴点 | 具体改进建议 |
|-----------|---------|-------------|
| **refactor-architecture** | 渐进式上下文加载 | 将 9 phases 改为"每阶段只加载当前 phase 到上下文"，避免一次性加载全部历史叙事导致 token 溢出 |
| | 明确的 skip 条件 | 为每个 phase 增加"何时跳过本阶段"的判断标准（如 Phase 3 如果旧系统无核心 Bug 可跳过） |
| | Rollback 指令 | 为每个 phase 增加"如果本阶段失败，如何回滚"的具体操作指南 |
| **skill-evolve** | 知识自增殖 | 增加"使用本 skill 后的经验自动沉淀"机制，每次项目结束后自动更新 history.md 的案例 |
| | 压缩记忆 | 增加 evolution-log.md 机制，记录每次使用本 skill 的观察、模式、改进点 |
| **c3-skill** | 架构冻结 + Eval 绑定 | 将"根据地建设"阶段的监控/文档要求，绑定到具体的代码文件和检查命令（如 `grep -r "TODO"` 检查文档完整性） |
| | Destruction Gate | 在"分裂风险"阶段，增加自动检查：如果某技术路线会导致核心数据丢失，自动拒绝 |
| **skill-creator** | 并行调研模式 | 在"突围准备"阶段，引入 3+ agent 并行调研（工具/API/约束），而非串行 |
| | 量化迭代对比 | 为每个 phase 定义量化指标（如 Phase 4 遵义会议：会议时长、决策覆盖率、执行偏差率） |
| | 实战案例库 | 在 history.md 中增加"每条规则背后的真实工程事故"案例 |
| **migrate-to-codex** | Self-Healing Loop | 在迁移执行中增加自动检查→修复→重运行的循环，而非一次性执行 |
| | 状态报告 | 为每个 phase 增加完成状态标记（Completed / Check before using / Skipped） |
| **prompt-architect** | 质量评分系统 | 为"长征总结"模板增加 5 维度评分（战略清晰度/执行具体性/团队上下文/方案完整性/文档结构） |
| | 框架选择决策树 | 为"转移目标选择"（重构/迁移/重写）增加决策树，帮助用户快速判断 |

---

## 五、各 Skill 的适用场景矩阵

| 场景 | 推荐 Skill | 不推荐 | 理由 |
|------|-----------|--------|------|
| 大型 TS/React 单体重构 | refactor-architecture | 长征 | 前者有具体的代码级操作指南和 AST 感知 |
| 跨技术栈迁移（PHP→Go） | 长征工程方法论 | refactor-architecture | 后者只针对 TS/JS 生态 |
| 团队士气低落、项目濒临失败 | 长征工程方法论 | refactor-architecture | 前者的历史叙事和生存哲学有极强的激励作用 |
| Skill 持续改进 | skill-evolve | 其他所有 | 专门设计用于改进 skill 本身 |
| 架构文档治理 | c3-skill | 其他所有 | 唯一提供"架构即代码"和机器验证的 |
| 创建新 Skill | skill-creator | skill-evolve | 前者从 0 到 1，后者从 1 到 N |
| 提示词优化 | prompt-architect | 其他所有 | 27 个框架覆盖最全 |
| 工具链迁移 | migrate-to-codex | 其他所有 | OpenAI 官方，有 CLI 工具链 |
| 多团队协作的大型重构 | 长征 + refactor-architecture 组合 | 单一使用 | 前者提供团队管理和士气框架，后者提供技术执行细节 |

---

## 六、一句话定位

| Skill | 一句话定位 |
|-------|----------|
| **refactor-architecture** | "大型 TS/React 代码库的安全重构手术刀" |
| **长征工程方法论** | "在极端约束下活着到达目的地的战略迁移哲学" |
| **skill-evolve** | "让 skill 自己进化，人工投入递减的知识自增殖系统" |
| **c3-skill** | "LLM 可导航、机器可验证的架构治理基础设施" |
| **skill-creator** | "从 0 到 1 创建高质量 skill 的工业化生产线" |
| **migrate-to-codex** | "Claude Code 到 Codex 的自动化迁移管道" |
| **prompt-architect** | "27 个研究框架驱动的提示词质量提升引擎" |

---

## 七、结论

**长征工程方法论**在 GitHub 同类 Skill 中的独特定位：

1. **唯一使用历史叙事隐喻**的方法论，记忆性和传播性远超技术术语命名
2. **唯一关注"团队士气"和"可持续性"**（休整窗口、生存优先），其他 Skill 都是纯技术视角
3. **唯一将"反模式"与历史政治隐喻绑定**（左倾错误工程化），警示效果更强
4. **在"跨技术栈迁移"和"团队濒临失败"场景下**，是最佳选择

**建议的改进方向**：
- 引入 refactor-architecture 的"渐进式上下文加载"和"skip 条件"
- 引入 skill-creator 的"量化迭代对比"和"实战案例库"
- 引入 c3-skill 的"eval 绑定"和"destruction gate"概念
- 引入 skill-evolve 的"evolution-log"机制，让方法论自我进化

---

> **最终判断**：长征工程方法论不是"又一个重构 skill"，而是填补了"大型工程迁移中团队管理、士气维持、战略决策"这一空白维度。它与技术执行类 Skill（refactor-architecture、c3-skill）是互补关系，而非竞争关系。
