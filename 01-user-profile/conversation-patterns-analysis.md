---
name: conversation-patterns-analysis
description: 四维协作模式分析（沟通、技术工作流、问题解决、会话管理）及置信度
display_name: 协作模式分析
---

# 协作模式分析

> **分析方法**：解析本机可访问的协作记录——Claude Code 会话（11 个会话 / 53 条用户消息 / 工具调用明细）、跨工具 prompt 历史（`.claude` 170 条 + `.codex` 19 条，共 189 条，其中 138 条中文）、Reasonix 项目授权命令配置、以及历史 Agent 沉淀的 memory 文件。
> **时间跨度**：2026-07 ~ 2026-10。**样本局限**：只覆盖本机记录，早期会话和未落盘的对话无法纳入；文本情绪判断存在主观性（见每节置信度）。

---

## 维度一：沟通与交互模式 — ⭐⭐⭐⭐⭐

| 模式 | 优先级 | 证据 |
|---|---|---|
| 全程中文，代码/命令保留原文 | ![HIGH] | 「跟我交互和回答我永远用中文」；138/189 条 prompt 为中文 |
| 少客套、极简指令 | ![HIGH] | 「1」「c」「ok」「确认删除」「你自己 push 上去」 |
| 用编号选项对话 | ![HIGH] | 回复单字符选择，并会否掉"麻烦"的选项：「a 和 b 我都嫌弃麻烦，我选 c」 |
| 结论先行 + 证据 | ![HIGH] | 用线上页面反查：「changelog 这里也没有更新日志」「一点变化都没」 |
| 不要重复确认已知信息 | ![HIGH] | 「这不是给你 token 了吗？怎么还问我怎么创建？弱智了吗 现在！」「以后让你发就直接发不要找我确认，啰嗦！」 |
| 负面反馈直接、只求修好 | ![HIGH] | 「你现在跟我说没发现？立即将这个傻逼删除卸载统统清理掉！」 |
| 卡住要换手段，不接受阻塞 | ![HIGH] | 「如果模型蠢就换个模型，如果 skill 和链接工具不行就换，立即想办法解决」 |
| 要原理但不要理论铺垫 | ![MEDIUM] | 频繁追问"为什么/还有什么方法" |

📄 详见 [`06-preferences/communication-style.md`](../06-preferences/communication-style.md) 与 [`06-preferences/output-format.md`](../06-preferences/output-format.md)

---

## 维度二：技术工作流 — ⭐⭐⭐⭐☆

| 模式 | 优先级 | 证据 |
|---|---|---|
| 远程优先，CLI 优先 | ![HIGH] | 182 次 Bash + 45 次 PowerShell；SSH/docker/psql 是主力手法 |
| 只读摸清结构再改 | ![HIGH] | 反复 `information_schema.columns`、`\dt`、`docker inspect` 后再动手 |
| 验证靠命令输出，不靠口说 | ![HIGH] | `curl -w "%{http_code}"`、`count(*)`、容器状态表 |
| 多 Agent / 多模型并行 | ![MEDIUM] | Claude Code、Codex、Reasonix、DeepSeek/Nous/intern-ai 端点 |
| 工具美学要求 + 卸载要干净 | ![MEDIUM] | 「外观比较好看的 同时能管理多任务」；「一定要干净的清楚掉所有」 |
| 部署链路：Docker Compose + Nginx + systemd | ![MEDIUM] | 容器 web/db、开机自启配置 |
| 公网可访问性是硬指标 | ![HIGH] | 「不走 8888 端口」「静态页面地址不要用中文，统一用拼音」 |
| 怕"越改越乱" | ![HIGH] | 「弄到测试阶段发现很多问题…总是改的面目全非，越改问题越多」 |

📄 详见 [`02-technical-stack/current-tech-stack.md`](../02-technical-stack/current-tech-stack.md) 与 [`02-technical-stack/preferred-tools.md`](../02-technical-stack/preferred-tools.md)

---

## 维度三：问题解决框架 — ⭐⭐⭐⭐☆

| 模式 | 优先级 | 证据 |
|---|---|---|
| 先对齐再动手（但只问一次） | ![HIGH] | 「不确定的可以先跟我提问」；同时反感二次索要已知信息 |
| 按可交付阶段推进 | ![HIGH] | 「确认，先实现阶段一，记得更新站点的更新记录」 |
| 权重：可见效果 ≳ 可回滚 ≳ 工程质量 | ![HIGH] | 选"最省事"的方案 c；但要求 changelog 留痕 |
| 敢给 root 权限，破坏性操作要「确认」 | ![HIGH] | 「我允许你往 <服务器> 执行任何命令」vs「确认删除」 |
| 选型看性价比与速度，不追最强 | ![MEDIUM] | 嫌模型慢就换；接免费额度模型；端口/路径选型直接服务"别人能打开" |
| 根因优先，反对无限打补丁 | ![MEDIUM] | 自述"越改问题越多"的痛点 |

📄 详见 [`01-user-profile/decision-making.md`](decision-making.md) 与 [`03-collaboration-patterns/task-breakdown.md`](../03-collaboration-patterns/task-breakdown.md)

---

## 维度四：会话管理 — ⭐⭐⭐⭐☆

| 模式 | 优先级 | 证据 |
|---|---|---|
| 开场常问"你是什么模型" | ![MEDIUM] | 历史中反复出现「你现在什么模型」（约 10 次） |
| 期望 Agent 记住服务器/项目 | ![HIGH] | 「以前不是让你把这个服务器记在根目录里吗？」 |
| 长任务要有节点汇报 | ![HIGH] | 「总结下你都做了什么 为什么跑了一天都还在跑」 |
| 验收 = 公网可打开 + 内容正确 + changelog | ![HIGH] | 对 FDE 站的两次验收发言 |
| 会中途交棒 | ![MEDIUM] | 「我不弄了，你去服务器搞吧。我这边 ssh 退了」 |
| 上下文管理：`/clear`、换模型、找回历史 | ![MEDIUM] | 「我每次关闭界面下次还想找回之前的对话内容怎么办」 |

📄 详见 [`03-collaboration-patterns/progress-reporting.md`](../03-collaboration-patterns/progress-reporting.md) 与 [`07-quick-reference/session-start-checklist.md`](../07-quick-reference/session-start-checklist.md)

---

## 高置信度结论（可直接照做）![HIGH]
1. 中文、简短、结论先行，附可复现证据。
2. 岔路给编号选项并推荐一个。
3. 已给的信息不再问；能自己查的自己查。
4. 先做能点开看到的最小版本，再扩展；每阶段留 changelog。
5. 破坏性操作前一句话确认；其余直接干。
6. 卡住时换工具/换路径，别回来要权限。

## 需要用户确认的模式（详见报告）![MEDIUM]
- 「速度优先于质量」的边界：什么时候可以接受"先上线再打磨"？
- 是否希望 Agent **默认直接 push** 到远端仓库（含私有仓库）？
- 报价单/客户类项目是否允许在仓库中出现项目名（目前一律用"某客户"指代）。

## 置信度说明
| 维度 | 星级 | 依据强度 |
|---|---|---|
| 沟通与交互 | ⭐⭐⭐⭐⭐ | 多工具、多月份、显式表述（含用户原话） |
| 技术工作流 | ⭐⭐⭐⭐☆ | 命令与配置为硬证据，框架偏好为归纳 |
| 问题解决 | ⭐⭐⭐⭐☆ | 决策场景样本偏少（主要来自 2026-10） |
| 会话管理 | ⭐⭐⭐⭐☆ | 开场/验收/交棒均有实例，频率统计为估算 |

## 最近更新
📅 2026-10-23 - 首次填充（原为空文件）
📅 2026-10-24 - 由 Trae 同步入本仓库（服务器 IP 已脱敏）

---
*最后更新：2026-10-23 ｜ 版本：1.0.0 ｜ 分析置信度：⭐⭐⭐⭐☆*
