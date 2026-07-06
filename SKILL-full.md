# Skill: 长征工程方法论 (Long March Engineering Methodology) — 单文件合并版

> **版本**: 1.3.0-full
> **说明**: 本文件为单文件合并版，包含所有 Phase、模板、示例、监控指南和参考文档。适用于不支持多文件 Skill 的系统。
> **推荐**: 如 SkillHub 支持多文件，请使用分文件版以获得更好的按需加载体验。

---

## 目录

- [一、快速开始](#一快速开始)
- [二、核心哲学](#二核心哲学)
- [三、适用性自检](#三适用性自检)
- [四、Phase 状态机](#四phase-状态机)
- [五、九阶段技术决策流程](#五九阶段技术决策流程)
  - [Phase 1: 危机评估与转移决策](#phase-1-危机评估与转移决策)
  - [Phase 2: 突围准备与暗线部署](#phase-2-突围准备与暗线部署)
  - [Phase 3: 初期迁移与损失控制](#phase-3-初期迁移与损失控制)
  - [Phase 4: 里程碑复盘与路线修正](#phase-4-里程碑复盘与路线修正)
  - [Phase 5: 敏捷迭代与方案切换](#phase-5-敏捷迭代与方案切换)
  - [Phase 6: 核心瓶颈限时攻坚](#phase-6-核心瓶颈限时攻坚)
  - [Phase 7: 技术分歧快速裁决](#phase-7-技术分歧快速裁决)
  - [Phase 8: 多团队协作验收](#phase-8-多团队协作验收)
  - [Phase 9: 运维治理与资产沉淀](#phase-9-运维治理与资产沉淀)
- [六、反模式](#六反模式)
- [七、与已有 Skill 兼容性声明](#七与已有-skill-兼容性声明)
- [八、监控体系建设指南](#八监控体系建设指南)
- [九、模板库](#九模板库)
- [十、示例场景](#十示例场景)
- [十一、参考文档：长征历史时间线](#十一参考文档长征历史时间线)
- [十二、版本记录](#十二版本记录)

---

## 一、快速开始

### 方式一：按当前问题触发（推荐）

直接描述你当前遇到的问题，AI 会自动加载对应内容：

| 你说 | 查看章节 |
|------|---------|
| "旧系统太烂了，要不要重构？" | [Phase 1](#phase-1-危机评估与转移决策) |
| "准备迁移，需要数据双写方案" | [Phase 2](#phase-2-突围准备与暗线部署) |
| "迁移出问题了，数据不一致" | [Phase 3](#phase-3-初期迁移与损失控制) |
| "需要开架构评审会" | [Phase 4](#phase-4-里程碑复盘与路线修正) + [模板](#模板一架构评审会议) |
| "要做灰度发布" | [Phase 5](#phase-5-敏捷迭代与方案切换) |
| "性能瓶颈急需解决" | [Phase 6](#phase-6-核心瓶颈限时攻坚) + [模板](#模板二核心攻坚计划) |
| "技术方案争论不休" | [Phase 7](#phase-7-技术分歧快速裁决) |
| "准备验收交付" | [Phase 8](#phase-8-多团队协作验收) + [模板](#模板三迁移复盘报告) |
| "上线后需要监控" | [Phase 9](#phase-9-运维治理与资产沉淀) + [监控指南](#八监控体系建设指南) |
| "没有监控，怎么获取 P95？" | [监控指南](#八监控体系建设指南) |
| "想看完整迁移案例" | [示例场景](#十示例场景) |

### 方式二：按 Phase 触发

```
/lmr phase 1    # 危机评估
/lmr phase 4    # 架构评审（自动加载模板）
/lmr phase 6    # 核心攻坚（自动加载模板）
```

---

## 二、核心哲学

> **生存优先**：系统能跑 > 架构优雅。先止血，再治病。没有"完美迁移"，只有"活着到达目的地的迁移"。
> 
> **路线灵活性**：没有唯一正确的架构，只有适应当下约束的架构。计划是用来调整的，不是用来执行的。
> 
> **团队可持续性**：迁移是马拉松，不是冲刺。每完成一个里程碑，必须给团队技术债务清理 Sprint。

---

## 三、适用性自检

以下 4 个问题必须全部回答"是"：

1. [ ] 旧系统是否已出现**无法局部修复**的系统性问题？（如：框架 EOL、性能瓶颈触及天花板）
2. [ ] 团队是否已达成**"必须迁移"的共识**？（非某一个人的技术理想）
3. [ ] 是否有**至少 2 名核心工程师**可以全职投入迁移？
4. [ ] 业务方是否**知情并同意**迁移可能带来的阶段性影响？（形式：邮件/会议纪要/IM 记录均可）

如果任一问题回答"否"，**不要使用本 Skill**。先解决共识问题，或采用局部优化方案。

---

## 四、Phase 状态机

### 正常流程

```
Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5 → Phase 6 → Phase 7 → Phase 8 → Phase 9
```

### 失败回溯规则

| 当前 Phase | 失败触发条件 | 回溯目标 |
|-----------|-------------|---------|
| Phase 2 | PoC 失败 / 数据双写导致旧系统性能下降 | Phase 1 |
| Phase 3 | 核心数据丢失 / 新系统故障率过高 / 团队 burnout | Phase 2 |
| Phase 4 | 会议无法达成共识 | Phase 3 |
| Phase 5 | 灰度阶段错误率过高 / 客户投诉上升 | Phase 4 |
| Phase 6 | Deadline 到期未解决 / 引入新 Bug | Phase 5 |
| Phase 7 | 决策后团队分裂 / 决策导致核心数据受损 | Phase 4 |
| Phase 8 | 核心模块未通过验收 | Phase 6 |
| Phase 9 | 监控缺失导致故障发现延迟 | Phase 8 |

### 跳过规则

| 当前 Phase | Skip 条件 | 直接进入 |
|-----------|----------|---------|
| Phase 1 | 旧系统已完全不可用 | Phase 3（紧急重写） |
| Phase 1 | 团队已在前一项目中使用过本 Skill | Phase 2（复用结论） |
| Phase 2 | 采用"重写"策略 | Phase 3（跳过数据双写） |
| Phase 2 | 模块无外部依赖 | Phase 3（跳过"与依赖方谈判"） |
| Phase 3 | 采用"重写"策略 | Phase 3（无灰度，直接全量） |
| Phase 3 | 模块无数据一致性要求 | Phase 4（跳过数据一致性检查） |
| Phase 4 | 前 3 个 Phase 数据指标全部达标 | Phase 5（无异常，无需复盘） |
| Phase 5 | 模块无用户可见影响 | Phase 6（跳过 AB 测试） |
| Phase 5 | 技术方案已在前一项目中验证过 | Phase 6（跳过"方案验证"） |
| Phase 6 | 前 5 个 Phase 无核心瓶颈 | Phase 7（性能指标全部达标） |
| Phase 6 | 瓶颈可通过扩容解决 | Phase 7（优先扩容，不攻坚） |
| Phase 7 | 分歧在 2 周内通过数据验证解决 | Phase 8（无需裁决） |
| Phase 7 | 分歧仅涉及非核心模块 | Phase 8（由模块负责人直接决定） |
| Phase 8 | 单人项目 | Phase 9（无需"多团队协作验收"） |
| Phase 9 | 项目为内部工具，无合规要求 | Phase 9（跳过"合规审计"） |
| Phase 9 | 系统流量极低（< 100 QPS） | Phase 9（跳过"容量规划"和"故障演练"） |

### 多轮迁移循环

```
第一轮: Phase 1 → Phase 2 → ... → Phase 9
    ↓ [技术债务清理 Sprint: 2 周]
第二轮: Phase 1(简化) → Phase 2 → ... → Phase 9
    ↓ [技术债务清理 Sprint: 2 周]
第三轮: Phase 1(简化) → Phase 2 → ... → Phase 9
```

**每轮简化规则**：Phase 1 可简化（复用上一轮的问题根因分析），Phase 2 可简化（复用上一轮的技术栈和暗线部署方案），其他 Phase 不可简化。

**疲劳检测**：第 3 轮开始前，团队压力评分若 < 历史 P95 × 0.8，暂停迁移。

---

## 五、九阶段技术决策流程



---

### Phase 01 Crisis Assessment

确认迁移必要性，选择迁移策略（重构 / 迁移 / 重写）。

## 通过标准
- [ ] 旧系统问题根因报告已产出（含监控数据、故障记录、性能瓶颈分析）
- [ ] 迁移策略已选定（见下方决策树）
- [ ] 迁移范围已明确（哪些模块迁移、哪些保留、哪些废弃）
- [ ] 业务方已知情同意（邮件/会议纪要/IM 记录均可）

## 失败标准（触发回滚）
- 旧系统问题可通过局部优化解决（如：加索引、升级 Redis、优化 SQL）
- 团队未达成共识，仅某一个人推动
- 业务方不接受任何影响

## Skip 条件
- 旧系统已完全不可用 → 直接跳到 Phase 3（紧急重写）
- 团队已在前一项目中使用过本 Skill 且问题根因已明确 → 跳过根因分析，复用结论

## Rollback 方案
若评估结论为"无需迁移"，产出《局部优化方案》并归档，项目终止。

---

## 迁移策略决策树

```
旧系统是否还能运行核心流程？
├─ 否（核心流程中断频率 > 历史 P95 值）
│   └─ 重写（Emergency Rewrite）
│       └─ 目标：72 小时内恢复核心流程
│       └─ 范围：仅核心交易链路，非核心功能全部关闭
│       └─ 团队：全员投入，无日常维护
│
└─ 是
    └─ 技术债务是否可局部修复？
        ├─ 是（如：单表数据量 < 1000 万，索引可优化，配置可调）
        │   └─ 局部优化（Refactor-in-Place）
        │       └─ 目标：性能提升 50%，不改架构
        │       └─ 范围：瓶颈模块
        │       └─ 团队：1-2 人，2 周完成
        │
        └─ 否（如：框架 EOL，数据库版本锁死，无法横向扩容）
            └─ 渐进式迁移（Strangler Fig）
                └─ 目标：6-12 个月完成全量迁移
                └─ 范围：按业务域拆分，逐模块迁移
                └─ 团队：3-5 人核心组 + 1-2 人维护旧系统
                └─ 关键：数据双写 + 影子流量 + 灰度切换
```

---

## 根因分析检查清单

```
□ 收集过去 3 个月的故障记录（次数、类型、恢复时间）
□ 收集性能基线（P50/P95/P99 响应时间，取 P95 作为阈值基准）
□ 检查框架/中间件版本是否 EOL（如：Hyperf 2.2 已停止维护）
□ 检查数据库是否触及性能天花板（连接池使用率、慢查询比例）
□ 检查团队维护成本（修复一个 Bug 的平均时间是否逐月上升）
□ 评估局部优化成本 vs 迁移成本（如果局部优化 < 2 周且效果可验证，优先局部优化）
```

---

## 阈值校准方法（动态，非固定值）

所有阈值必须基于**历史数据 P95 值**设定，而非固定数值：

| 指标 | 校准方法 | 示例 |
|------|---------|------|
| 核心流程中断频率 | 取过去 3 个月故障次数的 P95 | 若 P95 = 2 次/周，则阈值设为 2 次/周 |
| 性能瓶颈判定 | 取过去 3 个月 P95 响应时间的 P95 | 若 P95 = 800ms，则阈值设为 800ms |
| 局部优化上限 | 团队历史数据：局部优化项目的平均耗时 | 若历史平均 1.5 周，则超过 2 周考虑迁移 |

**调整规则**：
- 高频交易系统（如游戏内购）：阈值收紧至 P90
- 低频后台系统：阈值放宽至 P99
- 每 2 周重新校准一次（数据漂移）

---

## 伪代码示例：根因分析脚本

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。生产使用前必须经过代码审查、测试和性能评估。

```php
// 伪代码：收集过去 3 个月的性能基线
// 关键注意事项：
// 1. 必须按业务模块分组，避免全局平均掩盖局部问题
// 2. 必须排除促销活动等异常时段数据
// 3. 必须记录 P95 而非平均值（平均值对长尾延迟不敏感）

function collectPerformanceBaseline($days = 90) {
    $metrics = fetchFromPrometheus([
        'metric' => 'http_request_duration_seconds',
        'range' => "{$days}d",
        'aggregation' => 'quantile(0.95)',  // 关键：用 P95，不用平均值
        'group_by' => 'module',              // 关键：按模块分组
    ]);

    // 排除异常时段（如：双 11、周年庆）
    $excludePeriods = getPromotionalPeriods($days);
    $filtered = filterOut($metrics, $excludePeriods);

    return calculateP95PerModule($filtered);
}

// 伪代码：检查框架 EOL
// 关键注意事项：
// 1. 必须检查官方维护状态，而非仅检查版本号
// 2. 必须评估社区活跃度（GitHub issues 响应时间、PR 合并频率）

function checkFrameworkEOL($framework, $currentVersion) {
    $officialStatus = fetchOfficialStatus($framework, $currentVersion);
    $communityHealth = assessCommunityHealth($framework); // GitHub 活跃度

    return $officialStatus === 'EOL' || $communityHealth < 0.3; // 社区健康度 < 30% 视为风险
}
```


---

### Phase 02 Preparation

完成技术预研、数据双写、接口兼容层、影子流量验证。

## 通过标准
- [ ] 新系统技术栈 PoC 已通过（核心接口可运行）
- [ ] 数据双写机制已部署，旧系统写入成功率 100%，新系统写入延迟 < 历史 P95 × 1.2
- [ ] 接口兼容层已部署，旧系统调用方可无缝切换
- [ ] 影子流量已运行 7 天，新系统响应结果与旧系统差异率 < 历史 P95 错误率
- [ ] 与依赖方（支付渠道、第三方 SDK）已达成兼容协议

## 失败标准（触发回滚）
- PoC 无法运行核心业务流程
- 数据双写导致旧系统性能下降 > 历史 P95 × 1.5
- 影子流量差异率 > 历史 P95 错误率 × 2

## Skip 条件
- 采用"重写"策略（Phase 1 决策树）→ 跳过数据双写和影子流量
- 模块无外部依赖 → 跳过"与依赖方谈判"

## Rollback 方案
- PoC 失败 → 产出《技术栈不可行报告》，终止迁移，启动局部优化
- 数据双写导致旧系统性能下降 → 立即关闭双写，回滚到只读同步

---

## 伪代码示例：数据双写

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。关键注意事项必须全部满足后方可使用。

```php
// 伪代码：数据双写（AOP 拦截写入操作）
// 关键注意事项：
// 1. ⚠️ 必须实现幂等性：Kafka 消息可能重复消费，新系统必须去重（如：唯一键或幂等 token）
// 2. ⚠️ 必须异步执行：双写不能阻塞旧系统主流程，失败时记录日志待修复
// 3. ⚠️ 必须监控双写延迟：若延迟 > 历史 P95 × 1.2，触发告警
// 4. ⚠️ 必须处理网络分区：Kafka 不可用时，不能导致旧系统写入失败

function interceptWriteOperation($operation, $args, $result) {
    // 1. 旧系统写入已完成（主流程）
    // 2. 异步双写到新系统
    async(function() use ($operation, $args, $result) {
        try {
            $message = [
                'idempotency_key' => generateUUID(),  // 关键：幂等性 token
                'operation' => $operation,
                'payload' => $args,
                'old_result' => $result,
                'timestamp' => microtime(true),
            ];

            sendToKafka('sync-topic', $message);

        } catch (NetworkException $e) {
            // 关键：双写失败不影响旧系统，记录日志待修复
            logError('Data sync failed', ['error' => $e, 'message' => $message]);
            // 可选：写入本地队列，Kafka 恢复后重试
            writeToLocalQueue($message);
        }
    });

    return $result; // 立即返回，不等待双写完成
}
```

---

## 伪代码示例：接口兼容层

```php
// 伪代码：接口兼容层（版本路由）
// 关键注意事项：
// 1. ⚠️ 必须实现 fallback：v2 失败时自动回退到 v1，不能返回 404/500
// 2. ⚠️ 必须记录路由决策：用于排查问题
// 3. ⚠️ 必须支持动态切换：通过配置中心热更新，无需重启服务

function routeRequest($request) {
    $version = $request->getHeader('X-API-Version') ?: 'v1';

    if ($version === 'v2') {
        try {
            $response = callV2Handler($request);
            logRouteDecision($request, 'v2', 'success');
            return $response;

        } catch (Exception $e) {
            // 关键：fallback 到 v1
            logRouteDecision($request, 'v2', 'fallback_to_v1', ['error' => $e]);
            return callV1Handler($request);
        }
    }

    return callV1Handler($request);
}
```

---

## 伪代码示例：影子流量

```php
// 伪代码：影子流量（异步复制请求到新系统）
// 关键注意事项：
// 1. ⚠️ 必须异步执行：不能阻塞主流程响应
// 2. ⚠️ 必须错误隔离：新系统不可用不能导致消费者崩溃
// 3. ⚠️ 必须结果对比：记录新旧系统响应差异，差异率 > 阈值时告警
// 4. ⚠️ 必须资源控制：限制并发请求数，避免压垮新系统

function replicateShadowTraffic($originalRequest, $originalResponse) {
    async(function() use ($originalRequest, $originalResponse) {
        try {
            $newResponse = callNewSystem($originalRequest);

            $diff = compareResponses($originalResponse, $newResponse);

            if ($diff->rate > getThreshold('shadow_diff_rate')) { // 动态阈值
                alert('Shadow traffic diff exceeded', ['diff' => $diff]);
            }

        } catch (Exception $e) {
            // 关键：错误隔离，记录但不崩溃
            logError('Shadow traffic failed', ['error' => $e]);
            // 可选：发送到死信队列，人工分析
            sendToDLQ($originalRequest, 'shadow-traffic-dlq');
        }
    }, ['concurrency_limit' => 10]); // 关键：限制并发数
}
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 新系统写入延迟 | 旧系统写入延迟的 P95 × 1.2（允许 20% 额外开销） |
| 影子流量差异率 | 旧系统历史错误率的 P95（若旧系统本身有 0.1% 错误，新系统差异率阈值 = 0.1%） |
| 旧系统性能下降 | 旧系统历史响应时间的 P95 × 1.5（超过此值立即关闭双写） |


---

### Phase 03 Initial Migration

首批模块迁移，接受可控损失，保留核心骨干。

## 通过标准
- [ ] 首批迁移模块（建议"非核心但高频"，如：用户资料查询）已上线
- [ ] 旧系统该模块流量已切换 10% 到新系统，无异常
- [ ] 核心交易数据（订单、支付）100% 强一致，无丢失
- [ ] 非核心数据允许延迟 < 历史 P95 × 1.5，丢失率 < 历史错误率 P95
- [ ] 团队已接受"初期损失"心理建设（见下方可控损失清单）

## 失败标准（触发回滚）
- 核心交易数据丢失 > 0（零容忍）
- 新系统故障率 > 旧系统历史故障率 P95 × 2
- 团队核心成员 burnout（通过匿名问卷检测，评分 < 3/5）

## Skip 条件
- 采用"重写"策略 → 此阶段即为"紧急上线核心链路"，无灰度
- 模块无数据一致性要求 → 跳过数据一致性检查

## Rollback 方案
- 流量切换回旧系统（接口兼容层秒级切换）
- 数据修复：对比新旧系统差异，自动修复或人工介入

---

## 可控损失清单（必须与业务方确认）

| 损失类型 | 可接受上限 | 补偿方案 | 确认方式 |
|---------|-----------|---------|---------|
| 非核心报表延迟 | < 历史 P95 × 1.5 | 延迟期间提供缓存数据快照 | 邮件/IM |
| 非核心功能不可用 | < 历史故障时长 P95 | 功能降级页面 + 补偿通知 | 邮件/IM |
| 统计数据丢失 | < 历史错误率 P95 | 事后补录 + 数据平滑 | 邮件/IM |
| 日志丢失 | < 历史错误率 P95 × 2 | 增加日志采样率 | 邮件/IM |
| 核心交易数据丢失 | **0%** | 无补偿，零容忍 | 必须确认 |

**关键**：所有"可接受上限"必须基于历史数据 P95，而非固定数值。

---

## 伪代码示例：数据一致性校验

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。

```php
// 伪代码：数据一致性校验（对比新旧系统）
// 关键注意事项：
// 1. ⚠️ 必须限制时间窗口：只对比最近 24 小时数据，避免全表扫描导致锁表
// 2. ⚠️ 必须分批处理：每批 < 1000 条，避免内存溢出和长时间锁表
// 3. ⚠️ 必须处理并发变更：校验期间数据可能已变更，差异需人工二次确认
// 4. ⚠️ 修复操作必须记录审计日志：谁、何时、修改了什么
// 5. ⚠️ 核心交易数据差异必须立即告警，非核心数据差异可批量修复

function checkDataConsistency($module, $timeWindow = '24h') {
    $diffCount = 0;
    $batchSize = 1000;

    // 关键：只对比最近 24 小时，避免全表扫描
    $startTime = now()->sub($timeWindow);

    foreach (fetchBatches($module, $startTime, $batchSize) as $batch) {
        foreach ($batch as $oldRecord) {
            $newRecord = fetchFromNewSystem($oldRecord->id);

            $diff = compareRecords($oldRecord, $newRecord);

            if ($diff->hasDifference) {
                $diffCount++;

                if ($module === 'payment' || $module === 'order') {
                    // 核心交易数据：立即告警 + 人工确认
                    alertCritical("Core data inconsistency", ['id' => $oldRecord->id, 'diff' => $diff]);
                    // 禁止自动修复，必须人工确认
                    queueForManualReview($oldRecord, $newRecord, $diff);

                } else {
                    // 非核心数据：记录差异，批量修复
                    logDiff($oldRecord->id, $diff);
                    // 修复时记录审计日志
                    auditLog('auto_fix', [
                        'id' => $oldRecord->id,
                        'field' => $diff->field,
                        'old_value' => $diff->old,
                        'new_value' => $diff->new,
                    ]);
                    autoFix($newRecord, $diff);
                }
            }
        }
    }

    return $diffCount;
}
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 非核心数据延迟 | 历史延迟 P95 × 1.5 |
| 非核心数据丢失率 | 历史错误率 P95 |
| 新系统故障率容忍 | 旧系统历史故障率 P95 × 2 |
| 团队压力阈值 | 匿名问卷评分 < 3/5（基于历史评分 P95） |


---

### Phase 04 Milestone Review

数据驱动的架构评审，修正错误决策，确立新的决策机制。

## 通过标准
- [ ] 里程碑数据已收集（见下方数据收集清单）
- [ ] 架构评审会议已召开，时长 2 小时，参会人员 ≥ 5 人
- [ ] 会议产出《路线修正决议》（简化版 ADR），含：原路线、修正后路线、决策人、执行时限
- [ ] 团队对修正后路线达成共识（无反对票，或反对意见已记录并处理）

## 失败标准（触发回滚）
- 会议无数据支撑，纯主观讨论
- 会议无决策人拍板，陷入无限期争论
- 修正后路线未得到业务方确认

## Skip 条件
- 前 3 个 phase 数据指标全部达标（无异常）→ 跳过复盘，直接进入 Phase 5
- 团队已在前一项目中使用过本 Skill 且决策机制已明确 → 跳过"确立决策机制"

## Rollback 方案
- 会议无法达成共识 → 由最高技术负责人拍板，记录反对意见，2 周后再次复盘

---

## 数据收集清单（会议前必须完成）

```
□ 系统故障率（过去 2 周）：旧系统 vs 新系统，对比历史 P95
□ 性能指标（P50/P95/P99）：使用监控工具导出，对比历史 P95
□ 团队反馈（匿名问卷）：进度评分、压力评分、技术方案问题（1-5 分）
□ 业务方反馈：功能降级影响、客户投诉数量变化、收入影响
□ 数据一致性：差异数量、修复数量、未修复数量
□ 资源消耗：迁移团队工时、旧系统维护工时、新增基础设施成本
```

---

## 会议议程（2 小时）

| 时段 | 内容 | 负责人 | 产出 |
|------|------|--------|------|
| 0-30min | 数据回顾 | 记录者 | 数据汇总表 |
| 30-60min | 错误识别 | 全员 | 错误清单 |
| 60-105min | 路线修正 | 决策者 | 修正决议 |
| 105-120min | 决策确认 | 决策者 | 执行计划 + 下次复盘时间 |

**参会角色**：
- 决策者（1 人）：有最终技术决策权
- 记录者（1 人）：负责会议记录和 ADR 产出
- 反对者（1 人）：专门挑刺，必须提出至少 1 个反对意见
- 执行者（2-3 人）：负责后续执行

---

## 简化版 ADR 模板

```markdown
# ADR-XXX: [标题]

## 背景
[1-2 段，说明迁移项目背景和当前问题]

## 原路线
[之前选择的技术路线，含决策依据]

## 问题
[数据证明原路线存在的问题，必须引用具体数据]

## 修正后路线
[新的技术路线，含决策依据]

## 执行计划
| 任务 | 负责人 | Deadline | 验收标准 |
|------|--------|----------|---------|
| [任务1] | [姓名] | [日期] | [标准] |

## 风险记录
[记录反对意见或潜在风险，即使被否决也要记录]

## 下次复盘
[日期]
```

**完整版 ADR**（可选，仅用于重大决策）：见 `templates/full-adr.md`

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 会议时长 | 基于历史会议效率：若历史会议平均 1.5 小时可达成共识，则设为 2 小时 |
| 参会人数 | 核心决策圈 + 1 名反对者（避免信息过载） |
| 复盘间隔 | 基于问题复杂度：简单问题 1 周，复杂问题 2-4 周 |


---

### Phase 05 Agile Iteration

通过灰度发布、AB 测试、快速切换验证假设，无效则立即调整。

## 通过标准
- [ ] 灰度发布已部署，流量切换比例：10% → 30% → 50% → 100%，每阶段稳定 3 天
- [ ] AB 测试已运行，新系统 vs 旧系统核心指标差异 < 历史波动 P95
- [ ] 方案切换机制已验证（可在 5 分钟内从 100% 新系统切回 100% 旧系统）
- [ ] 若某方案验证失败，已记录失败原因并切换到备选方案

## 失败标准（触发回滚）
- 灰度阶段新系统错误率 > 旧系统历史错误率 P95 × 1.5
- 灰度阶段客户投诉量上升 > 历史投诉波动 P95
- 方案切换机制无法在 5 分钟内完成

## Skip 条件
- 模块无用户可见影响（后台数据处理）→ 跳过 AB 测试，仅做灰度
- 技术方案已在前一项目中验证过 → 跳过"方案验证"，直接灰度

## Rollback 方案
- 流量切换回旧系统（版本路由秒级切换）
- 失败方案记录到《失败方案库》，避免团队重复踩坑

---

## 发布策略选择指南

| 策略 | 适用场景 | 切换时间 | 风险 |
|------|---------|---------|------|
| **灰度发布**（按用户/流量比例） | 大多数场景 | 数天到数周 | 中 |
| **金丝雀发布**（按特定用户群） | 新功能验证、高风险变更 | 数小时到数天 | 低 |
| **蓝绿部署**（全量切换） | 活动高峰期、需要秒级回滚 | 秒级 | 低（成本高） |
| **特性开关**（按功能维度） | 游戏内购、A/B 测试 | 实时 | 低 |

**选择建议**：
- 支付相关模块：优先蓝绿部署或特性开关（秒级回滚）
- 用户资料查询：灰度发布即可
- 新功能上线：金丝雀发布（内部员工/特定地区先体验）

---

## 伪代码示例：灰度发布

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。

```php
// 伪代码：基于用户 ID 哈希的灰度发布
// 关键注意事项：
// 1. ⚠️ 必须支持动态调整比例：通过配置中心热更新，无需重启
// 2. ⚠️ 必须记录灰度用户：用于问题排查和回滚通知
// 3. ⚠️ 必须支持白名单：关键用户（如：内部员工、VIP）可强制走旧系统或新系统
// 4. ⚠️ 必须监控灰度用户的新系统体验：延迟、错误率、转化率

function routeGrayRelease($request) {
    $userId = $request->getUserId();
    $grayPhase = getConfig('gray_release.phase'); // 动态配置

    // 白名单检查
    if (isInWhitelist($userId, $grayPhase)) {
        return routeToVersion($request, getWhitelistVersion($userId));
    }

    // 基于用户 ID 哈希的灰度分配
    $hash = crc32($userId) % 100;
    $ratio = getGrayRatio($grayPhase); // 10, 30, 50, 100

    if ($hash < $ratio) {
        logGrayUser($userId, $grayPhase); // 记录灰度用户
        return routeToVersion($request, 'v2');
    }

    return routeToVersion($request, 'v1');
}

// 灰度比例配置（动态）
function getGrayRatio($phase) {
    return match($phase) {
        'phase1' => 10,
        'phase2' => 30,
        'phase3' => 50,
        'phase4' => 100,
        default => 0,
    };
}
```

---

## 伪代码示例：特性开关（Feature Toggle）

```php
// 伪代码：特性开关（适用于游戏内购等场景）
// 关键注意事项：
// 1. ⚠️ 开关必须支持实时切换：通过配置中心，毫秒级生效
// 2. ⚠️ 开关必须有默认状态：配置中心不可用时，默认走旧逻辑
// 3. ⚠️ 开关必须有监控：记录开关状态变更和流量分布
// 4. ⚠️ 开关必须可清理：功能稳定后，必须删除开关代码，避免技术债务

function processPayment($request) {
    $featureFlag = getFeatureFlag('new_payment_flow');

    if ($featureFlag === 'enabled') {
        return newPaymentFlow($request);
    }

    // 默认：旧逻辑
    return oldPaymentFlow($request);
}

// 配置中心获取（带本地缓存和默认回退）
function getFeatureFlag($name) {
    try {
        return configCenter()->get($name, 'disabled'); // 默认 disabled
    } catch (Exception $e) {
        logError('Config center unavailable', ['flag' => $name]);
        return 'disabled'; // 关键：默认走旧逻辑
    }
}
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 灰度阶段错误率容忍 | 旧系统历史错误率 P95 × 1.5 |
| 客户投诉量容忍 | 历史投诉量波动 P95（排除季节性因素） |
| 灰度稳定期 | 基于历史问题暴露时间：若历史问题通常在 2 天内暴露，则稳定期设为 3 天 |
| 切换时间要求 | 基于业务容忍度：支付模块 1 分钟，非核心模块 5 分钟 |


---

### Phase 06 Core Bottleneck

集中最强资源，在明确 deadline 内解决核心性能瓶颈或关键 Bug。

## 通过标准
- [ ] 核心问题已明确（如：支付回调处理延迟 > 历史 P95 × 2）
- [ ] 攻坚小组已组建（1 架构师 + 1 核心开发 + 1 测试，可选）
- [ ] Deadline 已设定（技术评估时间 × 1.5 倍缓冲，最长不超过 2 周）
- [ ] 攻坚期间，小组成员不参与其他项目，日常维护由"牵制部队"承担
- [ ] 问题已解决，且验证通过（性能提升 > 历史基线 50%，或 Bug 修复率 100%）

## 失败标准（触发回滚）
- Deadline 到期未解决，且未提前 3 天预警
- 攻坚期间引入新 Bug，导致其他模块故障率 > 历史 P95
- 攻坚小组成员 burnout（匿名问卷评分 < 3/5）

## Skip 条件
- 前 5 个 phase 无核心瓶颈（性能指标全部达标）→ 跳过此阶段
- 瓶颈可通过扩容解决（如：增加 Swoole Worker 数）→ 优先扩容，不攻坚

## Rollback 方案
- Deadline 到期未解决 → 启动 Plan B（临时扩容、降级方案、回滚到旧系统）
- 攻坚失败记录到《攻坚失败案例库》，分析根因

---

## 攻坚小组组建规范

| 角色 | 人数 | 职责 | 要求 |
|------|------|------|------|
| 队长（架构师） | 1 | 技术决策、方案设计、风险评估 | 必须理解当前技术栈 |
| 核心开发 | 1 | 编码实现、性能测试 | 必须熟悉目标模块 |
| 测试（可选） | 1 | 测试用例、回归验证 | 若有自动化测试可省略 |
| 替补成员 | 1 | 队长不可用时接手 | 提前指定，熟悉上下文 |

**资源隔离**：
- 攻坚期间不参与其他项目
- 日常维护由"牵制部队"（1-2 人）承担
- 牵制部队负责人提前指定

---

## 伪代码示例：性能瓶颈排查

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。

```php
// 伪代码：性能瓶颈排查（Swoole + Hyperf 环境）
// 关键注意事项：
// 1. ⚠️ 必须采集多维指标：不能只采集响应时间，必须同时采集 CPU、内存、IO、网络
// 2. ⚠️ 必须区分系统瓶颈和代码瓶颈：系统瓶颈（如：MySQL 连接池耗尽）需要扩容，代码瓶颈（如：N+1 查询）需要优化
// 3. ⚠️ 必须在生产环境采样：开发环境数据通常无法反映真实压力
// 4. ⚠️ 必须记录基线：优化前后的对比必须基于同一时段、同一负载

function diagnoseBottleneck($module, $duration = '1h') {
    $metrics = collectMetrics($module, $duration);

    // 1. 检查 Swoole 协程和内存
    $coroutineMetrics = [
        'coroutine_num' => swoole_coroutine_stats()['coroutine_num'],
        'coroutine_peak_num' => swoole_coroutine_stats()['coroutine_peak_num'],
        'memory_usage' => memory_get_usage(true),
        'memory_peak' => memory_get_peak_usage(true),
    ];

    // 2. 检查 MySQL 连接池
    $dbMetrics = [
        'pool_used' => getDbPoolUsedConnections(),
        'pool_total' => getDbPoolTotalConnections(),
        'slow_queries' => countSlowQueries($duration),
    ];

    // 3. 检查 Redis 连接池
    $redisMetrics = [
        'pool_used' => getRedisPoolUsedConnections(),
        'pool_total' => getRedisPoolTotalConnections(),
        'memory_usage' => getRedisMemoryUsage(),
    ];

    // 4. 检查 Kafka 消费延迟
    $kafkaMetrics = [
        'consumer_lag' => getKafkaConsumerLag($module),
        'consume_rate' => getKafkaConsumeRate($module),
    ];

    // 5. 分析瓶颈类型
    $bottleneck = analyzeBottleneck([
        'coroutine' => $coroutineMetrics,
        'db' => $dbMetrics,
        'redis' => $redisMetrics,
        'kafka' => $kafkaMetrics,
    ]);

    return $bottleneck;
}

function analyzeBottleneck($metrics) {
    // 系统瓶颈判断（需要扩容）
    if ($metrics['db']['pool_used'] / $metrics['db']['pool_total'] > 0.9) {
        return ['type' => 'system', 'component' => 'db_pool', 'action' => 'scale_up'];
    }

    if ($metrics['redis']['pool_used'] / $metrics['redis']['pool_total'] > 0.9) {
        return ['type' => 'system', 'component' => 'redis_pool', 'action' => 'scale_up'];
    }

    // 代码瓶颈判断（需要优化）
    if ($metrics['db']['slow_queries'] > getThreshold('slow_query_count')) {
        return ['type' => 'code', 'component' => 'sql', 'action' => 'optimize_queries'];
    }

    if ($metrics['coroutine']['memory_peak'] > getThreshold('memory_peak')) {
        return ['type' => 'code', 'component' => 'memory_leak', 'action' => 'fix_leak'];
    }

    return ['type' => 'unknown', 'action' => 'deep_dive_profiling'];
}
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 核心问题判定 | 性能指标 > 历史 P95 × 2，或故障率 > 历史 P95 × 3 |
| Deadline 缓冲 | 技术评估时间 × 1.5（基于历史攻坚项目的实际耗时/预估耗时比率） |
| 性能提升目标 | 优化后 P95 < 优化前 P95 × 0.5（即提升 50%） |
| 新 Bug 容忍 | 其他模块故障率 < 历史 P95 |


---

### Phase 07 Tech Dispute

技术争论超过 2 周无结论时，由决策人拍板，保护核心团队。

## 通过标准
- [ ] 技术分歧已明确（双方方案已书面化，含优缺点分析）
- [ ] 争论时长已确认（超过 2 周或 3 次会议无结论）
- [ ] 决策人已听取双方意见，并做出最终决定
- [ ] 反对意见已记录（即使被否决）
- [ ] 核心团队未分裂（所有成员继续执行决策，即使个人不同意）

## 失败标准（触发回滚）
- 决策人未在 48 小时内做出决定
- 决策后仍有团队成员拒绝执行，导致项目停滞
- 决策导致核心数据或核心产品受损

## Skip 条件
- 分歧在 2 周内通过数据验证解决（如：AB 测试证明某方案更优）→ 无需裁决
- 分歧仅涉及非核心模块 → 由模块负责人直接决定，无需升级

## Rollback 方案
- 决策被证明错误（数据验证）→ 立即召开紧急复盘，修正决策
- 团队成员因决策离职 → 启动人才替补计划

---

## 决策人资格检查清单

决策人必须满足以下条件，方可行使决策权：

- [ ] **技术理解**：必须理解当前技术栈（Hyperf/Swoole/Kafka/MySQL）的基本原理
- [ ] **上下文掌握**：必须阅读过本次分歧的双方方案文档
- [ ] **数据意识**：必须要求双方提供数据验证（PoC、性能测试、故障模拟）
- [ ] **反对意见听取**：必须给予双方同等时间陈述，不得打断
- [ ] **责任承担**：必须对决策结果负责，决策错误时主动复盘

**不满足以上条件时**：决策人应委托满足条件的人代行决策权，或延长决策时间以补足条件。

---

## 技术分歧裁决模板

```markdown
## 技术分歧裁决 - [模块名称]

### 分歧概述
- 分歧主题：[如：订单状态机用 Saga 模式还是 TCC 模式]
- 争论时长：[X 周 / X 次会议]
- 涉及人员：[名单]

### 方案 A
- 提出人：[姓名]
- 方案描述：[详细描述]
- 优点：[1, 2, 3]
- 缺点：[1, 2, 3]
- 数据验证：[PoC 结果 / 性能测试 / 无]

### 方案 B
- 提出人：[姓名]
- 方案描述：[详细描述]
- 优点：[1, 2, 3]
- 缺点：[1, 2, 3]
- 数据验证：[PoC 结果 / 性能测试 / 无]

### 决策
- 决策人：[姓名]
- 决定采用：方案 [A/B]
- 决策理由：[必须基于数据或团队约束，而非个人偏好]
- 执行时限：[日期]

### 风险记录
[记录反对意见或潜在风险]

### 风险预案
- 若方案 A 失败：[Plan B]
- 若核心成员离职：[替补计划]
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 争论超期判定 | 2 周或 3 次会议（基于历史决策效率：若历史平均 1 周可决策，则收紧到 1.5 周） |
| 决策时限 | 48 小时（基于业务紧急度：支付相关 24 小时，非核心 72 小时） |
| 决策人资格 | 必须完成上述 5 项检查清单 |


---

### Phase 08 Delivery

定义验收标准，完成交付，给团队休整窗口。

## 通过标准
- [ ] 验收标准已定义（见下方验收标准清单）
- [ ] 所有模块已通过验收（或明确标记为"遗留问题"并分配负责人）
- [ ] 多团队已签署《交付确认书》（各团队负责人确认）
- [ ] 团队休整窗口已安排（建议基于历史项目节奏：通常为 3-5 天或 1 个 Sprint 的 20%）

## 失败标准（触发回滚）
- 核心模块未通过验收，但被迫交付
- 无休整窗口，团队立即投入下一项目
- 遗留问题未分配负责人，成为"技术债务黑洞"

## Skip 条件
- 单人项目 → 无需"多团队协作验收"，但必须有"自我验收清单"

## Rollback 方案
- 核心模块未通过验收 → 延期交付，修复后再验收
- 遗留问题未分配负责人 → 强制分配，否则项目不关闭

---

## 验收标准清单

| 维度 | 验收标准 | 测试方法 | 通过阈值 |
|------|---------|---------|---------|
| 功能完整性 | 所有核心功能可用 | 自动化测试 + 人工走查 | 核心功能 100% 通过 |
| 性能指标 | 响应时间、QPS、内存占用 | 压测工具（wrk/ab） | P95 < 历史 P95 × 0.8（即提升 20%） |
| 数据一致性 | 新旧系统数据一致 | 数据对比脚本 | 核心数据 100% 一致，非核心 < 历史错误率 P95 |
| 监控覆盖 | 所有关键路径有监控 | 检查 Grafana/Prometheus | 关键路径 100% 覆盖 |
| 告警有效 | 告警规则准确，无噪音 | 模拟故障触发告警 | 告警准确率 > 90%（基于历史告警数据） |
| 文档完整 | 架构文档、API 文档、运维手册 | 文档审查 | 核心模块 100% 有文档 |
| 回滚可行 | 可在容忍时间内回滚到旧系统 | 演练回滚操作 | 回滚时间 < 业务容忍时间（支付 1 分钟，非核心 5 分钟） |
| 合规审计 | 审计日志完整 | 审计日志抽查 | 100% 核心操作有审计日志 |

---

## 交付确认书模板

```markdown
## 交付确认书 - [项目名称]

### 交付内容
- [ ] 模块清单：[列出所有交付模块]
- [ ] 遗留问题：[列出未解决的问题，含负责人和 Deadline]

### 验收结果
| 维度 | 结果 | 备注 |
|------|------|------|
| 功能完整性 | [通过/未通过] | [备注] |
| 性能指标 | [通过/未通过] | [备注] |
| 数据一致性 | [通过/未通过] | [备注] |
| 监控覆盖 | [通过/未通过] | [备注] |
| 告警有效 | [通过/未通过] | [备注] |
| 文档完整 | [通过/未通过] | [备注] |
| 回滚可行 | [通过/未通过] | [备注] |
| 合规审计 | [通过/未通过] | [备注] |

### 团队确认
- [ ] 后端团队负责人：[确认] [日期]
- [ ] 前端团队负责人：[确认] [日期]
- [ ] 运维团队负责人：[确认] [日期]
- [ ] 测试团队负责人：[确认] [日期]
- [ ] 架构师：[确认] [日期]

### 休整窗口（技术债务清理 Sprint）
- 时间：[日期范围]
- 安排：无新功能开发，只做维护、学习、文档补全、技术债务清理
- 下一项目启动时间：[日期]

### 遗留问题跟踪
| 问题 | 负责人 | Deadline | 优先级 |
|------|--------|----------|--------|
| [问题1] | [姓名] | [日期] | [P0/P1/P2] |
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 性能提升目标 | 优化后 P95 < 优化前 P95 × 0.8（基于历史优化项目的数据） |
| 告警准确率 | 基于历史告警数据：真实告警 / 总告警 > 90% |
| 回滚时间 | 基于业务容忍度：支付模块 1 分钟，非核心 5 分钟（业务方确认） |
| 休整窗口时长 | 基于团队历史恢复周期：若历史数据显示团队需要 4 天恢复，则设为 4 天 |


---

### Phase 09 Ops Governance

从"项目模式"转向"产品模式"，建立监控、告警、文档、合规审计体系。

## 通过标准
- [ ] 监控体系已建立（见下方监控清单）
- [ ] 告警规则已配置（无噪音，准确率 > 历史告警准确率 P95）
- [ ] 运维文档已完整（架构文档、API 文档、故障处理手册、回滚手册）
- [ ] 技术资产已沉淀（复用组件、工具脚本、配置模板）
- [ ] 合规审计已通过（支付数据审计日志完整）
- [ ] 容量规划已制定（未来 6 个月增长预测 + 扩容计划）
- [ ] 故障演练已执行（Chaos Engineering：模拟 Redis 故障、MySQL 主从切换、Kafka 消费延迟）

## 失败标准（触发回滚）
- 监控缺失导致故障发现延迟 > 历史故障发现时间 P95
- 告警噪音率 > 50%（团队麻木，忽略告警）
- 合规审计未通过（核心操作缺失审计日志）

## Skip 条件
- 项目为内部工具，无合规要求 → 跳过"合规审计"
- 系统流量极低（< 100 QPS）→ 跳过"容量规划"和"故障演练"

## Rollback 方案
- 监控缺失 → 立即补全监控，故障发现延迟期间启用人工巡检
- 告警噪音 → 暂停非关键告警，优化规则后重新启用
- 合规未通过 → 暂停相关功能上线，补全审计日志后重新评估

---

## 监控清单（Hyperf/Swoole/Kafka/MySQL/Redis）

| 监控项 | 告警阈值（动态校准） | 检查频率 |
|--------|---------------------|---------|
| Swoole Worker 内存占用 | > 历史 P95 × 1.2 | 实时 |
| Swoole 协程数 | > 历史峰值 P95 | 实时 |
| MySQL 连接池使用率 | > 历史 P95 × 0.9 | 实时 |
| MySQL 慢查询数 | > 历史 P95 × 1.5 / 分钟 | 1 分钟 |
| Redis 连接池使用率 | > 历史 P95 × 0.9 | 实时 |
| Redis 内存使用率 | > 历史 P95 | 实时 |
| Kafka 消费延迟 | > 历史 P95 × 2 | 1 分钟 |
| Kafka 消费速率 | < 历史 P95 × 0.5 | 1 分钟 |
| HTTP 请求 P95 延迟 | > 历史 P95 | 实时 |
| HTTP 错误率 | > 历史错误率 P95 × 1.5 | 实时 |
| 业务指标（订单量） | 同比下降 > 历史波动 P95 | 5 分钟 |
| 审计日志完整性 | 缺失率 > 0（零容忍） | 1 小时 |

---

## 告警降噪与升级路径

### 降噪规则
1. **合并告警**：同一问题 5 分钟内只告警 1 次，后续发送"聚合通知"
2. **静默期**：非工作时间（22:00-08:00）仅 P0 告警通知，P1/P2 延迟到工作时间
3. **自动恢复**：指标恢复后自动发送"恢复通知"，避免人工确认
4. **阈值动态调整**：每周 review 告警准确率，准确率 < 80% 的告警规则必须调整阈值

### 升级路径
| 级别 | 响应时间 | 升级条件 | 升级对象 |
|------|---------|---------|---------|
| P0 | 5 分钟 | 核心交易中断、数据丢失 | 立即通知值班 + 技术负责人 + 业务负责人 |
| P1 | 30 分钟 | 性能严重下降、部分功能不可用 | 通知值班 + 技术负责人 |
| P2 | 4 小时 | 非核心功能异常、监控告警 | 通知值班，工作时间处理 |
| P3 | 24 小时 | 优化建议、容量预警 | 周报汇总，不即时通知 |

---

## 合规审计声明

⚠️ **重要免责声明**：
- 本 Skill **不提供 PCI DSS、GDPR 等合规认证指导**。
- 以下检查项仅为**工程实践参考**，不构成合规认证依据。
- **必须咨询专业合规团队**进行正式认证。

### 支付数据审计日志（参考实现）

⚠️ **免责声明**：以下代码仅为逻辑示意，未经测试，不可直接用于生产环境。

```php
// 伪代码：支付操作审计日志
// 关键注意事项：
// 1. ⚠️ 必须记录完整上下文：操作类型、用户、金额、渠道、前后状态、操作人、请求 ID
// 2. ⚠️ 必须防篡改：审计日志应写入 WORM 存储或独立数据库，应用层不可修改
// 3. ⚠️ 必须异步写入：不能阻塞支付主流程，失败时进入本地队列重试
// 4. ⚠️ 必须定期检查：每小时检查核心操作是否有对应审计日志，缺失率 > 0 立即告警
// 5. ⚠️ 必须保留期限：根据合规要求设定保留期限（如：PCI DSS 要求 1 年）
// 6. ⚠️ 查询性能：审计表必须按时间分区，避免全表扫描导致锁表

function logPaymentAudit($action, $context) {
    $log = [
        'timestamp' => microtime(true),
        'action' => $action, // create_order, update_status, refund, etc.
        'user_id' => $context['user_id'] ?? null,
        'order_no' => $context['order_no'] ?? null,
        'amount' => $context['amount'] ?? null,
        'channel' => $context['channel'] ?? null, // wechat, alipay, apple, etc.
        'ip' => $context['ip'] ?? null,
        'request_id' => $context['request_id'] ?? null,
        'before_status' => $context['before_status'] ?? null,
        'after_status' => $context['after_status'] ?? null,
        'operator' => $context['operator'] ?? 'system',
    ];

    // 异步写入审计日志（不阻塞主流程）
    async(function() use ($log) {
        try {
            // 写入独立审计数据库（不可由应用层修改）
            writeToAuditDB($log);

            // 同时发送 Kafka，用于实时审计监控
            sendToKafka('audit-log-topic', $log);

        } catch (Exception $e) {
            // 失败时写入本地队列，稍后重试
            writeToLocalAuditQueue($log);
            logError('Audit log failed', ['error' => $e, 'log' => $log]);
        }
    });
}

// 伪代码：审计日志完整性检查
// 关键注意事项：
// 1. ⚠️ 必须按时间分区查询：避免全表扫描，只查最近 1 小时分区
// 2. ⚠️ 必须在从库或只读实例执行：避免影响主库性能
// 3. ⚠️ 缺失率 > 0 必须立即告警，但告警阈值不是"0 条缺失"，而是"缺失率 > 0.001%"（允许网络抖动等偶发情况）

function checkAuditLogIntegrity($timeWindow = '1h') {
    $startTime = now()->sub($timeWindow);

    // 在只读实例上执行
    $businessCount = readOnlyDB()->table('orders')
        ->where('created_at', '>', $startTime)
        ->count();

    $auditCount = readOnlyDB()->table('payment_audit_log')
        ->where('timestamp', '>', $startTime->getTimestamp())
        ->count();

    $missingRate = ($businessCount - $auditCount) / $businessCount;

    // 阈值：允许 0.001% 的偶发缺失（网络抖动、磁盘满等）
    $threshold = 0.00001; // 0.001%

    if ($missingRate > $threshold) {
        alert("Audit log missing rate: {$missingRate}");
    }

    return $missingRate;
}
```

---

## 阈值校准

| 指标 | 校准方法 |
|------|---------|
| 监控告警阈值 | 历史 P95 × 1.2（允许 20% 波动） |
| 告警准确率 | 历史真实告警 / 总告警的 P95 |
| 故障发现延迟 | 历史故障发现时间的 P95 |
| 审计日志缺失率 | 0.001%（允许偶发网络抖动，非绝对 0） |
| 容量规划 | 基于过去 6 个月增长趋势的线性外推 + 业务方活动计划 |


---

## 六、反模式

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

## 七、与已有 Skill 兼容性声明

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

## 八、监控体系建设指南

> 本指南帮助团队从"零监控"或"基础监控"逐步建立到能获取"历史 P95"的监控体系。
> 
> 目标：让团队能在 2-4 周内获取核心指标的 P95 值，为动态阈值提供数据基础。
> 
> 如果团队已有 Prometheus/Grafana，可直接跳到"阶段 3"。

---

## 阶段 1：基础日志 + 简单告警（1 周完成）

**目标**：建立最基本的可观测性，能回答"系统是否挂了"。

**适用场景**：团队只有服务器 CPU/内存监控，没有应用层监控。

### 1.1 应用日志标准化

所有 HTTP 请求、数据库操作、缓存操作必须记录结构化日志：

```json
{
  "timestamp": "2026-07-03T10:30:00.000Z",
  "level": "INFO",
  "request_id": "req-abc123",
  "module": "order",
  "action": "create_order",
  "duration_ms": 150,
  "status": "success",
  "error_code": null,
  "user_id": "user-456",
  "order_no": "ORD-789"
}
```

**关键字段**：
- `timestamp`：ISO 8601 格式，精确到毫秒
- `request_id`：全链路唯一 ID，用于追踪
- `module`：业务模块（order, payment, user 等）
- `action`：具体操作
- `duration_ms`：操作耗时（毫秒）
- `status`：success / error / timeout
- `error_code`：错误码（如有）

**Hyperf 配置示例**：

```php
// config/autoload/logger.php
return [
    'default' => [
        'handler' => [
            'class' => Monolog\Handler\StreamHandler::class,
            'constructor' => [
                'stream' => BASE_PATH . '/runtime/logs/hyperf.log',
                'level' => Monolog\Logger::DEBUG,
            ],
        ],
        'formatter' => [
            'class' => Monolog\Formatter\JsonFormatter::class,
        ],
    ],
];

// 在中间件中记录请求日志
class RequestLogMiddleware implements MiddlewareInterface
{
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler)
    {
        $start = microtime(true);
        $requestId = $request->getHeaderLine('X-Request-ID') ?: uniqid('req-');

        try {
            $response = $handler->handle($request);
            $status = 'success';
            $errorCode = null;
        } catch (\Throwable $e) {
            $status = 'error';
            $errorCode = get_class($e);
            throw $e;
        } finally {
            $duration = (microtime(true) - $start) * 1000;

            logger()->info('request_log', [
                'request_id' => $requestId,
                'module' => getModuleFromPath($request->getUri()->getPath()),
                'action' => $request->getUri()->getPath(),
                'duration_ms' => round($duration, 2),
                'status' => $status,
                'error_code' => $errorCode,
                'user_id' => $request->getAttribute('user_id'),
            ]);
        }

        return $response;
    }
}
```

⚠️ **注意事项**：
- 日志必须异步写入，不能阻塞请求响应
- 日志文件必须按天切割，避免单文件过大
- 敏感字段（密码、token）必须脱敏

### 1.2 简单告警（脚本级别）

用脚本定期检查日志，发现异常时发送告警：

```bash
#!/bin/bash
# check_errors.sh - 每小时检查错误率

LOG_FILE="/data/www/logs/hyperf-$(date +%Y-%m-%d).log"
HOUR=$(date +%H)

# 统计过去 1 小时的错误数
ERROR_COUNT=$(grep '"status":"error"' $LOG_FILE | grep "$(date +%Y-%m-%dT$HOUR)" | wc -l)
TOTAL_COUNT=$(grep '"timestamp"' $LOG_FILE | grep "$(date +%Y-%m-%dT$HOUR)" | wc -l)

if [ $TOTAL_COUNT -gt 0 ]; then
    ERROR_RATE=$(echo "scale=4; $ERROR_COUNT / $TOTAL_COUNT * 100" | bc)

    # 阈值：基于历史观察，初始设为 1%（后续根据数据调整）
    if (( $(echo "$ERROR_RATE > 1.0" | bc -l) )); then
        echo "ALERT: Error rate ${ERROR_RATE}% in hour $HOUR" |         mail -s "[ALERT] Error Rate High" ops@rastargame.com
    fi
fi
```

**初始阈值设定方法**：
- 运行脚本 1 周，收集每小时错误率
- 取 P95 作为初始阈值（如：P95 = 0.5%，则阈值设为 0.5%）
- 后续每周根据新数据调整

---

## 阶段 2：Prometheus + Grafana 基础部署（2-3 周完成）

**目标**：建立应用层指标监控，能获取 P50/P95/P99 响应时间、QPS、错误率。

**适用场景**：团队已有基础日志，需要更精细的指标和可视化。

### 2.1 Hyperf Metric 组件配置

```php
// composer require hyperf/metric

// config/autoload/metric.php
return [
    'default' => 'prometheus',
    'prometheus' => [
        'driver' => Hyperf\Metric\Adapter\Prometheus\MetricFactory::class,
        'mode' => Hyperf\Metric\Constants::MODE_PULL,
        'route' => '/metrics',
    ],
];

// 在中间件中记录 HTTP 指标
class MetricMiddleware implements MiddlewareInterface
{
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler)
    {
        $start = microtime(true);
        $path = $request->getUri()->getPath();
        $method = $request->getMethod();

        try {
            $response = $handler->handle($request);
            $status = (string) $response->getStatusCode();
        } catch (\Throwable $e) {
            $status = '500';
            throw $e;
        } finally {
            $duration = microtime(true) - $start;

            // 记录响应时间直方图（自动计算 P50/P95/P99）
            make_histogram('http_request_duration_seconds', ['path', 'method', 'status'])
                ->observe($duration, [$path, $method, $status]);

            // 记录请求计数
            make_counter('http_requests_total', ['path', 'method', 'status'])
                ->inc([$path, $method, $status]);
        }

        return $response;
    }
}
```

### 2.2 数据库连接池监控

```php
// 在数据库连接池管理中记录指标
class ConnectionPoolMetrics
{
    public function recordPoolMetrics($pool)
    {
        make_gauge('db_pool_used_connections', ['pool_name'])
            ->set($pool->getCurrentConnections(), [$pool->getName()]);

        make_gauge('db_pool_total_connections', ['pool_name'])
            ->set($pool->getMaxConnections(), [$pool->getName()]);

        make_gauge('db_pool_wait_queue', ['pool_name'])
            ->set($pool->getWaitQueueLength(), [$pool->getName()]);
    }
}
```

### 2.3 Redis 连接池监控

```php
class RedisPoolMetrics
{
    public function recordPoolMetrics($pool)
    {
        make_gauge('redis_pool_used_connections', ['pool_name'])
            ->set($pool->getCurrentConnections(), [$pool->getName()]);

        make_gauge('redis_pool_total_connections', ['pool_name'])
            ->set($pool->getMaxConnections(), [$pool->getName()]);
    }
}
```

### 2.4 Swoole 协程监控

```php
// 在自定义进程中定期采集 Swoole 协程指标
class CoroutineMetricsProcess extends AbstractProcess
{
    public function handle(): void
    {
        while (true) {
            $stats = swoole_coroutine_stats();

            make_gauge('swoole_coroutine_num')
                ->set($stats['coroutine_num']);

            make_gauge('swoole_coroutine_peak_num')
                ->set($stats['coroutine_peak_num']);

            make_gauge('swoole_coroutine_active_num')
                ->set($stats['coroutine_num'] - $stats['coroutine_peak_num']);

            sleep(5); // 每 5 秒采集一次
        }
    }
}
```

### 2.5 Grafana Dashboard 配置

创建 Dashboard，包含以下 Panel：

| Panel | 查询 | 说明 |
|-------|------|------|
| HTTP P95 延迟 | `histogram_quantile(0.95, http_request_duration_seconds_bucket)` | 按 path 分组 |
| HTTP 错误率 | `rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m])` | 按 path 分组 |
| QPS | `rate(http_requests_total[1m])` | 按 path 分组 |
| DB 连接池使用率 | `db_pool_used_connections / db_pool_total_connections` | 按 pool_name 分组 |
| Redis 连接池使用率 | `redis_pool_used_connections / redis_pool_total_connections` | 按 pool_name 分组 |
| Swoole 协程数 | `swoole_coroutine_num` | 实时 |

**P95 获取方法**：
- 在 Grafana 中查看 HTTP P95 延迟 Panel
- 选择时间范围：过去 3 个月
- 导出数据或截图，记录每个模块的 P95 值
- 每周更新一次（数据漂移）

---

## 阶段 3：业务指标监控（1 周完成）

**目标**：监控业务层面的指标（订单量、支付成功率、用户活跃度），用于业务影响评估。

### 3.1 业务指标定义

| 指标 | 类型 | 采集方式 | 告警阈值 |
|------|------|---------|---------|
| 订单创建量 | Counter | 订单创建时 +1 | 同比下降 > 历史波动 P95 |
| 支付成功率 | Gauge | 支付成功数 / 支付总数 | < 历史 P95 × 0.95 |
| 用户注册量 | Counter | 注册时 +1 | 同比下降 > 历史波动 P95 |
| 活跃用户 | Gauge | 每小时统计 | 同比下降 > 历史波动 P95 |

```php
// 业务指标采集示例
class OrderMetrics
{
    public function recordOrderCreated($order)
    {
        make_counter('business_orders_created_total', ['channel', 'status'])
            ->inc([$order->channel, $order->status]);
    }

    public function recordPaymentResult($payment, $success)
    {
        make_counter('business_payments_total', ['channel', 'result'])
            ->inc([$payment->channel, $success ? 'success' : 'failure']);
    }
}
```

---

## 阶段 4：Swoole/Hyperf 专用监控（1-2 周完成）

**目标**：监控 Swoole 特有的指标（Worker 内存、连接池状态、Task 队列），用于排查协程和内存问题。

### 4.1 Worker 内存监控

```php
class WorkerMemoryMetrics
{
    public function recordMemoryUsage()
    {
        make_gauge('swoole_worker_memory_usage_bytes', ['worker_id'])
            ->set(memory_get_usage(true), [getmypid()]);

        make_gauge('swoole_worker_memory_peak_bytes', ['worker_id'])
            ->set(memory_get_peak_usage(true), [getmypid()]);
    }
}
```

### 4.2 Kafka 消费监控

```php
class KafkaConsumerMetrics
{
    public function recordLag($topic, $group)
    {
        $lag = $this->kafka->getConsumerLag($topic, $group);

        make_gauge('kafka_consumer_lag', ['topic', 'group'])
            ->set($lag, [$topic, $group]);
    }

    public function recordConsumeRate($topic, $group)
    {
        $rate = $this->kafka->getConsumeRate($topic, $group);

        make_gauge('kafka_consume_rate', ['topic', 'group'])
            ->set($rate, [$topic, $group]);
    }
}
```

---

## 无监控时的 Fallback 方案

如果团队当前没有任何监控，无法获取"历史 P95"，使用以下临时方案：

### 临时阈值（基于行业经验）

| 指标 | 临时阈值 | 说明 |
|------|---------|------|
| HTTP P95 延迟 | < 500ms | 游戏内购接口通常要求 < 200ms，后台接口可放宽到 < 1000ms |
| HTTP 错误率 | < 0.1% | 核心交易接口 < 0.01%，非核心接口 < 0.5% |
| DB 连接池使用率 | < 80% | 超过 80% 可能出现连接等待 |
| Redis 连接池使用率 | < 80% | 同上 |
| Swoole 协程数 | < 10000 | 超过 10000 可能内存不足 |
| Kafka 消费延迟 | < 1000 条 | 超过 1000 条可能消费能力不足 |

**使用规则**：
1. 使用临时阈值启动迁移
2. 同时启动阶段 1-2 的监控建设（1-3 周内完成）
3. 监控建设完成后，立即用历史 P95 替换临时阈值
4. 每 2 周重新校准一次

---

## 监控建设检查清单

### 阶段 1 完成检查（1 周后）
- [ ] 所有 HTTP 请求记录结构化日志
- [ ] 日志包含 request_id、module、action、duration_ms、status
- [ ] 日志按天切割
- [ ] 简单告警脚本运行正常（每小时检查错误率）
- [ ] 初始阈值已设定（基于 1 周观察数据）

### 阶段 2 完成检查（3-4 周后）
- [ ] Prometheus 部署完成，/metrics 端点可访问
- [ ] Grafana Dashboard 部署完成，可查看 P95 延迟
- [ ] DB 连接池监控已配置
- [ ] Redis 连接池监控已配置
- [ ] Swoole 协程监控已配置
- [ ] 过去 3 个月的 P95 数据已导出

### 阶段 3 完成检查（4-5 周后）
- [ ] 业务指标（订单量、支付成功率）已采集
- [ ] 业务指标 Dashboard 已配置
- [ ] 业务告警规则已配置

### 阶段 4 完成检查（5-7 周后）
- [ ] Worker 内存监控已配置
- [ ] Kafka 消费延迟监控已配置
- [ ] 所有监控项的告警规则已配置
- [ ] 告警降噪规则已配置（5 分钟内同一问题只告警 1 次）

---

## 阈值校准流程

```
每周一：
  1. 导出过去 7 天的监控数据
  2. 计算每个指标的 P95
  3. 对比上周 P95，若差异 > 10%，分析原因（数据漂移？业务变更？）
  4. 更新 Skill 中的阈值配置
  5. 记录校准日志（ADR 格式，简要记录）

每月：
  1. 回顾过去 4 周的阈值校准记录
  2. 识别趋势（阈值在收紧还是放宽？）
  3. 调整告警策略（如：从固定阈值改为动态阈值）
```

---

> **监控不是迁移的阻碍，而是迁移的基石。没有监控的迁移等于盲飞。**

---

## 九、模板库



### 模板一：架构评审会议

1. 复制本模板到会议文档
2. 按"示例"填写，不要留空
3. 会议前必须完成"数据收集清单"
4. 会议后必须产出 ADR

---

## 示例：RastarGame 支付模块迁移 Phase 4 复盘

### 会议信息
- 项目名称：RastarGame 支付模块迁移
- Phase：Phase 4 复盘（第 1 轮迁移，支付模块）
- 日期：2026-07-10
- 时长：2 小时

### 参会人员
| 角色 | 姓名 | 职责 |
|------|------|------|
| 决策者 | 周俊仁（架构师） | 最终技术决策 |
| 记录者 | 张三（后端组长） | 会议记录 + ADR 产出 |
| 反对者 | 李四（运维工程师） | 必须提出至少 1 个反对意见 |
| 执行者 | 王五（核心开发） | 后续执行 |
| 执行者 | 赵六（测试工程师） | 后续执行 |

### 议程

#### 1. 数据回顾（30 分钟）

| 指标 | 旧系统 | 新系统 | 历史 P95 | 状态 |
|------|--------|--------|----------|------|
| 故障率 | 0.05% | 0.08% | 0.06% | 🔴 异常（新系统 > 历史 P95 × 1.5 = 0.09%） |
| P95 延迟 | 200ms | 350ms | 250ms | 🔴 异常（新系统 > 历史 P95 × 1.2 = 300ms） |
| QPS 峰值 | 5000 | 4800 | 4500 | 🟡 接近阈值（新系统 < 历史 P95 × 0.9 = 2250，但低于旧系统） |
| 团队压力评分 | 3.2/5 | - | 3.5/5 | 🟢 正常 |

**数据收集方法**：
- 故障率：Grafana Dashboard `http_errors_rate` Panel，过去 2 周数据
- P95 延迟：Grafana Dashboard `http_p95_latency` Panel，过去 2 周数据
- QPS：Grafana Dashboard `http_qps` Panel，过去 2 周数据
- 团队压力：匿名问卷（飞书文档），5 人填写，平均 3.2/5

#### 2. 错误识别（30 分钟）

- [x] 无上下文架构采纳？→ **否**（微服务数量 3 个，团队 5 人，比例合理）
- [x] 大爆炸部署？→ **否**（灰度 10%→30%→50%→100%，每阶段稳定 3 天）
- [x] 重写优先综合征？→ **部分是**（Phase 3 遇到支付回调延迟问题，团队曾提议重写回调模块，但经 RCA 分析后发现是 MySQL 索引问题，已优化）
- [x] 分析瘫痪？→ **否**（技术分歧在 1 周内通过数据验证解决）

#### 3. 路线修正（45 分钟）

- 原路线：支付模块直接迁移到 Hyperf 3.2，使用新数据库连接池
- 修正后路线：
  1. 回滚支付模块到 50% 灰度（当前 100% 新系统）
  2. 优化 MySQL 索引（支付回调查询缺少联合索引）
  3. 增加 Redis 缓存层（支付状态缓存 30 秒）
  4. 重新灰度到 100%，观察 3 天
- 决策理由：数据证明 P95 延迟从 200ms 上升到 350ms，根因是 MySQL 查询慢（慢查询日志显示支付回调查询平均 180ms），非架构问题
- 执行时限：2026-07-17（1 周）

#### 4. 决策确认（15 分钟）

- 决策人宣布最终决定：采用修正后路线，回滚到 50% 灰度，优化索引后重新灰度
- 反对意见记录：李四反对回滚到 50%，认为应该直接优化索引后保持 100%。处理方案：保留李四意见，若优化后 P95 仍 > 300ms，则采纳李四方案（直接优化，不回滚）
- 下次复盘时间：2026-07-17（1 周后）

### 产出物
- [x] ADR-004: 支付模块 P95 延迟优化方案
- [x] 会议记录（含李四反对意见）
- [x] 执行计划（王五负责索引优化，赵六负责回归测试，Deadline: 2026-07-14）

---

## 空白模板（复制后填写）

### 会议信息
- 项目名称：[填写]
- Phase：[填写]
- 日期：[填写]
- 时长：2 小时

### 参会人员
| 角色 | 姓名 | 职责 |
|------|------|------|
| 决策者 | [填写] | 最终技术决策 |
| 记录者 | [填写] | 会议记录 + ADR 产出 |
| 反对者 | [填写] | 必须提出至少 1 个反对意见 |
| 执行者 | [填写] | 后续执行 |
| 执行者 | [填写] | 后续执行 |

### 议程

#### 1. 数据回顾（30 分钟）

| 指标 | 旧系统 | 新系统 | 历史 P95 | 状态 |
|------|--------|--------|----------|------|
| 故障率 | [X%] | [Y%] | [Z%] | [正常/异常] |
| P95 延迟 | [Xms] | [Yms] | [Zms] | [正常/异常] |
| QPS 峰值 | [X] | [Y] | [Z] | [正常/异常] |
| 团队压力评分 | [X/5] | - | [Z/5] | [正常/异常] |

**数据收集方法**：
- [填写数据来源，如：Grafana Dashboard、匿名问卷]

#### 2. 错误识别（30 分钟）

- [ ] 无上下文架构采纳？→ [是/否，说明]
- [ ] 大爆炸部署？→ [是/否，说明]
- [ ] 重写优先综合征？→ [是/否，说明]
- [ ] 分析瘫痪？→ [是/否，说明]

#### 3. 路线修正（45 分钟）

- 原路线：[描述]
- 修正后路线：[描述]
- 决策理由：[必须基于数据]
- 执行时限：[日期]

#### 4. 决策确认（15 分钟）

- 决策人宣布最终决定：[描述]
- 反对意见记录：[如有，记录]
- 下次复盘时间：[日期]

### 产出物
- [ ] ADR-XXX：[标题]
- [ ] 会议记录（含反对意见）
- [ ] 执行计划（任务分解到个人）


### 模板二：核心攻坚计划

1. 复制本模板到项目文档
2. 按"示例"填写，不要留空
3. 攻坚前必须完成"资源隔离"确认
4. 攻坚后必须产出"攻坚总结"

---

## 示例：RastarGame 支付回调延迟攻坚

### 攻坚目标
- 核心问题：支付回调处理延迟从 200ms 上升到 350ms，导致用户重复支付（用户以为支付失败，再次点击支付）
- 成功标准：P95 < 200ms（恢复旧系统水平）

### 突击队
| 角色 | 姓名 | 职责 | 替补 |
|------|------|------|------|
| 队长（架构师） | 周俊仁 | 技术决策、方案设计、风险评估 | 张三（后端组长） |
| 核心开发 | 王五 | 编码实现、性能测试 | - |
| 测试 | 赵六 | 测试用例、回归验证 | - |

### 资源隔离
- [x] 攻坚期间不参与其他项目：王五暂停订单模块开发，全力投入支付回调优化
- [x] 日常维护由牵制部队承担：李四（运维工程师）负责日常报警处理、日志查看
- [x] 牵制部队负责人：李四

### 时间计划
- 技术评估时间：3 天（王五分析慢查询日志、Redis 监控、代码审查）
- Deadline：2026-07-14（评估 3 天 × 1.5 = 4.5 天，取整 5 天，从 2026-07-10 开始）
- 每日站会：每天 10:00，15 分钟（王五汇报进度、赵六汇报测试结果、周俊仁评估风险）

### 风险预案
- 若到期未完成：Plan B —— 临时增加 Redis 缓存层（支付状态缓存 30 秒），延迟从 350ms 降到 250ms，同时继续优化 MySQL 索引
- 若引入新 Bug：回滚到攻坚前版本（Git tag: payment-v1.2.0），恢复旧系统逻辑
- 若成员 burnout：张三替补接手，王五强制休息 2 天（2026-07-15 至 2026-07-16）

### 产出物
- [x] 问题根因报告：MySQL 支付回调查询缺少联合索引 (order_no, status)，导致全表扫描
- [x] 优化方案：增加联合索引，预计查询时间从 180ms 降到 20ms
- [x] 性能测试报告：优化后 P95 = 180ms，满足成功标准
- [x] 攻坚总结：成功，耗时 4 天，比 Deadline 提前 1 天

---

## 空白模板（复制后填写）

### 攻坚目标
- 核心问题：[描述，如：支付回调延迟 > 历史 P95 × 2]
- 成功标准：[可量化，如：P95 < 历史 P95 × 0.5]

### 突击队
| 角色 | 姓名 | 职责 | 替补 |
|------|------|------|------|
| 队长（架构师） | [填写] | 技术决策、方案设计 | [填写] |
| 核心开发 | [填写] | 编码实现、性能测试 | - |
| 测试（可选） | [填写] | 测试用例、回归验证 | - |

### 资源隔离
- [ ] 攻坚期间不参与其他项目：[说明]
- [ ] 日常维护由牵制部队承担：[说明]
- [ ] 牵制部队负责人：[姓名]

### 时间计划
- 技术评估时间：[X] 天
- Deadline：[日期]（评估 × 1.5 = [Y] 天）
- 每日站会：每天 [时间]，15 分钟

### 风险预案
- 若到期未完成：[Plan B]
- 若引入新 Bug：[回滚到攻坚前版本]
- 若成员 burnout：[替补成员接手，队长强制休息 2 天]

### 产出物
- [ ] 问题根因报告
- [ ] 优化方案文档
- [ ] 性能测试报告（优化前后对比）
- [ ] 攻坚总结（成功/失败经验）


### 模板三：迁移复盘报告

1. 复制本模板到项目文档
2. 按"示例"填写，不要留空
3. 交付确认书必须所有负责人签字（或 IM 确认）
4. 技术债务清理 Sprint 必须安排，不可跳过

---

## 示例：RastarGame 支付模块迁移复盘（第 1 轮）

### 项目信息
- 项目名称：RastarGame 支付模块迁移
- 迁移周期：2026-06-15 至 2026-07-10（4 周）
- 涉及模块：支付核心、支付回调、支付对账

### 迁移成果
- 完成率：3/3 模块（100%）
- 性能提升：P95 从 200ms → 180ms（优化后），QPS 从 5000 → 5200
- 团队成长：3 人掌握 Hyperf 3.2（王五、赵六、李四），1 人掌握 Kafka DLQ（张三）

### 经验教训

#### 做对了什么
| Phase | 具体做法 | 效果 |
|-------|---------|------|
| Phase 2 | 数据双写提前 2 周部署，旧系统写入成功率 100% | 迁移期间零数据丢失 |
| Phase 4 | 发现 P95 延迟上升后立即回滚到 50% 灰度 | 避免问题扩大，用户投诉仅增加 5 起 |
| Phase 6 | 组建突击队攻坚支付回调延迟，4 天解决 | 比 Deadline 提前 1 天 |

#### 做错了什么
| 反模式 | 具体表现 | 改进措施 |
|--------|---------|---------|
| Rewrite-First Syndrome | Phase 3 遇到延迟问题时，团队曾提议重写回调模块 | 建立 RCA 强制机制，任何重写决策必须有数据支撑 |
| Context-Free Architecture Adoption | 初期计划引入 8 个微服务，后调整为 3 个 | 建立"单体优先"原则，团队人数/微服务数量 < 2 时禁止拆分 |

### 沉淀资产
| 资产类型 | 名称 | 位置 | 复用场景 |
|---------|------|------|---------|
| 组件 | 数据双写 AOP 组件 | `app/Aspect/DataSyncAspect.php` | 第 2 轮订单模块迁移 |
| 脚本 | 数据一致性校验脚本 | `bin/data-consistency-check.php` | 所有迁移项目的 Phase 3 |
| 文档 | 支付模块迁移手册 | 飞书文档《支付模块迁移总结》 | 新成员入职培训 |

### 运维治理
- 监控体系：Grafana `http://grafana.rastargame.internal/d/payment`
- 告警规则：准确率 95%，噪音率 5%（过去 2 周数据）
- 运维文档：飞书文档《支付模块运维手册》
- 合规审计：通过（100% 核心操作有审计日志，PCI DSS 检查项全部通过）

### 遗留问题
| 问题 | 负责人 | Deadline | 优先级 |
|------|--------|----------|--------|
| 支付对账报表延迟从 1 分钟增加到 3 分钟 | 张三 | 2026-07-24 | P1 |
| Redis 缓存未设置过期时间，存在内存泄漏风险 | 李四 | 2026-07-17 | P2 |

### 技术债务清理 Sprint
- 时间：2026-07-13 至 2026-07-17（1 周）
- 安排：
  - 周一：补全支付模块 API 文档（王五）
  - 周二：优化 Redis 缓存过期时间（李四）
  - 周三：整理 Kafka DLQ 消费日志（张三）
  - 周四：团队技术分享：Hyperf 3.2 迁移经验（周俊仁）
  - 周五：代码审查：支付模块重构代码（全员）
- 下一项目启动时间：2026-07-20（第 2 轮：订单模块迁移）

---

## 空白模板（复制后填写）

### 项目信息
- 项目名称：[填写]
- 迁移周期：[日期范围]
- 涉及模块：[列出]

### 迁移成果
- 完成率：[X/Y 模块]
- 性能提升：P95 [旧 Xms → 新 Yms]，QPS [旧 X → 新 Y]
- 团队成长：[描述]

### 经验教训

#### 做对了什么
| Phase | 具体做法 | 效果 |
|-------|---------|------|
| [Phase X] | [描述] | [数据] |

#### 做错了什么
| 反模式 | 具体表现 | 改进措施 |
|--------|---------|---------|
| [反模式] | [描述] | [措施] |

### 沉淀资产
| 资产类型 | 名称 | 位置 | 复用场景 |
|---------|------|------|---------|
| [组件/脚本/文档] | [名称] | [路径] | [场景] |

### 运维治理
- 监控体系：[Grafana 地址]
- 告警规则：[准确率 X%，噪音率 Y%]
- 运维文档：[文档链接]
- 合规审计：[通过/未通过，备注]

### 遗留问题
| 问题 | 负责人 | Deadline | 优先级 |
|------|--------|----------|--------|
| [问题1] | [姓名] | [日期] | [P0/P1/P2] |

### 技术债务清理 Sprint
- 时间：[日期范围]
- 安排：
  - [日期]：[任务]（[负责人]）
  - [日期]：[任务]（[负责人]）
- 下一项目启动时间：[日期]


### 模板四：完整版 ADR

- 提案日期：[日期]
- 决策日期：[日期]
- 状态：已接受 / 已拒绝 / 已取代（被 ADR-YYY 取代）

## 背景
[迁移项目的背景，1-2 段]

## 原路线
[之前选择的技术路线，含决策依据]

## 问题
[数据证明原路线存在的问题，必须引用具体数据]

## 修正后路线
[新的技术路线，含决策依据]

## 影响
- 对已上线模块的影响：[描述]
- 对未迁移模块的影响：[描述]
- 对团队的影响：[描述]
- 对业务方的影响：[描述]

## 执行计划
| 任务 | 负责人 | Deadline | 验收标准 |
|------|--------|----------|---------|
| [任务1] | [姓名] | [日期] | [标准] |

## 风险记录
[记录反对意见或潜在风险，即使被否决也要记录]

## 相关 ADR
- [ADR-YYY]：被本 ADR 取代的 ADR
- [ADR-ZZZ]：与本 ADR 相关的其他 ADR


---

## 十、示例场景



### 示例一：Hyperf 2.2 → 3.2 升级（技术栈迁移）

> 场景：RastarGame 后端服务使用 Hyperf 2.2 + Swoole 4.x，Hyperf 2.2 已 EOL，Swoole 4.x 不再维护，需要升级到 Hyperf 3.2 + Swoole 5.x。
> 
> 项目规模：5 个业务模块（支付、订单、用户、客服、统计），约 8 万行 PHP 代码，团队 8 人。

---

## Phase 1: 危机评估与转移决策

### 问题根因报告

| 问题 | 数据 | 根因 |
|------|------|------|
| Hyperf 2.2 EOL | 官方已停止维护，最后更新 2024-06 | 技术债务 |
| Swoole 4.x 不兼容 PHP 8.2 | 升级 PHP 8.2 后 Swoole 编译失败 | 依赖锁死 |
| 安全漏洞无法修复 | CVE-2024-XXXX 影响 Swoole 4.x，无补丁 | 安全风险 |
| 新特性无法使用 | 需要 Hyperf 3.2 的 Kafka 原生支持 | 功能限制 |

### 迁移策略选择

根据决策树：
- 旧系统还能运行？→ 是（当前运行正常）
- 技术债务是否可局部修复？→ 否（框架 EOL 无法局部修复）
- **结论：渐进式迁移（Strangler Fig）**

### 迁移范围

| 模块 | 优先级 | 迁移方式 | 说明 |
|------|--------|---------|------|
| 支付核心 | P0 | 第 1 轮 | 风险最高，必须优先验证 |
| 订单管理 | P1 | 第 2 轮 | 依赖支付模块，等支付稳定后迁移 |
| 用户中心 | P1 | 第 2 轮 | 相对独立，可与订单并行 |
| 客服系统 | P2 | 第 3 轮 | 非核心，可延后 |
| 统计报表 | P2 | 第 3 轮 | 非核心，可延后 |

### 业务方确认
- 确认方式：邮件（2026-06-01，CTO 发全员邮件说明迁移计划）
- 接受影响：支付模块迁移期间，支付成功率可能短暂下降（灰度阶段），报表延迟可能增加 2-3 分钟

---

## Phase 2: 突围准备与暗线部署

### 技术预研（PoC）

**PoC 目标**：验证 Hyperf 3.2 + Swoole 5.x 能否运行支付核心接口（创建订单、支付回调、查询订单状态）。

**PoC 结果**：
- ✅ Hyperf 3.2 安装成功
- ✅ Swoole 5.x 编译成功（PHP 8.2）
- ✅ 支付核心接口可运行（创建订单、支付回调）
- ⚠️ Kafka 消费者配置变化：Hyperf 3.2 的 Kafka 组件配置格式与 2.2 不同，需要调整
- ⚠️ DB 连接池配置变化：Hyperf 3.2 默认连接池大小从 10 改为 32，需要评估

**PoC 耗时**：3 天（1 人）

### 数据双写部署

**方案**：
- 旧系统（Hyperf 2.2）写入 MySQL 后，通过 AOP 拦截，异步发送到 Kafka
- 新系统（Hyperf 3.2）消费 Kafka 消息，写入新数据库
- 旧系统和新系统使用**不同的数据库**（避免互相影响）

**部署结果**：
- 旧系统写入成功率：100%（7 天观察）
- 新系统写入延迟：平均 200ms，P95 350ms（目标：< 500ms，通过）
- 数据一致性：99.98%（差异 0.02% 为网络抖动导致的 Kafka 消息丢失，已补录）

### 接口兼容层

**方案**：
- Nginx 层路由：根据 Header `X-API-Version` 路由到旧系统或新系统
- 默认路由到旧系统（v1），灰度阶段逐步切换到新系统（v2）
- 响应格式保持不变（JSON 结构、字段名、错误码）

**部署结果**：
- 旧系统调用方无需修改代码
- 灰度切换可在 30 秒内完成（Nginx reload）

### 影子流量

**方案**：
- 旧系统处理请求后，复制请求参数和响应结果到 Kafka `shadow-traffic-topic`
- 新系统消费影子流量，模拟处理，对比响应结果
- 仅对比响应结构（字段名、类型），不对比动态字段（时间戳、随机 ID）

**部署结果**：
- 影子流量运行 7 天
- 响应差异率：0.05%（差异为时间戳字段，非业务逻辑问题，通过）

---

## Phase 3: 初期迁移与损失控制

### 首批迁移模块：支付核心

**灰度计划**：
- 第 1 天：10% 流量（内部员工 + 1% 真实用户）
- 第 4 天：30% 流量（新增 10% 真实用户）
- 第 7 天：50% 流量（新增 20% 真实用户）
- 第 10 天：100% 流量（全部切换）

**实际执行**：
- 第 1-3 天：10% 灰度，无异常，P95 延迟 200ms（与旧系统持平）
- 第 4-6 天：30% 灰度，发现支付回调延迟上升到 350ms（旧系统 200ms）
- **触发 Phase 4 紧急复盘**

### 可控损失

| 损失类型 | 实际损失 | 可接受上限 | 状态 |
|---------|---------|-----------|------|
| 支付回调延迟 | 350ms（旧 200ms） | < 500ms | 🟡 接近上限 |
| 支付成功率 | 99.95%（旧 99.98%） | > 99.9% | 🟢 正常 |
| 客户投诉 | 5 起（旧 1 起/周） | < 10 起/周 | 🟢 正常 |
| 核心交易数据丢失 | 0 | 0 | 🟢 正常 |

---

## Phase 4: 里程碑复盘与路线修正（紧急复盘）

### 数据回顾

| 指标 | 旧系统 | 新系统（30% 灰度） | 历史 P95 | 状态 |
|------|--------|-------------------|----------|------|
| 故障率 | 0.01% | 0.02% | 0.015% | 🟡 接近阈值 |
| P95 延迟 | 200ms | 350ms | 250ms | 🔴 异常 |
| QPS 峰值 | 5000 | 4800 | 4500 | 🟡 接近阈值 |
| 团队压力评分 | 3.5/5 | 4.2/5 | 3.5/5 | 🔴 异常 |

### 错误识别

- [x] 无上下文架构采纳？→ **否**（微服务 3 个，团队 8 人，比例合理）
- [x] 大爆炸部署？→ **否**（灰度 10%→30%）
- [x] 重写优先综合征？→ **部分是**（团队曾提议重写支付回调模块，但 RCA 分析后发现是 MySQL 索引问题）
- [x] 分析瘫痪？→ **否**（1 周内通过数据验证解决）

### 路线修正

- 原路线：直接迁移到 Hyperf 3.2，使用新数据库连接池
- 修正后路线：
  1. 回滚支付模块到 50% 灰度（从 30% 回退，非 100% 回滚）
  2. 优化 MySQL 索引（支付回调查询缺少联合索引 `order_no + status`）
  3. 增加 Redis 缓存层（支付状态缓存 30 秒）
  4. 重新灰度到 100%，观察 3 天

### 决策确认

- 决策人：周俊仁（架构师）
- 反对意见：李四（运维）认为不需要回滚，直接优化索引即可。处理方案：保留意见，若优化后 P95 仍 > 300ms，则采纳李四方案。
- 下次复盘：2026-06-20（1 周后）

---

## Phase 5: 敏捷迭代与方案切换

### 优化后灰度

- 第 1 天（2026-06-14）：索引优化 + Redis 缓存部署，灰度 50%
- P95 延迟：180ms（优于旧系统 200ms）
- 支付成功率：99.97%（优于旧系统 99.95%）
- 客户投诉：0 起

- 第 4 天（2026-06-17）：灰度 100%
- 无异常，团队压力评分降至 3.0/5（低于历史 P95，正常）

---

## Phase 6: 核心瓶颈限时攻坚

本次迁移中未遇到需要攻坚的核心瓶颈（Phase 4 的问题通过索引优化解决，未进入攻坚阶段）。

---

## Phase 7: 技术分歧快速裁决

本次迁移中未遇到技术分歧超期的情况（Phase 4 的问题在 1 周内通过数据验证解决）。

---

## Phase 8: 多团队协作验收

### 验收结果

| 维度 | 结果 | 备注 |
|------|------|------|
| 功能完整性 | 通过 | 核心功能 100% 通过 |
| 性能指标 | 通过 | P95 180ms（旧 200ms），提升 10% |
| 数据一致性 | 通过 | 核心数据 100% 一致 |
| 监控覆盖 | 通过 | 关键路径 100% 覆盖 |
| 告警有效 | 通过 | 准确率 95%，噪音率 5% |
| 文档完整 | 通过 | 核心模块 100% 有文档 |
| 回滚可行 | 通过 | 回滚时间 30 秒（Nginx reload） |
| 合规审计 | 通过 | 100% 核心操作有审计日志 |

### 团队确认
- 后端团队负责人：王五（确认）2026-06-20
- 运维团队负责人：李四（确认）2026-06-20
- 测试团队负责人：赵六（确认）2026-06-20
- 架构师：周俊仁（确认）2026-06-20

### 技术债务清理 Sprint
- 时间：2026-06-23 至 2026-06-27（1 周）
- 安排：
  - 周一：补全支付模块 API 文档（王五）
  - 周二：优化 Redis 缓存过期时间（李四）
  - 周三：整理 Kafka DLQ 消费日志（张三）
  - 周四：团队技术分享：Hyperf 3.2 迁移经验（周俊仁）
  - 周五：代码审查：支付模块重构代码（全员）
- 下一项目启动时间：2026-06-30（第 2 轮：订单模块迁移）

---

## Phase 9: 运维治理与资产沉淀

### 监控体系
- Grafana Dashboard：`http://grafana.rastargame.internal/d/payment`
- 监控项：HTTP P95 延迟、QPS、错误率、DB 连接池、Redis 连接池、Swoole 协程数、Kafka 消费延迟

### 告警规则
- 准确率：95%（过去 2 周数据）
- 噪音率：5%
- 降噪规则：同一问题 5 分钟内只告警 1 次

### 技术资产
| 资产 | 位置 | 复用场景 |
|------|------|---------|
| 数据双写 AOP 组件 | `app/Aspect/DataSyncAspect.php` | 第 2 轮订单模块迁移 |
| 数据一致性校验脚本 | `bin/data-consistency-check.php` | 所有迁移项目的 Phase 3 |
| 灰度发布中间件 | `app/Middleware/GrayReleaseMiddleware.php` | 所有迁移项目的 Phase 5 |
| 支付模块迁移手册 | 飞书文档《支付模块迁移总结》 | 新成员入职培训 |

### 遗留问题
| 问题 | 负责人 | Deadline | 优先级 |
|------|--------|----------|--------|
| 支付对账报表延迟从 1 分钟增加到 3 分钟 | 张三 | 2026-07-07 | P1 |
| Redis 缓存未设置过期时间，存在内存泄漏风险 | 李四 | 2026-06-30 | P2 |

---

## 总结

| 指标 | 数据 |
|------|------|
| 迁移周期 | 5 周（2026-06-01 至 2026-06-20） |
| 涉及模块 | 1 个（支付核心） |
| 团队投入 | 3 人核心组（周俊仁、王五、赵六）+ 1 人维护旧系统（李四） |
| 性能提升 | P95 从 200ms → 180ms（提升 10%） |
| 数据一致性 | 100%（核心数据零丢失） |
| 客户投诉 | 5 起（灰度期间），优化后 0 起 |
| 团队成长 | 3 人掌握 Hyperf 3.2，1 人掌握 Kafka DLQ |
| 技术资产 | 4 个可复用组件/脚本/文档 |

**关键教训**：
1. 数据双写提前部署是关键，确保了迁移期间零数据丢失
2. 灰度阶段必须密切监控 P95 延迟，发现问题立即回滚
3. RCA 分析比重写更有效，索引优化比重写回调模块快 10 倍
4. 技术债务清理 Sprint 不可跳过，团队需要恢复时间


### 示例二：MySQL 5.7 → 8.0 升级（数据库迁移）

> 场景：RastarGame 数据库使用 MySQL 5.7，性能瓶颈触及天花板（单表 5000 万行，查询 P95 > 2 秒），需要升级到 MySQL 8.0。
> 
> 项目规模：3 个核心数据库（支付、订单、用户），约 2TB 数据，团队 5 人。

---

## Phase 1: 危机评估与转移决策

### 问题根因报告

| 问题 | 数据 | 根因 |
|------|------|------|
| 单表 5000 万行 | `orders` 表 5000 万行，查询 P95 > 2 秒 | 数据量增长 |
| MySQL 5.7 不支持窗口函数 | 复杂报表查询需要多次子查询，性能差 | 功能限制 |
| 索引优化已达极限 | 已添加 10 个索引，但写入性能下降 | 索引瓶颈 |
| 主从延迟 > 5 秒 | 高峰期主从延迟 5-10 秒 | 复制瓶颈 |

### 迁移策略选择

根据决策树：
- 旧系统还能运行？→ 是（但性能差）
- 技术债务是否可局部修复？→ 否（索引优化已达极限，数据量持续增长）
- **结论：渐进式迁移（Strangler Fig）**

### 迁移范围

| 数据库 | 优先级 | 迁移方式 | 说明 |
|--------|--------|---------|------|
| 支付库 | P0 | 第 1 轮 | 数据量 800GB，风险最高 |
| 订单库 | P1 | 第 2 轮 | 数据量 1TB，依赖支付库 |
| 用户库 | P1 | 第 2 轮 | 数据量 200GB，相对独立 |

---

## Phase 2: 突围准备与暗线部署

### 技术预研（PoC）

**PoC 目标**：验证 MySQL 8.0 性能提升，测试数据迁移工具。

**PoC 结果**：
- ✅ MySQL 8.0 安装成功
- ✅ 窗口函数性能提升：复杂报表查询从 2 秒降到 200ms（10 倍提升）
- ✅ 数据迁移工具测试：MySQL 8.0 的 `clone` 插件可在 4 小时内克隆 800GB 数据
- ⚠️ 字符集变化：MySQL 8.0 默认 `utf8mb4_0900_ai_ci`，与 5.7 的 `utf8mb4_general_ci` 排序规则不同，需要评估影响
- ⚠️ 密码插件变化：MySQL 8.0 默认 `caching_sha2_password`，需要更新客户端连接配置

**PoC 耗时**：5 天（2 人）

### 数据双写部署

**方案**：
- 旧系统（MySQL 5.7）写入后，通过 MySQL 5.7 的 Binlog 同步到 MySQL 8.0（使用 Canal 或 Maxwell）
- 新系统（MySQL 8.0）作为只读副本，验证数据一致性
- 双写期间，旧系统为主库，新系统为只读副本

**部署结果**：
- Binlog 同步延迟：平均 100ms，P95 500ms（目标：< 1 秒，通过）
- 数据一致性：99.99%（差异 0.01% 为 Binlog 解析延迟，已补录）
- 字符集问题：发现 3 个字段排序规则差异，已修正

### 接口兼容层

**方案**：
- 应用层通过数据库代理（如 MySQL Router）路由读写请求
- 写请求路由到 MySQL 5.7（主库）
- 读请求逐步路由到 MySQL 8.0（只读副本）

**部署结果**：
- 应用层无需修改数据库连接配置
- 切换可在 1 分钟内完成（代理层配置变更）

---

## Phase 3: 初期迁移与损失控制

### 首批迁移数据库：支付库

**灰度计划**：
- 第 1 周：读请求 10% 路由到 MySQL 8.0（非核心查询）
- 第 2 周：读请求 30% 路由到 MySQL 8.0（新增报表查询）
- 第 3 周：读请求 50% 路由到 MySQL 8.0（新增支付历史查询）
- 第 4 周：读请求 100% 路由到 MySQL 8.0
- 第 5 周：写请求切换（MySQL 8.0 提升为主库，MySQL 5.7 降为只读副本）

**实际执行**：
- 第 1-2 周：读请求 10%→30%，无异常，报表查询 P95 从 2 秒降到 200ms
- 第 3 周：读请求 50%，发现 1 个报表查询结果排序与旧系统不同（字符集差异）
- **触发 Phase 4 复盘**

### 可控损失

| 损失类型 | 实际损失 | 可接受上限 | 状态 |
|---------|---------|-----------|------|
| 报表查询排序差异 | 1 个报表结果排序不同 | < 0.1% 报表差异 | 🟡 接近上限 |
| 查询延迟 | 200ms（旧 2 秒） | < 500ms | 🟢 正常 |
| 核心交易数据丢失 | 0 | 0 | 🟢 正常 |

---

## Phase 4: 里程碑复盘与路线修正

### 数据回顾

| 指标 | 旧系统（MySQL 5.7） | 新系统（MySQL 8.0，50% 读） | 历史 P95 | 状态 |
|------|---------------------|---------------------------|----------|------|
| 报表查询 P95 | 2 秒 | 200ms | 1.5 秒 | 🟢 正常（优于历史） |
| 数据一致性 | 100% | 99.99% | 99.98% | 🟢 正常 |
| 团队压力评分 | 3.5/5 | 3.8/5 | 3.5/5 | 🟢 正常 |

### 错误识别

- [x] 无上下文架构采纳？→ **否**
- [x] 大爆炸部署？→ **否**
- [x] 重写优先综合征？→ **否**
- [x] 分析瘫痪？→ **否**

### 路线修正

- 问题：1 个报表查询结果排序与旧系统不同
- 根因：MySQL 8.0 默认排序规则 `utf8mb4_0900_ai_ci` 与 5.7 的 `utf8mb4_general_ci` 不同，导致中文排序差异
- 修正方案：
  1. 修改报表查询，显式指定排序规则 `COLLATE utf8mb4_general_ci`
  2. 或者：修改 MySQL 8.0 表结构，统一使用 `utf8mb4_general_ci`
- 决策：采用方案 2（统一排序规则），避免每个查询都显式指定
- 执行时限：2026-07-03（3 天）

---

## Phase 5: 敏捷迭代与方案切换

### 修正后灰度

- 第 1 天（2026-07-01）：统一排序规则，灰度 50% 读请求
- 报表查询结果：100% 与旧系统一致
- 无异常

- 第 4 天（2026-07-04）：读请求 100% 路由到 MySQL 8.0
- 第 7 天（2026-07-07）：写请求切换，MySQL 8.0 提升为主库
- 无异常，支付核心 P95 延迟 150ms（旧 200ms）

---

## Phase 6-9: 后续阶段

Phase 6-9 按标准流程执行，无特殊问题。

**技术债务清理 Sprint**：2026-07-10 至 2026-07-14（1 周）

**遗留问题**：
- 订单库迁移（第 2 轮）：2026-07-17 启动
- 用户库迁移（第 2 轮）：2026-07-17 启动（与订单库并行）

---

## 总结

| 指标 | 数据 |
|------|------|
| 迁移周期 | 6 周（2026-06-15 至 2026-07-07） |
| 涉及数据库 | 1 个（支付库，800GB） |
| 团队投入 | 2 人核心组 + 1 人维护旧系统 |
| 性能提升 | 报表查询 P95 从 2 秒 → 200ms（10 倍提升） |
| 数据一致性 | 99.99%（差异已补录） |
| 关键教训 | 字符集/排序规则差异是数据库迁移的隐形陷阱，必须在 PoC 阶段验证 |


### 示例三：单体支付系统 → 模块化（架构重构）

> 场景：RastarGame 支付系统是一个 5000 行的 PHP 文件，包含支付创建、支付回调、支付对账、支付退款、支付统计等所有逻辑。代码耦合严重，维护困难，需要重构为模块化架构。
> 
> 项目规模：1 个核心文件（5000 行），团队 3 人。

---

## Phase 1: 危机评估与转移决策

### 问题根因报告

| 问题 | 数据 | 根因 |
|------|------|------|
| 单文件 5000 行 | `PaymentService.php` 5000 行，包含 15 个方法 | 职责不单一 |
| 修改 1 个功能影响 5 个功能 | 平均每次 PR 影响 3-5 个无关功能 | 耦合严重 |
| 单元测试覆盖率 5% | 5000 行代码只有 250 行有测试 | 测试困难 |
| 新成员上手时间 2 周 | 需要理解全部 5000 行代码才能修改 | 知识门槛高 |

### 迁移策略选择

根据决策树：
- 旧系统还能运行？→ 是（功能正常）
- 技术债务是否可局部修复？→ 否（单文件 5000 行，局部重构无法解决根本问题）
- **结论：局部优化（Refactor-in-Place）→ 渐进式模块化**

**注意**：虽然选择了"局部优化"，但这里的"局部优化"不是加索引或调配置，而是**代码重构**。由于团队只有 3 人，不引入微服务，而是将单文件拆分为多个类文件。

### 迁移范围

| 模块 | 优先级 | 拆分方式 | 说明 |
|------|--------|---------|------|
| 支付创建 | P0 | 第 1 轮 | 核心功能，优先验证 |
| 支付回调 | P0 | 第 1 轮 | 核心功能，与支付创建并行 |
| 支付对账 | P1 | 第 2 轮 | 非核心，可延后 |
| 支付退款 | P1 | 第 2 轮 | 非核心，可延后 |
| 支付统计 | P2 | 第 3 轮 | 非核心，可延后 |

---

## Phase 2: 突围准备与暗线部署

### 技术预研（PoC）

**PoC 目标**：验证将 `PaymentService.php` 拆分为多个类文件后，功能是否正常。

**PoC 方案**：
1. 创建 `PaymentCreateService.php`，从 `PaymentService.php` 提取支付创建逻辑
2. 保持 `PaymentService.php` 不变，作为兼容层调用 `PaymentCreateService.php`
3. 运行单元测试，验证功能一致性

**PoC 结果**：
- ✅ `PaymentCreateService.php` 创建成功，200 行代码
- ✅ 单元测试通过（原有测试 + 新增测试）
- ✅ `PaymentService.php` 作为兼容层，功能不变
- ⚠️ 需要更新自动加载配置（Composer autoload）

**PoC 耗时**：2 天（1 人）

### 数据双写（代码层面）

**方案**：
- 旧代码（`PaymentService.php`）和新代码（`PaymentCreateService.php`）同时存在
- 旧代码作为兼容层，调用新代码
- 所有调用方先走旧代码，逐步迁移到直接调用新代码

**部署结果**：
- 旧代码调用成功率：100%（功能不变）
- 新代码单元测试覆盖率：80%（目标：> 70%，通过）

---

## Phase 3: 初期迁移与损失控制

### 首批迁移模块：支付创建

**灰度计划**：
- 第 1 周：1 个调用方（内部管理后台）直接调用 `PaymentCreateService.php`
- 第 2 周：3 个调用方（App、H5、管理后台）直接调用 `PaymentCreateService.php`
- 第 3 周：所有调用方迁移完成，删除 `PaymentService.php` 中的支付创建逻辑

**实际执行**：
- 第 1 周：管理后台迁移成功，无异常
- 第 2 周：App 迁移成功，H5 迁移时发现 1 个兼容问题（H5 传入的参数格式与 App 不同）
- **触发 Phase 4 复盘**

### 可控损失

| 损失类型 | 实际损失 | 可接受上限 | 状态 |
|---------|---------|-----------|------|
| H5 支付创建失败 | 1 个兼容问题，影响 1 小时 | < 2 小时 | 🟡 接近上限 |
| 核心功能异常 | 0 | 0 | 🟢 正常 |

---

## Phase 4: 里程碑复盘与路线修正

### 数据回顾

| 指标 | 旧代码 | 新代码 | 历史 P95 | 状态 |
|------|--------|--------|----------|------|
| 单元测试覆盖率 | 5% | 80% | 70% | 🟢 正常 |
| 代码复杂度 | 5000 行/文件 | 200 行/文件 | 500 行/文件 | 🟢 正常 |
| 团队压力评分 | 4.0/5 | 3.5/5 | 3.5/5 | 🟢 正常 |

### 错误识别

- [x] 无上下文架构采纳？→ **否**（未引入微服务，仅模块化拆分）
- [x] 大爆炸部署？→ **否**（逐步迁移调用方）
- [x] 重写优先综合征？→ **否**（基于 RCA，非盲目重写）
- [x] 分析瘫痪？→ **否**（1 周内解决兼容问题）

### 路线修正

- 问题：H5 传入的参数格式与 App 不同，导致兼容问题
- 根因：旧代码 `PaymentService.php` 对参数格式做了隐式转换，新代码 `PaymentCreateService.php` 没有保留这个转换
- 修正方案：
  1. 在 `PaymentCreateService.php` 中增加参数格式兼容层（支持 App 和 H5 两种格式）
  2. 增加单元测试覆盖两种参数格式
- 执行时限：2026-07-10（2 天）

---

## Phase 5-9: 后续阶段

按标准流程执行，无特殊问题。

**技术债务清理 Sprint**：2026-07-13 至 2026-07-17（1 周）

**遗留问题**：
- 支付回调模块拆分（第 2 轮）：2026-07-20 启动
- 支付对账模块拆分（第 2 轮）：2026-07-20 启动（与支付回调并行）

---

## 总结

| 指标 | 数据 |
|------|------|
| 迁移周期 | 3 周（2026-06-20 至 2026-07-10） |
| 涉及模块 | 1 个（支付创建，从 5000 行拆分为 200 行） |
| 团队投入 | 2 人 |
| 单元测试覆盖率 | 从 5% → 80% |
| 代码复杂度 | 从 5000 行/文件 → 200 行/文件 |
| 关键教训 | 代码重构时，必须保留旧代码的隐式行为（如参数转换），否则会导致兼容问题 |


---

## 十一、参考文档：长征历史时间线

> 以下内容为历史参考文档，用于理解本 Skill 的命名渊源。**无需阅读即可使用本 Skill 的技术决策流程**。

> 本文档详细记录了中国工农红军长征的全过程及重要节点事件，作为《长征工程方法论》技能包的补充参考资料。

---

## 一、战略转移决策（1934年7月-10月）

### 1.1 第五次反"围剿"失败

1933年9月，蒋介石调集100万兵力、200架飞机，对中央苏区发动第五次"围剿"。博古、李德采取"堡垒对堡垒"的阵地战方针，与装备精良的国民党军硬碰硬，导致红军损失惨重。至1934年10月，中央苏区已无法坚守，红军被迫决定战略转移。

**关键数据**：
- 国民党军兵力：约100万
- 红军兵力：约8.6万人
- 转移决策时间：1934年10月

### 1.2 突围准备

- **1934年7月**：红七军团（约6000人）组成北上抗日先遣队，向闽、浙、皖、赣边界出动，牵制国民党军，为主力转移创造条件。
- **1934年9月**：与粤军陈济棠谈判达成"借道"协议，为后续突破第一道封锁线铺路。
- **1934年10月10日**：中共中央、中革军委率中央红军主力8.6万人，从江西瑞金、于都等地出发，开始长征。

---

## 二、突破封锁线（1934年10月-12月）

### 2.1 突破第一道封锁线

**时间**：1934年10月21日-25日
**地点**：江西信丰、安远间
**战果**：红军突破国民党军第一道封锁线，向湖南进军。粤军陈济棠按"借道"协议，未全力阻击。

### 2.2 突破第二道封锁线

**时间**：1934年11月2日-8日
**地点**：湖南汝城、广东城口间
**战果**：红军突破第二道封锁线，进入湖南南部。

### 2.3 突破第三道封锁线

**时间**：1934年11月8日-15日
**地点**：湖南良田、宜章间
**战果**：红军突破第三道封锁线，向湘江推进。

### 2.4 湘江战役（第四道封锁线）——长征中最惨烈的战役

**时间**：1934年11月27日-12月1日
**地点**：广西兴安、全州间湘江两岸
**过程**：
- 蒋介石调集40万大军，在湘江两岸布下第四道封锁线，企图将红军全歼于湘江东岸。
- 红军分左右两翼强渡湘江，与国民党军展开激烈血战。
- 红三军团第5师在新圩阻击桂军，伤亡惨重，师参谋长胡震、第14团团长黄冕昌牺牲。
- 红五军团第34师担任后卫，被敌军切断退路，师长陈树湘腹部重伤被俘，断肠明志，壮烈牺牲。

**损失数据**：
- 红军从出发时的8.6万人锐减至3万余人
- 红八军团番号撤销，红五军团损失过半
- 湘江被鲜血染红，当地百姓"三年不饮湘江水，十年不食湘江鱼"

**意义**：湘江战役是长征中最惨烈的一战，血的教训使红军上下认识到"左"倾军事路线的错误，为遵义会议的召开埋下伏笔。

---

## 三、通道转兵与黎平会议（1934年12月）

### 3.1 通道会议

**时间**：1934年12月12日
**地点**：湖南通道县
**内容**：毛泽东建议放弃原定与红二、六军团会合的计划，改向敌人力量薄弱的贵州进军。博古、李德坚决反对，但周恩来、张闻天、王稼祥等支持毛泽东的意见。会议决定暂时西进占领黎平。

### 3.2 黎平会议

**时间**：1934年12月18日
**地点**：贵州黎平
**内容**：
- 中共中央政治局会议，讨论红军战略方针问题。
- 毛泽东主张继续向贵州西北进军，在川黔边建立根据地。
- 会议通过《中央政治局关于战略方针之决定》，放弃与红二、六军团会合的计划，确定向贵州北部进军。
- 这是红军战略转变的开始，为遵义会议做了准备。

### 3.3 猴场会议

**时间**：1934年12月31日-1935年1月1日
**地点**：贵州瓮安猴场
**内容**：
- 重申黎平会议决定，强调红军必须打过乌江，向遵义进军。
- 限制博古、李德的军事指挥权，规定"关于作战方针以及作战时间与地点的选择，军委必须在政治局会议上做报告"。
- 为遵义会议召开扫除了障碍。

---

## 四、遵义会议——生死攸关的转折点（1935年1月）

### 4.1 强渡乌江

**时间**：1935年1月2日-6日
**地点**：贵州余庆、瓮安间乌江江界河渡口
**过程**：红军突破乌江天险，击溃黔军，为攻占遵义打开通道。

### 4.2 攻占遵义

**时间**：1935年1月7日
**过程**：红军攻占遵义城，获得休整机会。

### 4.3 遵义会议

**时间**：1935年1月15日-17日
**地点**：贵州遵义老城枇杷桥（今遵义会议会址）
**参会人员**：
- 政治局委员：毛泽东、朱德、陈云、周恩来、张闻天、博古
- 政治局候补委员：王稼祥、邓发、刘少奇、凯丰
- 红军总部及各军团负责人：刘伯承、李富春、林彪、聂荣臻、彭德怀、杨尚昆、李卓然、邓小平、李德、伍修权（翻译）

**会议内容**：
1. **博古作报告**：强调客观困难，为第五次反"围剿"失败辩解。
2. **周恩来作副报告**：主动承担责任，批评"左"倾军事路线错误。
3. **张闻天作反报告**：系统批判"左"倾军事路线。
4. **毛泽东发言**：分析"左"倾军事路线的错误，提出正确的军事路线。
5. **王稼祥发言**：明确支持毛泽东，建议取消博古、李德军事指挥权，由毛泽东参与军事指挥。

**会议决定**：
- 增选毛泽东为中央政治局常委
- 取消博古、李德、周恩来组成的"三人团"，仍由最高军事首长朱德、周恩来为军事指挥者，周恩来为党内委托的对于指挥军事下最后决心的负责者
- 会后不久，成立由毛泽东、周恩来、王稼祥组成的三人军事指挥小组

**历史意义**：
- 结束了王明"左"倾教条主义在党中央的统治
- 确立了以毛泽东为代表的新的中央的正确领导
- 在极端危急的历史关头，挽救了党，挽救了红军，挽救了中国革命
- 是中国共产党从幼年走向成熟的标志

---

## 五、四渡赤水——运动战的巅峰（1935年1月-3月）

### 5.1 一渡赤水

**时间**：1935年1月29日
**背景**：土城战斗失利，红军主动撤出战斗
**行动**：红军从贵州土城、元厚场一带西渡赤水河，进入四川古蔺、叙永地区，向川南进军。

### 5.2 二渡赤水

**时间**：1935年2月18日-21日
**背景**：国民党军在川南集结重兵，红军北渡长江计划受阻
**行动**：红军回师东进，在太平渡、二郎滩等地二渡赤水，重入贵州，奇袭娄山关，再占遵义城。

**战果**：
- 歼灭和击溃敌军2个师又8个团
- 俘敌约3000人
- 取得长征以来最大的一次胜利

### 5.3 三渡赤水

**时间**：1935年3月16日-17日
**背景**：蒋介石亲赴贵阳督战，调集大军向遵义压来
**行动**：红军在茅台镇三渡赤水，再次进入四川古蔺、叙永地区，佯装北渡长江。

### 5.4 四渡赤水

**时间**：1935年3月21日-22日
**行动**：红军在太平渡、二郎滩等地四渡赤水，回师贵州，南渡乌江，威逼贵阳。

**战略意义**：
- 四渡赤水历时3个多月，红军在川黔滇边界机动迂回，声东击西
- 彻底打乱了蒋介石的追剿计划
- 是红军战争史上以少胜多、灵活机动的经典战例
- 被毛泽东称为"平生得意之笔"

---

## 六、巧渡金沙江（1935年5月）

**时间**：1935年5月3日-9日
**地点**：云南禄劝皎平渡
**过程**：
- 红军南渡乌江后，佯攻贵阳，调出滇军孙渡部增援。
- 红军趁云南空虚，直插云南，威逼昆明。
- 红军主力急行军，由皎平渡巧渡金沙江。
- 用7条小船，历时7天7夜，将红军主力全部渡过金沙江。

**意义**：
- 红军摆脱了几十万国民党军的围追堵截
- 取得了战略转移中具有决定意义的胜利
- 实现了遵义会议确定的北渡长江、向川西北进军的目标

---

## 七、强渡大渡河与飞夺泸定桥（1935年5月）

### 7.1 会理会议

**时间**：1935年5月12日
**地点**：四川会理城郊铁厂
**内容**：
- 政治局扩大会议，统一对遵义会议以来战略方针的认识
- 批评林彪对毛泽东指挥的不满（要求撤换毛泽东、朱德军事指挥）
- 肯定毛泽东的军事指挥，维护新的领导核心

### 7.2 彝海结盟

**时间**：1935年5月22日
**地点**：四川冕宁彝海
**过程**：
- 红军先遣队司令员刘伯承与彝族果基家支首领小叶丹在彝海畔歃血为盟。
- 彝族战士加入红军，组成"中国彝民红军支队"。
- 红军顺利通过彝族区，为强渡大渡河赢得时间。

### 7.3 强渡大渡河

**时间**：1935年5月24日-25日
**地点**：四川石棉安顺场
**过程**：
- 红一军团一师一团一营营长孙继先率17勇士强渡大渡河，攻占对岸渡口。
- 但安顺场只有一条小船，全军渡河需要一个月，而敌军追兵将至。
- 红军决定兵分两路：一路继续从安顺场渡河，另一路沿大渡河西岸北上，夺取泸定桥。

### 7.4 飞夺泸定桥

**时间**：1935年5月29日
**地点**：四川泸定
**过程**：
- 红四团（团长王开湘、政委杨成武）接到命令，必须在29日夺取泸定桥。
- 红四团一昼夜急行军240里（约120公里），按时到达泸定桥。
- 泸定桥由13根铁索组成，桥面木板已被敌军拆除，只剩光溜溜的铁索。
- 22名突击队员（连长廖大珠率领）冒着对岸火力，攀着铁索向对岸冲击。
- 三连长王友才率战士扛着木板跟进，边冲锋边铺桥。
- 经过2小时激战，红军夺取泸定桥，歼灭守敌。

**意义**：
- 飞夺泸定桥是长征中最惊心动魄的战斗之一
- 打破了蒋介石"让朱毛做石达开第二"的妄想
- 为红军北上开辟了通道

---

## 八、翻雪山（1935年6月）

### 8.1 翻越夹金山

**时间**：1935年6月12日-18日
**地点**：四川宝兴夹金山
**环境**：
- 夹金山海拔4000多米，终年积雪，空气稀薄
- 山上气候变化无常，时而晴空万里，时而暴风骤雪
- 没有道路，只有陡峭的冰雪山路

**过程**：
- 红军穿着单衣草鞋，每人带一根木棍作拐杖，艰难攀登。
- 许多战士因高原反应、严寒、饥饿倒下，再也没能起来。
- 红一方面军先头部队与红四方面军先头部队在达维镇胜利会师。

### 8.2 两河口会议

**时间**：1935年6月26日
**地点**：四川懋功两河口
**内容**：
- 政治局会议，讨论红军战略方针
- 周恩来报告：目前战略方针是"首先取得甘肃南部，创造川陕甘苏区根据地"
- 会议通过《关于一、四方面军会合后战略方针的决定》
- 确定北上建立川陕甘根据地的方针

---

## 九、过草地（1935年8月）

### 9.1 毛儿盖会议

**时间**：1935年8月20日
**地点**：四川松潘毛儿盖
**内容**：
- 政治局会议，重申北上方针
- 批评张国焘南下主张
- 决定红军分左、右两路军北上

### 9.2 穿越松潘草地

**时间**：1935年8月21日-27日（右路军）；8月底-9月初（左路军部分）
**地点**：四川松潘草地（今若尔盖草原）
**环境**：
- 草地面积约15200平方公里，海拔3500米以上
- 沼泽密布，泥潭暗藏，表面是草，下面是深不可测的淤泥
- 没有道路，没有人家，没有粮食
- 天气恶劣，风雨、冰雹、严寒交替

**过程**：
- 红军战士以野菜、草根、皮带充饥，甚至煮食战友的遗体（极端情况下）。
- 许多战士陷入沼泽，来不及救援就被吞没。
- 夜晚露宿草地，战士们背靠背坐着取暖，有的战士睡着后再也没有醒来。
- 红一方面军（右路军）历时6天6夜，终于走出草地，到达班佑、巴西地区。

**损失**：
- 过草地是长征中最艰苦的一段路程
- 红一方面军损失数千人
- 红四方面军三过草地，损失更为惨重

---

## 十、分裂与整合（1935年9月）

### 10.1 巴西会议

**时间**：1935年9月2日-9日
**地点**：四川若尔盖巴西
**背景**：
- 张国焘拒绝北上，主张南下，并要挟右路军南下。
- 张国焘电令陈昌浩率右路军南下，企图以武力要挟中央。

**会议内容**：
- 9月2日：政治局会议，讨论红一方面军工作
- 9月9日：中央召开紧急会议（巴西会议），决定率红一、三军团和军委纵队先行北上，脱离危险区域。
- 发布《中央为执行北上方针告同志书》，重申北上方针。

### 10.2 俄界会议

**时间**：1935年9月12日
**地点**：甘肃迭部俄界（今高吉村）
**内容**：
- 政治局扩大会议，讨论张国焘分裂错误及部队整编问题。
- 通过《关于张国焘同志的错误的决定》，指出张国焘南下主张是"机会主义"和"军阀主义"表现。
- 决定将红一方面军主力和军委纵队改编为**中国工农红军陕甘支队**，彭德怀任司令员，毛泽东任政治委员。
- 支队下辖三个纵队，共约8000人。

---

## 十一、胜利到达陕北（1935年10月）

### 11.1 突破天险腊子口

**时间**：1935年9月17日
**地点**：甘肃迭部腊子口
**过程**：
- 腊子口是川西北进入甘肃的唯一通道，地势险要，易守难攻。
- 红四团（团长王开湘、政委杨成武）担任主攻。
- 战士"云贵川"（苗族，真名不详）攀崖而上，从侧翼迂回，配合正面突击。
- 红军突破腊子口，打开北上通道。

### 11.2 哈达铺整编

**时间**：1935年9月20日
**地点**：甘肃岷县哈达铺
**内容**：
- 红军在哈达铺休整，从报纸上得知陕北有红军根据地（刘志丹、谢子长创建的陕甘根据地）。
- 毛泽东在团以上干部会议上宣布："到陕北去！"
- 红军进行整编，将陕甘支队编为三个纵队。

### 11.3 翻越六盘山

**时间**：1935年10月7日
**地点**：宁夏固原六盘山
**过程**：
- 红军翻越六盘山，这是红军长征中翻越的最后一座大山。
- 毛泽东写下《清平乐·六盘山》："天高云淡，望断南飞雁。不到长城非好汉，屈指行程二万。"

### 11.4 吴起镇会师

**时间**：1935年10月19日
**地点**：陕西吴起镇
**过程**：
- 中央红军（陕甘支队）到达陕北吴起镇，与陕北红军（红十五军团）会师。
- 中央红军长征历时1年，行程约二万五千里，从8.6万人减少到约7000人。
- 毛泽东在吴起镇召开干部会议，总结长征，宣布长征胜利结束。

### 11.5 直罗镇战役

**时间**：1935年11月20日-24日
**地点**：陕西富县直罗镇
**过程**：
- 红军在直罗镇歼灭国民党军第109师，俘敌5300余人。
- 彻底粉碎了国民党军对陕甘根据地的第三次"围剿"。
- 为党中央把全国革命大本营放在西北举行了"奠基礼"。

---

## 十二、红二、四方面军长征（1935年11月-1936年10月）

### 12.1 红二、六军团开始长征

**时间**：1935年11月19日
**地点**：湖南桑植
**过程**：
- 红二、六军团（后合编为红二方面军）从湖南桑植出发，开始长征。
- 转战湖南、贵州、云南、四川、甘肃等省，行程约2万里。
- 1936年7月，与红四方面军在四川甘孜会师，红二、六军团合编为红二方面军，贺龙任总指挥，任弼时任政治委员。

### 12.2 红四方面军南下与再次北上

**时间**：1935年9月-1936年10月
**过程**：
- 1935年9月，张国焘率红四方面军及红一方面军部分南下，在川康边建立根据地。
- 南下红军在天全、芦山、名山地区与国民党军激战，损失惨重，由8万余人减至4万余人。
- 1936年2月，红四方面军向甘孜地区转移。
- 1936年6月，与红二方面军在甘孜会师。
- 在朱德、任弼时、贺龙、关向应等坚持下，以及红四方面军广大指战员要求下，张国焘被迫放弃南下主张，同意北上。

### 12.3 甘孜会师

**时间**：1936年6月-7月
**地点**：四川甘孜
**过程**：
- 红二、四方面军在甘孜会师。
- 红二、六军团合编为红二方面军。
- 两军共同北上，穿越草地，向甘肃南部进军。

---

## 十三、三大主力会师——长征胜利结束（1936年10月）

### 13.1 会宁大会师

**时间**：1936年10月10日
**地点**：甘肃会宁
**过程**：
- 红一、二、四方面军在甘肃会宁地区胜利会师。
- 中共中央、中华苏维埃中央政府、中央革命军事委员会发来贺电，祝贺三大主力会师。
- 红军长征胜利结束。

### 13.2 将台堡会师

**时间**：1936年10月22日
**地点**：宁夏西吉将台堡
**过程**：
- 红一、二方面军在将台堡会师。
- 至此，红军三大主力全部完成会师。

---

## 十四、长征数据统计

| 项目 | 数据 |
|------|------|
| **开始时间** | 1934年10月10日（中央红军） |
| **结束时间** | 1936年10月22日（三大主力会师） |
| **历时** | 约2年 |
| **行程** | 约二万五千里（中央红军） |
| **出发兵力** | 约8.6万人（中央红军） |
| **到达兵力** | 约7000人（中央红军到达陕北时） |
| **途经省份** | 江西、福建、广东、湖南、广西、贵州、云南、四川、西康、甘肃、陕西等11个省 |
| **翻越山脉** | 18座（其中5座终年积雪） |
| **渡过河流** | 24条 |
| **进行战役战斗** | 600余次 |
| **攻占县城** | 700余座 |

---

## 十五、长征中的主要会议

| 会议 | 时间 | 地点 | 主要内容 |
|------|------|------|---------|
| 通道会议 | 1934.12.12 | 湖南通道 | 初步改变行军方向，向贵州进军 |
| 黎平会议 | 1934.12.18 | 贵州黎平 | 确定向贵州北部进军，放弃与红二、六军团会合 |
| 猴场会议 | 1934.12.31 | 贵州猴场 | 限制博古、李德军事指挥权 |
| 遵义会议 | 1935.1.15-17 | 贵州遵义 | 确立毛泽东领导地位，生死攸关的转折点 |
| 扎西会议 | 1935.2.5-9 | 云南扎西 | 讨论红军战略方针，进行部队缩编 |
| 会理会议 | 1935.5.12 | 四川会理 | 统一战略方针认识，批评林彪 |
| 两河口会议 | 1935.6.26 | 四川两河口 | 确定北上建立川陕甘根据地 |
| 沙窝会议 | 1935.8.4-6 | 四川松潘沙窝 | 讨论一、四方面军会合后的形势与任务 |
| 毛儿盖会议 | 1935.8.20 | 四川毛儿盖 | 重申北上方针，决定分左右两路军北上 |
| 巴西会议 | 1935.9.2-9 | 四川巴西 | 决定率红一、三军团先行北上 |
| 俄界会议 | 1935.9.12 | 甘肃俄界 | 批判张国焘错误，改编陕甘支队 |

---

## 十六、长征中的英雄群体与个人

### 英雄群体
- **17勇士**：强渡大渡河的红一军团一师一团一营突击队
- **22勇士**：飞夺泸定桥的红四团突击队
- **红34师**：湘江战役后卫部队，几乎全部牺牲
- **红四团**：长征中的先锋团，突破乌江、夺取遵义、飞夺泸定桥、突破腊子口

### 英雄个人
- **陈树湘**：红34师师长，湘江战役腹部重伤被俘，断肠明志，壮烈牺牲
- **孙继先**：红一营营长，率17勇士强渡大渡河
- **廖大珠**：红四团2连连长，率22勇士飞夺泸定桥
- **"云贵川"**：苗族战士，攀崖迂回突破腊子口
- **小叶丹**：彝族果基家支首领，与刘伯承彝海结盟

---

## 十七、长征的历史意义

1. **保存了革命火种**：在国民党军的围追堵截下，红军主力得以保存，为后来的抗日战争和解放战争保留了骨干力量。

2. **确立了正确领导**：遵义会议确立了毛泽东在党中央和红军的领导地位，使中国共产党走向成熟。

3. **传播了革命思想**：红军途经11个省，宣传了共产党的政治主张，播下了革命的种子。

4. **铸就了长征精神**：
   - 把全国人民和中华民族的根本利益看得高于一切，坚定革命的理想和信念，坚信正义事业必然胜利的精神
   - 为了救国救民，不怕任何艰难险阻，不惜付出一切牺牲的精神
   - 坚持独立自主、实事求是，一切从实际出发的精神
   - 顾全大局、严守纪律、紧密团结的精神
   - 紧紧依靠人民群众，同人民群众生死相依、患难与共、艰苦奋斗的精神

---

> **"长征是宣言书，长征是宣传队，长征是播种机。"** ——毛泽东
> 
> **"长征是以我们胜利、敌人失败的结果而告结束。"** ——毛泽东

---

## 十二、版本记录

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2026-07-03 | 初始版本 |
| 1.1.0 | 2026-07-03 | 增加技术实现示例、通过/失败标准、合规审计 |
| 1.2.0 | 2026-07-03 | 拆分文件按需加载、修复代码示例（伪代码+免责声明）、阈值动态校准 |
| 1.3.0 | 2026-07-03 | 增加文件加载路由表、Phase 状态机、多轮迁移循环、示例场景索引、监控建设指南 |
| 1.3.0-full | 2026-07-03 | 单文件合并版，包含所有 Phase、模板、示例、监控指南和参考文档 |

---

> **在工程迁移中，没有完美的路线图，只有活着到达目的地的队伍。**
