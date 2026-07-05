# Skill: 长征工程方法论 (Long March Engineering Methodology)

## 基本信息

- **Skill ID**: `long-march-engineering`
- **版本**: 1.3.0
- **作者**: AI Assistant
- **适用场景**: 大规模系统重构、技术栈迁移、架构升级、技术债务治理
- **不适用场景**: 代码库 < 10k LOC（手动重构更快）、纯前端 UI 重构（无数据迁移）、无团队共识的"技术自嗨"项目
- **技术栈适配**: PHP 8.x / Hyperf 3.2 / Swoole / Kafka / MySQL / Redis（示例仅为参考，非生产代码）
- **触发词**: `/lmr`, `/long-march-review`, `/迁移方法论`

---

## 快速开始（30 秒上手）

### 方式一：按当前问题触发（推荐）

直接描述你当前遇到的问题，AI 会自动加载对应文件：

| 你说 | AI 加载 |
|------|---------|
| "旧系统太烂了，要不要重构？" | `SKILL.md` + `phase-01-crisis-assessment.md` |
| "准备迁移，需要数据双写方案" | `SKILL.md` + `phase-02-preparation.md` |
| "迁移出问题了，数据不一致" | `SKILL.md` + `phase-03-initial-migration.md` |
| "需要开架构评审会" | `SKILL.md` + `phase-04-milestone-review.md` + `templates/milestone-review.md` |
| "要做灰度发布" | `SKILL.md` + `phase-05-agile-iteration.md` |
| "性能瓶颈急需解决" | `SKILL.md` + `phase-06-core-bottleneck.md` + `templates/core-bottleneck.md` |
| "技术方案争论不休" | `SKILL.md` + `phase-07-tech-dispute.md` |
| "准备验收交付" | `SKILL.md` + `phase-08-delivery.md` + `templates/migration-retrospective.md` |
| "上线后需要监控" | `SKILL.md` + `phase-09-ops-governance.md` + `monitoring-setup.md` |
| "没有监控，怎么获取 P95？" | `SKILL.md` + `monitoring-setup.md` |

### 方式二：按 Phase 触发

如果你知道当前处于哪个 Phase：

```
/lmr phase 1    # 危机评估
/lmr phase 4    # 架构评审（自动加载模板）
/lmr phase 6    # 核心攻坚（自动加载模板）
```

### 方式三：查看示例

```
/lmr example    # 查看所有示例场景索引
```

---

## 核心哲学

> **生存优先**：系统能跑 > 架构优雅。先止血，再治病。没有"完美迁移"，只有"活着到达目的地的迁移"。
> 
> **路线灵活性**：没有唯一正确的架构，只有适应当下约束的架构。计划是用来调整的，不是用来执行的。
> 
> **团队可持续性**：迁移是马拉松，不是冲刺。每完成一个里程碑，必须给团队技术债务清理 Sprint。

---

## 适用性自检（使用本 Skill 前必须回答）

以下 4 个问题必须全部回答"是"（第 3 题根据迁移规模动态评估）：

1. [ ] 旧系统是否已出现**无法局部修复**的系统性问题？（如：框架 EOL、性能瓶颈触及天花板）
2. [ ] 团队是否已达成**"必须迁移"的共识**？（非某一个人的技术理想）
3. [ ] 根据迁移规模，人力资源是否充足？
   - **微型迁移**（1 人 1 周，1 个接口/模块）：**1 人即可**
   - **小型迁移**（2 人 2-4 周，1 个模块）：**至少 2 人**
   - **中型迁移**（3-5 人 1-3 个月，2-3 个模块）：**至少 3 人**
   - **大型迁移**（5+ 人 6-12 个月，全量系统）：**至少 5 人**
   - > 如果不确定规模，先按"小型迁移"评估（至少 2 人）
4. [ ] 业务方是否**知情并同意**迁移可能带来的阶段性影响？（形式：邮件/会议纪要/IM 记录均可，非必须正式签字）

**如果任一问题回答"否"**：
- 第 1-2 题或第 4 题回答"否" → **不要使用本 Skill**。先解决共识问题，或采用局部优化方案。
- 第 3 题回答"否"（人力资源不足） → **缩小迁移规模**（如：从"中型迁移"降级为"小型迁移"或"微型迁移"），或**暂缓迁移**，先补充人力。

---

## Phase 状态机

### 正常流程

```
Phase 1: 危机评估与转移决策
    ↓ [通过标准达成]
Phase 2: 突围准备与暗线部署
    ↓ [通过标准达成]
Phase 3: 初期迁移与损失控制
    ↓ [通过标准达成]
Phase 4: 里程碑复盘与路线修正
    ↓ [通过标准达成]
Phase 5: 敏捷迭代与方案切换
    ↓ [通过标准达成]
Phase 6: 核心瓶颈限时攻坚
    ↓ [通过标准达成]
Phase 7: 技术分歧快速裁决
    ↓ [通过标准达成]
Phase 8: 多团队协作验收
    ↓ [通过标准达成]
Phase 9: 运维治理与资产沉淀
    ↓ [完成]
项目结束
```

### 失败回溯规则

| 当前 Phase | 失败触发条件 | 回溯目标 | 说明 |
|-----------|-------------|---------|------|
| Phase 2 | PoC 失败 / 数据双写导致旧系统性能下降 > 阈值 | Phase 1 | 重新评估技术栈可行性或选择局部优化 |
| Phase 3 | 核心数据丢失 / 新系统故障率过高 / 团队 burnout | Phase 2 | 修正暗线部署方案，或暂停迁移恢复团队 |
| Phase 4 | 会议无法达成共识 | Phase 3 | 收集更多数据后再次复盘 |
| Phase 5 | 灰度阶段错误率过高 / 客户投诉上升 | Phase 4 | 紧急复盘，决定是否回滚或调整方案 |
| Phase 6 | Deadline 到期未解决 / 引入新 Bug | Phase 5 | 回滚到攻坚前版本，重新评估方案 |
| Phase 7 | 决策后团队分裂 / 决策导致核心数据受损 | Phase 4 | 重新复盘，更换决策人或调整决策机制 |
| Phase 8 | 核心模块未通过验收 | Phase 6 | 重新攻坚，或缩小交付范围 |
| Phase 9 | 监控缺失导致故障发现延迟 | Phase 8 | 延期交付，补全监控后重新验收 |

### 跳过规则

| 当前 Phase | Skip 条件 | 直接进入 |
|-----------|----------|----------|
| Phase 1 | 旧系统已完全不可用 | Phase 3（紧急重写） |
| Phase 1 | 团队已在前一项目中使用过本 Skill 且问题根因已明确 | Phase 2（复用结论） |
| Phase 2 | 采用"重写"策略（无旧系统可双写） | Phase 3（跳过数据双写） |
| Phase 2 | 模块无外部依赖 | Phase 3（跳过"与依赖方谈判"） |
| Phase 3 | 采用"重写"策略 | Phase 3（无灰度，直接全量上线核心链路） |
| Phase 3 | 模块无数据一致性要求 | Phase 4（跳过数据一致性检查） |
| Phase 4 | 前 3 个 Phase 数据指标全部达标 | Phase 5（无异常，无需复盘） |
| Phase 4 | 团队已在前一项目中使用过本 Skill 且决策机制已明确 | Phase 5（跳过"确立决策机制"） |
| Phase 5 | 模块无用户可见影响 | Phase 6（跳过 AB 测试，仅做灰度） |
| Phase 5 | 技术方案已在前一项目中验证过 | Phase 6（跳过"方案验证"，直接灰度） |
| Phase 6 | 前 5 个 Phase 无核心瓶颈 | Phase 7（性能指标全部达标） |
| Phase 6 | 瓶颈可通过扩容解决 | Phase 7（优先扩容，不攻坚） |
| Phase 7 | 分歧在 2 周内通过数据验证解决 | Phase 8（无需裁决） |
| Phase 7 | 分歧仅涉及非核心模块 | Phase 8（由模块负责人直接决定） |
| Phase 8 | 单人项目 | Phase 9（无需"多团队协作验收"，但需"自我验收清单"） |
| Phase 9 | 项目为内部工具，无合规要求 | Phase 9（跳过"合规审计"） |
| Phase 9 | 系统流量极低（< 100 QPS） | Phase 9（跳过"容量规划"和"故障演练"） |

### 多轮迁移循环

如果项目需要分多轮迁移（如：先支付模块，再订单模块，再用户模块）：

```
第一轮: Phase 1 → Phase 2 → ... → Phase 9
    ↓ [技术债务清理 Sprint: 2 周]
第二轮: Phase 1(简化) → Phase 2 → ... → Phase 9
    ↓ [技术债务清理 Sprint: 2 周]
第三轮: Phase 1(简化) → Phase 2 → ... → Phase 9
```

**每轮简化规则**：
- Phase 1 可简化：复用上一轮的问题根因分析，仅更新数据
- Phase 2 可简化：复用上一轮的技术栈和暗线部署方案
- 其他 Phase 不可简化，必须完整执行

**疲劳检测**：
- 第 3 轮开始前，团队压力评分若 < 历史 P95 × 0.8（即低于历史平均水平的 80%），暂停迁移
- 暂停期间：增加人员（替补机制）或延长期限（每轮间隔从 2 周延长到 4 周）

---

## 九阶段技术决策流程

| Phase | 名称 | 核心目标 | 文件 |
|-------|------|---------|------|
| Phase 1 | 危机评估与转移决策 | 确认迁移必要性，选择迁移策略 | `phase-01-crisis-assessment.md` |
| Phase 2 | 突围准备与暗线部署 | 技术预研、数据双写、兼容层、影子流量 | `phase-02-preparation.md` |
| Phase 3 | 初期迁移与损失控制 | 首批模块迁移，接受可控损失 | `phase-03-initial-migration.md` |
| Phase 4 | 里程碑复盘与路线修正 | 数据驱动的架构评审，修正路线 | `phase-04-milestone-review.md` |
| Phase 5 | 敏捷迭代与方案切换 | 灰度发布、AB 测试、快速切换 | `phase-05-agile-iteration.md` |
| Phase 6 | 核心瓶颈限时攻坚 | 集中最强资源，限时解决核心问题 | `phase-06-core-bottleneck.md` |
| Phase 7 | 技术分歧快速裁决 | 技术争论超期时，决策人拍板 | `phase-07-tech-dispute.md` |
| Phase 8 | 多团队协作验收 | 定义验收标准，完成交付 | `phase-08-delivery.md` |
| Phase 9 | 运维治理与资产沉淀 | 监控、告警、文档、合规审计 | `phase-09-ops-governance.md` |

---

## 反模式（技术术语定义）

### 1. Context-Free Architecture Adoption（无上下文架构采纳）
**定义**：照搬大厂架构，不考虑团队规模、维护能力和业务复杂度。
**检测**：团队人数/微服务数量 < 2？运维成本/开发成本 > 30%？
**修复**：单体优先，按需拆分。

### 2. Big-Bang Deployment（大爆炸部署）
**定义**：不做灰度，直接全量上线，赌"不会出问题"。
**检测**：是否有灰度配置？是否有回滚脚本？
**修复**：强制灰度 10%→30%→50%→100%，回滚演练。

### 3. Rewrite-First Syndrome（重写优先综合征）
**定义**：遇到问题不分析根因，直接喊"重写"。
**检测**：是否有 RCA 报告？重写决策是否基于数据？
**修复**：强制 RCA，渐进式重写，知识迁移。

### 4. Analysis Paralysis（分析瘫痪）
**定义**：技术争论超过 2 周或 3 次会议无结论。
**检测**：争论时长？是否有决策人？
**修复**：2 周后强制决策，记录反对意见，数据验证优先。

---

## 与已有 Skill 的兼容性声明

本 Skill 为**补充性 Skill**，不覆盖其他 Skill 的核心规则。冲突时以**具体场景 Skill** 为准。

| 已有 Skill | 关系 | 冲突解决规则 |
|-----------|------|-------------|
| PHP 代码审查 | 互补 | 代码质量以 PHP Skill 为准；迁移策略以本 Skill 为准 |
| Kafka DLQ | 依赖 | 本 Skill Phase 2 的数据双写依赖 Kafka Skill 的 DLQ 机制 |
| 支付对账 | 依赖 | 本 Skill Phase 9 的审计日志依赖支付对账的规范 |
| AI 技能管理 | 无关 | 无冲突 |

**术语映射**：
- 本 Skill "Rewrite-First Syndrome" = PHP Skill "盲目重构"
- 本 Skill "Big-Bang Deployment" = PHP Skill "无灰度上线"
- 本 Skill "Analysis Paralysis" = PHP Skill "过度设计争论"

**加载优先级**：本 Skill 为补充性 Skill，不覆盖其他 Skill 的核心规则。当用户同时触发多个 Skill 时，以**具体场景**判断：
- 代码审查场景 → PHP 代码审查 Skill 优先
- 迁移策略场景 → 本 Skill 优先
- 消息队列场景 → Kafka DLQ Skill 优先

---

## 示例场景索引

| 示例 | 场景 | 文件 |
|------|------|------|
| 示例 1 | Hyperf 2.2 → 3.2 升级（技术栈迁移） | `examples/hyperf-upgrade.md` |
| 示例 2 | MySQL 5.7 → 8.0 升级（数据库迁移） | `examples/mysql-upgrade.md` |
| 示例 3 | 单体支付系统 → 模块化（架构重构） | `examples/payment-modularization.md` |

---



---

## 最小可行迁移（MVM）方案

> 本 Skill 的 9 个 Phase 适用于大型迁移项目（6-12 个月，3-5 人团队）。对于小型迁移，可以使用以下简化方案。

### 微型迁移：1 人 1 周，1 个接口/模块

**适用场景**：
- 迁移 1 个 API 接口（如：用户资料查询从旧框架到新框架）
- 升级 1 个依赖库（如：Redis 客户端从 3.0 升级到 6.0）
- 优化 1 个数据库查询（如：添加索引，迁移到读写分离）

**简化 Phase 路径**：

```
Phase 1(简化) → Phase 3(无灰度) → Phase 8(自我验收)
```

**Phase 1 简化**：
- 不需要完整的根因分析报告
- 只需要回答："为什么需要迁移？"（1 句话）+ "迁移范围是什么？"（1 个接口/模块）
- 不需要业务方书面确认，只需要口头确认

**Phase 3 简化（无灰度）**：
- 直接切换（因为只影响 1 个接口，回滚成本低）
- 不需要数据双写（如果无数据变更）
- 不需要接口兼容层（如果调用方只有 1 个）
- 必须准备回滚脚本（1 分钟回滚）

**Phase 8 简化（自我验收）**：
- 不需要多团队确认
- 只需要自己验证：功能正常、性能达标、可回滚
- 验收清单（3 项）：
  1. [ ] 功能测试通过（手动测试 3 个场景）
  2. [ ] 性能对比（新系统 P95 < 旧系统 P95 × 1.2）
  3. [ ] 回滚演练（1 分钟内回滚成功）

**时间分配**：
- 第 1 天：Phase 1（评估 + 准备）
- 第 2-4 天：Phase 3（迁移 + 测试）
- 第 5 天：Phase 8（验收 + 文档）

---

### 小型迁移：2 人 2-4 周，1 个模块

**适用场景**：
- 迁移 1 个业务模块（如：支付核心模块，约 5000 行代码）
- 升级 1 个数据库（如：MySQL 5.7 → 8.0，单库 < 100GB）

**简化 Phase 路径**：

```
Phase 1 → Phase 2(简化) → Phase 3 → Phase 4(简化) → Phase 8
```

**Phase 2 简化**：
- 不需要完整的影子流量（如果模块无用户可见影响）
- 数据双写简化为"数据库 Binlog 同步"或"应用层双写"（2 选 1）
- 接口兼容层简化为"Nginx 路由"或"代码层 if-else 切换"

**Phase 4 简化**：
- 不需要正式的架构评审会议
- 只需要 1 小时的数据回顾（查看 Grafana 面板）
- 如果数据正常，直接进入 Phase 5/8

**时间分配**：
- 第 1 周：Phase 1 + Phase 2（简化）
- 第 2 周：Phase 3（灰度 10%→50%）
- 第 3 周：Phase 3（灰度 50%→100%）+ Phase 4（简化复盘）
- 第 4 周：Phase 8（验收 + 文档）

---

### 中型迁移：3-5 人 1-3 个月，2-3 个模块

**适用场景**：
- 迁移 2-3 个业务模块（如：支付 + 订单模块）
- 技术栈升级（如：Hyperf 2.2 → 3.2，涉及 5 个模块）

**标准 Phase 路径**：

```
Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5 → Phase 8 → Phase 9(简化)
```

**Phase 6-7 跳过条件**：
- 如果没有遇到核心瓶颈（性能指标达标），跳过 Phase 6
- 如果没有技术分歧（团队共识），跳过 Phase 7

**Phase 9 简化**：
- 不需要完整的容量规划和故障演练
- 只需要：监控覆盖核心路径、告警规则配置、基础运维文档

---

### 大型迁移：5+ 人 6-12 个月，全量系统

**适用场景**：
- 全量系统重构（如：单体应用拆分为微服务，涉及 10+ 模块）
- 技术栈全面替换（如：PHP → Go，MySQL → TiDB）

**完整 Phase 路径**：

```
Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5 → Phase 6 → Phase 7 → Phase 8 → Phase 9
```

**多轮迁移**：
- 分 3-5 轮，每轮 2-3 个月
- 每轮之间：技术债务清理 Sprint（2 周）
- 疲劳检测：第 3 轮开始前，团队压力评分若 < 历史 P95 × 0.8，暂停迁移

---

## 技术栈适配指南

> 本 Skill 的方法论适用于任何技术栈，代码示例仅为 PHP/Hyperf 参考。以下提供常见技术栈的映射表。

### 核心概念映射表

| 本 Skill 概念 | PHP/Hyperf | Go/Gin | Java/Spring | Node.js/Express | Python/FastAPI |
|--------------|-----------|--------|-------------|-----------------|----------------|
| **AOP 拦截器** | Hyperf AOP | Gin Middleware | Spring AOP | Express Middleware | FastAPI Dependencies |
| **数据双写** | Kafka 生产者 | NATS / RabbitMQ | Kafka / RabbitMQ | Redis Pub/Sub | Celery + Redis |
| **接口兼容层** | Nginx + 代码路由 | Traefik + 代码 | Spring Cloud Gateway | Nginx + 代码 | Nginx + 代码 |
| **灰度发布** | Nginx + 代码 | Traefik + 代码 | Spring Cloud Gateway | Nginx + 代码 | Nginx + 代码 |
| **影子流量** | Kafka 生产者 | NATS / RabbitMQ | Kafka / RabbitMQ | Redis Pub/Sub | Celery + Redis |
| **监控采集** | Prometheus Metric | Prometheus Client | Micrometer | prom-client | prometheus-client |
| **日志标准化** | Monolog JSON | zap / logrus | SLF4J + Logback | winston | structlog |
| **配置中心** | Nacos / Apollo | etcd / Consul | Spring Cloud Config | etcd / Consul | Consul |
| **数据库连接池** | Hyperf DB Pool | sql.DB | HikariCP | pg-pool | SQLAlchemy Pool |
| **缓存连接池** | Hyperf Redis Pool | go-redis Pool | Lettuce Pool | ioredis Pool | redis-py Pool |

### 数据双写：各技术栈实现思路

**通用原则**：
1. 旧系统写入主数据库后，异步发送消息到消息队列
2. 新系统消费消息队列，写入新数据库
3. 消息队列必须支持至少一次投递（at-least-once）
4. 新系统必须实现幂等性（防止重复消费）

**PHP/Hyperf**：
- 使用 Hyperf AOP 拦截写入操作
- 通过 Kafka 生产者异步发送
- 新系统通过 Kafka 消费者异步消费

**Go/Gin**：
- 使用 Gin Middleware 拦截请求
- 通过 goroutine 异步发送消息到 NATS/RabbitMQ
- 新系统通过 goroutine 消费消息

**Java/Spring**：
- 使用 Spring AOP 或 TransactionSynchronization 拦截写入
- 通过 KafkaTemplate 异步发送
- 新系统通过 @KafkaListener 消费

**Node.js/Express**：
- 使用 Express Middleware 拦截请求
- 通过 Redis Pub/Sub 或 RabbitMQ 异步发送
- 新系统通过 worker 进程消费

**Python/FastAPI**：
- 使用 FastAPI Dependencies 或 BackgroundTasks 拦截写入
- 通过 Celery + Redis 异步发送任务
- 新系统通过 Celery Worker 消费

### 灰度发布：各技术栈实现思路

**通用原则**：
1. 根据用户 ID 或请求特征分配流量比例
2. 支持动态调整比例（无需重启服务）
3. 支持白名单（内部员工、VIP 用户强制走特定版本）
4. 支持 fallback（新版本失败时自动回退到旧版本）

**PHP/Hyperf**：
- Nginx 层根据 Cookie/Header 路由
- 或代码层根据用户 ID 哈希判断版本
- 配置存储在 Nacos/Apollo，热更新

**Go/Gin**：
- Traefik 根据 Header/Path 路由
- 或代码层根据用户 ID 哈希判断版本
- 配置存储在 etcd/Consul，热更新

**Java/Spring**：
- Spring Cloud Gateway 根据 Header 路由
- 或代码层根据用户 ID 哈希判断版本
- 配置存储在 Spring Cloud Config，热更新

**Node.js/Express**：
- Nginx 根据 Cookie/Header 路由
- 或代码层根据用户 ID 哈希判断版本
- 配置存储在 Redis/Consul，热更新

**Python/FastAPI**：
- Nginx 根据 Cookie/Header 路由
- 或代码层根据用户 ID 哈希判断版本
- 配置存储在 Redis/Consul，热更新

### 监控体系建设：各技术栈推荐方案

| 阶段 | PHP/Hyperf | Go/Gin | Java/Spring | Node.js/Express | Python/FastAPI |
|------|-----------|--------|-------------|-----------------|----------------|
| **阶段 1: 基础日志** | Monolog JSON | zap JSON | SLF4J JSON | winston JSON | structlog JSON |
| **阶段 2: 指标采集** | Prometheus | Prometheus | Micrometer + Prometheus | prom-client | prometheus-client |
| **阶段 3: 可视化** | Grafana | Grafana | Grafana | Grafana | Grafana |
| **阶段 4: 业务指标** | Hyperf Metric | Prometheus Counter | Micrometer Counter | prom-client Counter | prometheus Counter |
| **轻量级方案** | 阿里云 SLS | 阿里云 SLS | 阿里云 SLS | 阿里云 SLS | 阿里云 SLS |
| **托管方案** | 腾讯云 Prometheus | 腾讯云 Prometheus | 腾讯云 Prometheus | 腾讯云 Prometheus | 腾讯云 Prometheus |

### 阈值校准：各技术栈通用方法

无论使用什么技术栈，阈值校准方法相同：

1. **收集过去 3 个月的监控数据**（通过日志分析或指标系统）
2. **计算每个指标的 P95**（第 95 百分位数）
3. **设定阈值 = 历史 P95 × 系数**（系数根据业务容忍度调整）
4. **每周一重新校准**（导出过去 7 天数据，更新 P95）
5. **每月回顾趋势**（阈值在收紧还是放宽？）

**无监控时的 Fallback**：
- 使用行业经验值作为临时阈值（见 [Phase 1 阈值校准](#阈值校准)）
- 同时启动阶段 1 监控建设（1 周内完成基础日志）
- 监控建设完成后，立即替换为历史 P95

---

## 版本记录

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2026-07-03 | 初始版本 |
| 1.1.0 | 2026-07-03 | 增加技术实现示例、通过/失败标准、合规审计 |
| 1.2.0 | 2026-07-03 | 拆分文件按需加载、修复代码示例（伪代码+免责声明）、阈值动态校准 |
| 1.3.0 | 2026-07-03 | 增加文件加载路由表、Phase 状态机、多轮迁移循环、示例场景索引、监控建设指南 |

---

> **在工程迁移中，没有完美的路线图，只有活着到达目的地的队伍。**
