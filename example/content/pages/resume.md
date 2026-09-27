---
id: 01KPM2ZN00QXWMADC6KWCF5A19
title: 简历
slug: resume
status: published
created_at: 2026-04-20T08:00:00+08:00
published_at: 2026-04-20T08:00:00+08:00
layout: resume
description: Your Name · 后端开发工程师
comments: false
profile:
  name: Your Name
  intent: 后端开发工程师
  status: 目前在职
  meta: [杭州, 3 年经验]
  contact:
    - {icon: mail, text: you@example.com, url: "mailto:you@example.com"}
    - {icon: github, text: github.com/yourname, url: "https://github.com/yourname"}
  stack: [Go, MySQL, Redis, Kafka, Docker, Kubernetes]
skills:
  - label: Go
    text: 熟悉 GMP 调度、GC 与逃逸分析，能用 Goroutine 与 Channel 写出可控的并发。
  - label: 数据库与缓存
    text: 熟悉 MySQL 的索引、事务与锁，能用 Redis 做分布式锁和缓存一致性。
  - label: 云原生
    text: 能写 Dockerfile 与 Compose，了解 Kubernetes 的 Pod、Deployment 与 Service。
experience:
  - org: 某某科技有限公司
    role: 后端开发工程师
    date: 2024.07 – 至今
    tags: [Go, Gin, Redis, Kafka]
    items:
      - lead: 计费系统
        text: 设计按用量计费的扣费链路，用分布式锁和幂等键杜绝重复扣费。
      - lead: 异步化
        text: 把非核心链路改为消息驱动，高峰期网关的延迟降到原来的一半。
projects:
  - name: almanac
    url: https://github.com/kite-plus/theme-almanac
    desc: 极简、内容向的个人站点主题
    tags: [Kite, Tailwind CSS]
    items:
      - text: 杂志风刊头、混合卡片 feed，项目、书影、动态、友链与简历一应俱全。
education:
  - school: 某某大学
    date: 2019 – 2023
    major: 计算机科学与技术 · 本科
summary:
  - text: 喜欢把复杂的系统拆成能讲清楚的小块，也喜欢把讲清楚的东西写下来。
links:
  - {icon: github, label: GitHub, text: yourname, url: "https://github.com/yourname"}
---
