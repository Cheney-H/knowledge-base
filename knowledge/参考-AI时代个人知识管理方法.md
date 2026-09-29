---
type: method
tags: [知识管理, PKM, AI时代, PCM, llm-wiki, 调研]
sources: [second-brain]
updated: 2026-09-29
---
# AI 时代个人知识管理方法（调研）

**一句话结论**：AI 时代 PKM 的瓶颈从"检索"转向"给 AI 提供你的上下文"，管理方式从"人整理知识"转向"人做判断、AI 做执行"；经典框架（PARA/Zettelkasten/第二大脑）仍是骨架，叠加 AI 层（PCM/AKM/llm-wiki）才面向未来。

## 背景 / 问题
LLM 已能检索和总结任何内容，"找信息"不再稀缺；稀缺的是 AI 不了解你的处境、专长与观点。因此知识管理的重心从"为自己以后检索而组织"转向"为喂给 AI 而组织上下文"。

## 结论 / 做法

### 新范式（面向未来）
- **个人上下文管理（PCM）**：保存"上下文"而非"内容"。三层上下文：身份（角色/专长/目标，稳定）、知识（高亮/批注/阅读史）、任务（当前在做的事）。实操：边读边高亮（每篇 3-7 条）、只在有真实反应时批注、每周和高亮"对话"归纳主题；把检索外包给 AI，思考留给自己。约 200-300 条后价值复利。
- **智能体化知识管理（AKM）**：翻转人机关系——AI 主动按"心跳"扫描知识库、提议行动、你批准后执行、结果写回。前提：知识库结构化 + 从只读逐步开放写权限 + 备份与最小权限安全。
- **知识复利 / llm-wiki（Karpathy）**：AI 在摄入时把知识编译进持久互链的 Markdown，一次编译长期维护、越用越厚；三动作 Ingest/Query/Lint；人挑来源提问题，AI 做记账苦力。详见 [[方法论-第二大脑搭建]]。

### 经典框架（仍是骨架）
- **PARA**（Projects/Areas/Resources/Archive，Tiago Forte）：按"可执行性/用途"而非主题分类，至今仍是默认心智模型。
- **CODE**（Capture/Organize/Distill/Express）：《第二大脑》操作框架，核心"大脑用来思考不用来存储、为行动而组织"；渐进式总结 + 中间产物。
- **Zettelkasten 卡片盒**：原子化笔记 + 链接，与 AI 知识图谱天然契合。

### 概念边界
- **PKM/Wiki** 结构化知识（给人看）；**RAG** 检索知识（喂 AI）；**Memory 系统** 让 AI 上下文随时间演化。理想形态是三者协同：结构化 PKM 同时充当 AI 的记忆与检索底座。

## 反例 / 边界（何时不适用）
- 检索仍是瓶颈的小型/一次性场景，不必上 PCM/AKM 重型机制。
- 开放 AI 写权限前必须先结构化 + 备份，且注意提示注入风险——"没有 100% 安全的 AI 使用"。
- 经典框架别追求"完美整理"，够用就好、边用边整理，否则卡在 Capture 就放弃。

## 相关
[[方法论-第二大脑搭建]] · [[second-brain]]

## 参考来源
- Agentic Knowledge Management — dsebastien.net
- Personal Context Management (Glasp)
- Karpathy llm-wiki (GitHub Gist) 及多篇解读
- Building a Second Brain (Tiago Forte) — CODE/PARA
- PKM vs RAG vs Wiki vs Memory Systems — Rost Glukhov
