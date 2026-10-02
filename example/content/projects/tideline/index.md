---
id: 01KNDF0W00V37E3S3E28JT97KB
slug: tideline
status: published
created_at: 2026-04-05T08:00:00+08:00
published_at: 2026-04-05T08:00:00+08:00
title: tideline
weight: 2
description: 一个把 RSS / 邮件订阅 / Twitter list 揉到一起的「信息潮汐」客户端，用半天读完一天的输入。
icon: icon.svg
cover: cover.svg
state: active
tech: [Go, SQLite, Vue]
repo: https://github.com/yourname/tideline
homepage: https://tideline.dev
---
## 简介

订阅源越多越焦虑，但全砍了又怕错过有价值的内容。tideline 试图换一个交互范式：每天只在固定时段拉一次（涨潮），其它时间无通知（退潮）。

把「持续在线」变回「每日刷新」。

## 主要能力

- **定时涨潮** —— 每天在你选的时间把所有来源拉一遍，其它时间不打扰
- **合并去重** —— 同一条新闻从三个来源进来，只留一条
- **读完即走** —— 一天的输入有一个明确的终点
