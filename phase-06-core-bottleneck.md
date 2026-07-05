# Phase 6: 核心瓶颈限时攻坚

## 目标
集中最强资源，在明确 deadline 内解决核心性能瓶颈或关键 Bug。

## 通过标准
- [ ] 核心问题已明确（如：支付回调处理延迟 > 历史 P95 × 2）
- [ ] 攻坚小组已组建（1 架构师 + 1 核心开发 + 1 测试，可选）
- [ ] Deadline 已设定（技术评估时间 × 1.5 倍缓冲，最长不超过 2 周）
- [ ] 攻坚期间，小组成员不参与其他项目，日常维护由"牵制部队"承担（牵制部队定义见 Phase 2）
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

**阈值计算前提条件（必须满足，否则使用 Fallback）**：

| 前提条件 | 要求 | 不满足时的 Fallback |
|---------|------|-------------------|
| **样本量** | >= 1000 个数据点 | 使用行业经验值作为临时阈值，同时增加采样频率 |
| **时间窗口** | >= 2 周 | 使用固定阈值（见下方"无历史数据时的临时阈值"），持续积累数据后替换 |
| **数据稳定性** | 排除异常时段（促销、DDoS、版本发布） | 手动标注异常时段后重新计算 |
| **P95 = 0 或不存在** | 历史从未触发过该指标 | 使用行业经验值作为初始阈值，积累 1000+ 样本后替换 |

**无历史数据时的临时阈值（行业经验值）**：

| 指标 | 临时阈值 | 说明 |
|------|---------|------|
| HTTP P95 延迟 | < 500ms | 游戏内购接口建议 < 200ms，后台接口可放宽到 < 1000ms |
| HTTP 错误率 | < 0.1% | 核心交易接口 < 0.01%，非核心接口 < 0.5% |
| DB 连接池使用率 | < 80% | 超过 80% 可能出现连接等待 |
| Redis 连接池使用率 | < 80% | 同上 |
| Swoole 协程数 | < 10000 | 超过 10000 可能内存不足 |
| Kafka 消费延迟 | < 1000 条 | 超过 1000 条可能消费能力不足 |
| 团队压力评分 | < 3/5 | 低于 3/5 表示 burnout 风险 |

**临时阈值使用规则**：
1. 使用临时阈值启动监控/迁移
2. 同时启动数据收集（见 [monitoring-setup.md](monitoring-setup.md)）
3. 积累 1000+ 样本且时间窗口 >= 2 周后，替换为历史 P95
4. 每周重新校准，持续优化


| 指标 | 校准方法 |
|------|----------|
| 核心问题判定 | 性能指标 > 历史 P95 × 2，或故障率 > 历史 P95 × 3 |
| Deadline 缓冲 | 技术评估时间 × 1.5（基于历史攻坚项目的实际耗时/预估耗时比率） |
| 性能提升目标 | 优化后 P95 < 优化前 P95 × 0.5（即提升 50%） |
| 新 Bug 容忍 | 其他模块故障率 < 历史 P95 |
