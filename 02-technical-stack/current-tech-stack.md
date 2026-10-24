# 当前技术栈

> 证据来源：2026-07 ~ 2026-10 协作记录。版本信息以实际环境为准，变更时请更新确认日期。

## 运行环境

- Windows 11 Pro
- PowerShell 为主，Git Bash 为辅
- VS Code、Windows Terminal、Obsidian

## AI 协作

- 主力：Claude Code
- 其他：Codex、Reasonix
- 自接兼容网关：DeepSeek / Nous / intern-ai，使用 `switch-model.py` 切换

## 部署

- Docker Compose（web + db）
- Nginx 反向代理
- systemd 开机自启

## 数据

- PostgreSQL（容器内）

## 约定

- 公网优先 80/443，避开非常规端口。
- 静态页路径与 URL 用拼音 slug，不用中文。
- 每轮修复只解决一类问题，改动保持幂等、可回滚。

## 最近更新
📅 2026-10-24 - 依据跨 Agent 分析报告首次成文

---
*版本：1.0.0*
