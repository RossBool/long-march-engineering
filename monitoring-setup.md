# 监控体系建设指南（Monitoring Setup Guide）

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
|------|------|---------|----------|
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
