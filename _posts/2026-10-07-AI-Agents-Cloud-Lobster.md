---
layout: post
title: "AI 进入“云端养龙虾”时代：从 Meta Muse 到 OpenAI Dots"
categories: [Thinking]
tags: [AI, Agent, Meta, OpenAI, OpenClaw]
description: 一个月之内，Meta 和 OpenAI 先后把“每人一台云电脑 + 一个常驻 Agent”做成了产品
author: "Vincent"
header-img: "img/hacker.png"
permalink: /:categories/:year/:month/:day/:title.html
---

最近 AI 的发展速度，快到让人有点跟不上。

年初大家还在折腾 OpenClaw，自己租台云主机、配 API Key、装 Skills，网友管这叫 **“养龙虾”**。才过了半年多，Meta 和 OpenAI 已经把这件事直接做成了消费级产品：**你不用自己养了，它们替你养。**

## 一个月，两只“大龙虾”

**9 月 8 日，Meta 发布 Muse。**

Muse 是 Meta 的个人 AI Agent，定位是帮你跑腿：订东西、管日程、处理邮件、执行多步骤任务。关键点在于它的运行方式——每个用户分配一台独立的云虚拟机（Meta 叫它 Muse Secure VM），Agent、它用的浏览器、你的各种登录凭证，都隔离在这台机器里。浏览器的操作过程对用户是可见的。

定价上，Meta 照旧走“先免费再收费”的路子：免费档每周 1 亿 token，付费档 20 美元和 100 美元每月。入口覆盖网页、iOS/Android App，还有 WhatsApp。

**9 月 29 日，OpenAI 在 DevDay 上发布 Dots。**

Dots 是 “always-on” 的常驻 Agent，跑在 9 月初刚发布的 GPT-6 Astra 上。同样，每个 dot 都有**自己的云电脑和浏览器**，可以接入 4000 多个应用插件，同时推进多个项目。你可以在 ChatGPT 里给它发消息、甚至打电话，也可以在 Slack 和 Teams 里直接 @ 它，像对待一个同事。

首个 dot 包含在 Pro 和 Business Premium 订阅里。

## 本质上是同一个模式

把两个产品放在一起看，会发现它们的架构几乎一样：

| 维度 | 自己养龙虾 (OpenClaw) | Meta Muse | OpenAI Dots |
|------|----------------------|-----------|-------------|
| 运行环境 | 自己租的云主机 / 本地 | 每人一台 Muse Secure VM | 每个 dot 一台云电脑 |
| 模型 | 自己选，自己付 API 费 | Meta 自研新一代模型 | GPT-6 Astra |
| 工作方式 | 常驻、主动执行 | 常驻、代办任务 | 常驻、多项目并行 |
| 运维 | 自己负责 | 平台托管 | 平台托管 |
| 门槛 | 要懂 Linux、配置、安全 | 下载 App 就能用 | 订阅 ChatGPT 即可 |

一句话总结：**AI 从“你问我答”的聊天框，变成了“一台 7×24 小时在线、替你干活的云主机”。**

这正是 OpenClaw 社区半年前摸索出来的模式——给 Agent 一台属于它自己的机器，让它有长期记忆、能主动执行、能操作浏览器和文件。只不过当时需要自己动手，现在巨头把它包装成了一键开通的服务。

开源社区趟路，大厂收割产品化，这个剧本在技术圈并不陌生。

## 从 SRE 的角度看：这是一个基础设施问题

作为做数据库运维的人，我看这件事的第一反应不是“好酷”，而是“这得多少机器”。

**1. 算力模型变了**

以前的 Chatbot 是无状态请求，用完即走，资源可以高度复用。现在每个用户、甚至每个 Agent 都要一台常驻 VM。几亿用户规模下，这不是推理成本的问题，而是**整个云基础设施的容量规划问题**。难怪 token 成了新的计费单位，而且各家都在定“每周额度”。

**2. 隔离和凭证管理是核心**

Agent 要替你登录邮箱、付款、下单，凭证必须放在某个地方。Muse 强调 “Secure VM”，本质就是用虚拟机级别的隔离来控制爆炸半径（blast radius）。这和我们做数据库多租户隔离的思路是一样的：**宁可多花资源，也不能让一个租户的问题波及其他人。**

**3. 可观测性决定信任**

Muse 让用户能看到 Agent 浏览器里在干什么，这一点我很认同。生产环境里我们不敢用一个没有日志、没有监控的自动化脚本去做 failover；同理，普通人也不会放心把信用卡交给一个“黑盒”。**能被观察，才能被信任。**

**4. 自己养 vs 托管，又是那道老题**

这其实和数据库领域“自建 MySQL vs 用 RDS”是同一个问题：

- **自己养龙虾**：灵活、可控、数据在自己手里，但要自己负责升级、安全、成本
- **托管 Agent**：省心、开箱即用，但被平台锁定，数据和行为都在别人的机器上

对大多数普通用户，托管一定是主流；对工程师和重度用户，自己养一只龙虾仍然有它的价值——至少你知道它在干什么。

## 写在最后

回头看 2026 年，AI 的节奏大概是这样的：

- **上半年**：开源 Agent 爆火，大家在自己的云主机上“养龙虾”
- **下半年**：Meta、OpenAI 先后入场，把“云端常驻 Agent”变成了标配

接下来，Google、Anthropic、苹果大概率也会给出自己的答案。可以预见，“每个人拥有一个跑在云端的数字分身”会很快从新鲜事变成日常。

对我们这些做基础设施的人来说，这既是挑战，也是机会：Agent 越多，背后的数据库、存储、网络、调度系统就越重要。**龙虾再聪明，也得有人给它修池子。**

---

*参考资料：*

- [Axios: Meta debuts Muse, its long-planned personal AI agent](https://www.axios.com/2026/09/08/meta-debuts-muse-personal-ai-agent)
- [MarkTechPost: Meta Introduces Muse, a Personal AI Agent That Runs on Its Own Dedicated Secure Cloud Computer](https://www.marktechpost.com/2026/09/08/meta-introduces-muse-a-personal-ai-agent-that-runs-on-its-own-dedicated-secure-cloud-computer/)
- [TechCrunch: OpenAI launches Dots](https://techcrunch.com/2026/09/29/openai-launches-dots-its-bubbly-agentic-avatar/)
- [The Next Web: OpenAI launches dots, always-on AI agents with their own cloud computers](https://thenextweb.com/news/openai-dots-always-on-ai-agents-cloud-computers-devday)
- [Wikipedia: OpenClaw](https://en.wikipedia.org/wiki/OpenClaw)
