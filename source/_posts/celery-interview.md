---
title: Celery 高频面试题（含详细答案）
date: 2025-07-01
tags: ['Celery', '异步任务', '消息队列']
categories: ['技术面试']
---

### 什么是 Celery？适合什么场景？

Celery 是基于 Python 的分布式任务队列系统，主要用于处理异步任务、定时任务等。适用于发送邮件、视频转码、数据同步等不要求实时响应的场景。

### Celery 架构由哪些核心组件组成？

包括：Broker（消息队列，如 Redis、RabbitMQ）、Worker（任务执行进程）、Task（定义任务逻辑）、Result Backend（存储任务结果）。

### Celery 支持哪些消息中间件？

支持 Redis、RabbitMQ、Amazon SQS、MongoDB、Kafka（实验性）等作为 broker。

### 如何在 Django 中集成 Celery？

创建 `celery.py`，在 `__init__.py` 中加载 app，配置 broker 和 backend，使用 `@shared_task` 装饰器定义任务，启动 worker 即可。

### 如何为任务设置自动重试？

可使用 `autoretry_for` 参数，也可以在任务内部手动调用 `self.retry()` 实现，支持设置最大重试次数、回退时间等。

### 如何配置定时任务？

使用 Celery Beat，结合 crontab 或 interval 来调度周期性任务。Beat 会将任务发送给 broker，由 worker 执行。

### Celery 的任务序列化方式有哪些？

支持 JSON（默认）、pickle、msgpack、yaml 等。建议使用 JSON，因其通用性和安全性更好。

### Celery 的任务优先级如何设置？

通过设置不同队列并配置队列权重或 worker 路由策略。支持 `priority` 参数（取决于 broker）。

### 如何监控 Celery 运行状态？

使用 Flower 监控 WebUI，或者配置 Prometheus 导出任务状态、失败率、执行时间等指标。

### Celery 中如何避免任务重复执行？

可以使用 Redis 锁机制、幂等处理、数据库状态记录等方式避免重复执行。