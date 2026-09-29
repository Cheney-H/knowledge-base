---
project: second-brain
doc: overview
status: active
updated: 2026-09-29
tags: [知识管理, PKM, 元项目]
---

# Cheney 的第二大脑 · 项目总览

## 这是什么
在 `knowledge-base` 仓库上搭建的个人知识管理系统（"第二大脑"），融合《打造第二大脑》(CODE/PARA) 与卡帕西 llm-wiki 知识复利理论。是管理其他所有项目的"元项目"。

## 目标
- 项目经验、决策、过程结构化留存，换环境/换 Agent 后可秒读接续
- 跨项目经验复利：通用经验提炼成独立、互链、可追溯的知识资产
- 工具无关：纯 Markdown + Git，任意 Agent（Ducc/Codex）可接管

## 边界 / 非目标
- 仅覆盖工作/项目，不涵盖个人生活（健康/财务等 PARA Areas）
- 不建 `raw/` 外部资料囤积层
- 不引入 Obsidian 插件写入或双向同步；Obsidian 仅作人类只读导航层

## 技术栈
- 存储：纯 Markdown + YAML frontmatter
- 版本/权威：Git + GitHub 私有仓库（Cheney-H/knowledge-base）
- 写入/维护：Ducc（或任意 Agent）
- 人类阅读：Obsidian（双链 + 图谱）；协作镜像：飞书（单向同步）

## 目录结构速览
```
knowledge-base/
├── INDEX.md              # 总入口
├── AGENTS.md / CLAUDE.md # schema（任意 Agent 接管必读）
├── log.md                # 记账层（倒序事件）
├── _templates/           # 模板（含 lessons、knowledge-page）
├── knowledge/            # 复利层：跨项目可复用资产
└── projects/<代号>/      # 项目层：overview/status/decisions/progress-log/next-steps/lessons
```

## 在新环境快速上手
1. `git clone git@github.com:Cheney-H/knowledge-base.git`
2. 对 Agent 说「读 AGENTS.md 和 INDEX.md，接管这个知识库」
3. 即可按三动作（Ingest/Distill/Lint）运行
