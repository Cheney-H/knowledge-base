# Cheney 第二大脑 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在现有 `knowledge-base` 仓库上搭建三层第二大脑（项目层 / 复利层 / 记账层），融合《第二大脑》CODE-PARA 与卡帕西 llm-wiki 知识复利。

**Architecture:** 纯 Markdown + Git，工具无关。新增 `knowledge/` 复利层、`log.md` 记账层、`AGENTS.md`/`CLAUDE.md` schema、两个模板，并为 aistock 补 `lessons.md`，扩展 INDEX/README。

**Tech Stack:** Markdown、Git、frontmatter（YAML）、Obsidian 双向链接语法 `[[...]]`。

**说明：** 无可运行代码，故每个 Task 的"验证步骤"检查文件存在与结构正确，替代传统单元测试。

---

### Task 1: 记账层 log.md

**Files:**
- Create: `log.md`

- [ ] **Step 1: 创建 log.md**

```markdown
# 知识库记账日志（Log）

> 倒序记录每次 Ingest / Distill / Lint 事件。最新在最上。
> 格式：`- YYYY-MM-DD [动作] 范围 — 摘要（由谁/哪个 Agent）`

## 事件

- 2026-09-29 [Setup] 全库 — 搭建三层架构：新增 knowledge/、log.md、AGENTS.md/CLAUDE.md、模板，aistock 补 lessons.md（Ducc）
```

- [ ] **Step 2: 验证**

Run: `test -f log.md && head -1 log.md`
Expected: 输出 `# 知识库记账日志（Log）`

- [ ] **Step 3: Commit**

```bash
git add log.md
git commit -m "feat: 新增 log.md 记账层"
```

---

### Task 2: 复利层页面模板

**Files:**
- Create: `_templates/knowledge-page.md`

- [ ] **Step 1: 创建模板**

```markdown
---
type: pattern | lesson | method
tags: []
sources: []
updated: YYYY-MM-DD
---
# 标题
**一句话结论**（顶层摘要，扫一眼就懂）

## 背景 / 问题

## 结论 / 做法

## 反例 / 边界（何时不适用）

## 相关
[[项目代号]] · [[另一个知识页]]
```

- [ ] **Step 2: 验证**

Run: `test -f _templates/knowledge-page.md && grep -c 'type: pattern' _templates/knowledge-page.md`
Expected: 输出 `1`

- [ ] **Step 3: Commit**

```bash
git add _templates/knowledge-page.md
git commit -m "feat: 新增复利层页面模板"
```

---

### Task 3: 项目踩坑经验模板 lessons.md

**Files:**
- Create: `_templates/lessons.md`

- [ ] **Step 1: 创建模板**

```markdown
---
project: <代号>
updated: YYYY-MM-DD
---
# <项目> · 踩坑与关键经验

> 本项目自己的经验（哪怕只对本项目有用也记）。够通用的会被提炼到 `knowledge/`。

## 关键经验
- （结论）— 背景 / 根因 / 怎么避免

## 踩过的坑
- （现象）— 根因 — 规避方法

## 已提炼到复利层
- [[知识页名]]（若某条经验已上升为通用知识，链接到此）
```

- [ ] **Step 2: 验证**

Run: `test -f _templates/lessons.md && grep -c '已提炼到复利层' _templates/lessons.md`
Expected: 输出 `1`

- [ ] **Step 3: Commit**

```bash
git add _templates/lessons.md
git commit -m "feat: 新增项目 lessons.md 模板"
```

---

### Task 4: 复利层目录与索引

**Files:**
- Create: `knowledge/INDEX.md`

- [ ] **Step 1: 创建 knowledge/INDEX.md**

```markdown
# 复利层 · 跨项目知识索引

> 一主题一页。从项目中提炼的可复用资产。扁平化，>30 页再拆子目录。

| 页面 | 类型 | 来源项目 | 更新 | 一句话 |
|---|---|---|---|---|
| _（暂无，首次 Distill 后填充）_ | | | | |

## 类型图例
- pattern：可复用技术/架构方案
- lesson：跨项目踩坑 + 根因 + 规避
- method：决策模式、工作方法论
```

- [ ] **Step 2: 验证**

Run: `test -f knowledge/INDEX.md && grep -c 'pattern' knowledge/INDEX.md`
Expected: 输出 ≥ `1`

- [ ] **Step 3: Commit**

```bash
git add knowledge/INDEX.md
git commit -m "feat: 新增 knowledge/ 复利层与索引"
```

---

### Task 5: schema 主文件 AGENTS.md

**Files:**
- Create: `AGENTS.md`

- [ ] **Step 1: 创建 AGENTS.md**

```markdown
# 知识库操作约定（AGENTS.md）

> 任意 Agent（Ducc/Claude/Codex/…）接管本库前必读。交接指令："读 AGENTS.md 和 INDEX.md，接管这个知识库。"

## 三层结构
- `projects/<代号>/`：项目层，记全（overview/status/decisions/progress-log/next-steps/lessons）
- `knowledge/`：复利层，跨项目可复用资产，一主题一页
- `log.md`：记账层，倒序记录所有事件

## 三个动作
- **Ingest（"整理一下 <项目>"）**：更新项目文档 + INDEX 时间戳 + 记 log.md + 提醒 push
- **Distill（"给 <项目> 做一次沉淀"）**：通读全项目 → 补 decisions/lessons → 提炼通用经验到 knowledge/ → 建双向链接 → 记 log.md
- **Lint（"做一次知识库体检"）**：扫全库找矛盾/过时/孤立页/缺失交叉引用/INDEX 不符 → 出报告

## 链接规则
1. 项目→知识：`[[知识页名]]`
2. 知识→项目：`sources` + "相关"段写 `[[项目代号]]`
3. 知识↔知识：相关互链
4. 保证双向可追溯

## frontmatter 规范
- knowledge 页：type / tags / sources / updated
- lessons 页：project / updated

## 人机分工
- 人：挑什么值得记、拍板哪些经验够通用、最终判断
- Agent：总结、写文档、维护交叉引用、记账、体检

## push 流程
`git add . && git commit -m "..." && git push`；重要节点从 GitHub 同步到飞书镜像。
```

- [ ] **Step 2: 验证**

Run: `test -f AGENTS.md && grep -c 'Distill' AGENTS.md`
Expected: 输出 ≥ `1`

- [ ] **Step 3: Commit**

```bash
git add AGENTS.md
git commit -m "feat: 新增 AGENTS.md schema 主文件"
```

---

### Task 6: CLAUDE.md 指向 AGENTS.md

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: 创建 CLAUDE.md**

```markdown
# CLAUDE.md

本知识库的操作约定统一维护在 [AGENTS.md](AGENTS.md)，请以其为准（避免两份漂移）。

Ducc / Claude 接管前请先读 `AGENTS.md` 和 `INDEX.md`。
```

- [ ] **Step 2: 验证**

Run: `test -f CLAUDE.md && grep -c 'AGENTS.md' CLAUDE.md`
Expected: 输出 ≥ `1`

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "feat: 新增 CLAUDE.md 指向 AGENTS.md"
```

---

### Task 7: 为 aistock 补 lessons.md

**Files:**
- Create: `projects/aistock/lessons.md`

- [ ] **Step 1: 复制模板并填充占位（内容由后续 Distill 补全）**

基于 `_templates/lessons.md` 创建，project 填 `aistock`，updated 填当天：

```markdown
---
project: aistock
updated: 2026-09-29
---
# AiStock · 踩坑与关键经验

> 本项目自己的经验（哪怕只对本项目有用也记）。够通用的会被提炼到 `knowledge/`。

## 关键经验
- _（待下次 Distill 从 decisions.md / progress-log.md 提炼填充）_

## 踩过的坑
- _（待填充）_

## 已提炼到复利层
- _（暂无）_
```

- [ ] **Step 2: 验证**

Run: `test -f projects/aistock/lessons.md && grep -c 'project: aistock' projects/aistock/lessons.md`
Expected: 输出 `1`

- [ ] **Step 3: Commit**

```bash
git add projects/aistock/lessons.md
git commit -m "feat: aistock 补 lessons.md"
```

---

### Task 8: 扩展 INDEX.md（加复利层区 + 项目结构补 lessons.md）

**Files:**
- Modify: `INDEX.md`

- [ ] **Step 1: 在 INDEX.md 项目表格后、"状态图例"前插入复利层区块**

在 `---` 分隔线后插入：

```markdown
## 复利层（跨项目知识）
> 从项目中提炼的可复用资产，入口见 [knowledge/INDEX.md](knowledge/INDEX.md)。

```

- [ ] **Step 2: 更新"每个项目的标准结构"代码块，加入 lessons.md**

将现有结构块中的：
```
└── next-steps.md    # 下一步、待办、卡点
```
替换为：
```
├── next-steps.md    # 下一步、待办、卡点
└── lessons.md       # 本项目踩坑与关键经验
```

- [ ] **Step 3: 验证**

Run: `grep -c '复利层' INDEX.md && grep -c 'lessons.md' INDEX.md`
Expected: 两行均输出 ≥ `1`

- [ ] **Step 4: Commit**

```bash
git add INDEX.md
git commit -m "docs: INDEX 加复利层区块与 lessons.md 结构"
```

---

### Task 9: 更新 README.md（三层架构 + 运行机制 + 多 Agent 接管）

**Files:**
- Modify: `README.md`

- [ ] **Step 1: 在"目录结构"代码块替换为三层结构**

将现有目录结构块替换为：
```
knowledge-base/
├── README.md
├── INDEX.md           # 总入口
├── AGENTS.md          # schema 主文件（任意 Agent 接管必读）
├── CLAUDE.md          # 指向 AGENTS.md
├── log.md             # 记账层（倒序事件）
├── _templates/        # 模板（含 lessons.md、knowledge-page.md）
├── knowledge/         # 复利层：跨项目可复用资产
│   └── INDEX.md
└── projects/
    └── <代号>/        # 项目层：overview/status/decisions/progress-log/next-steps/lessons
```

- [ ] **Step 2: 在"设计理念"后新增"运行机制"小节**

```markdown
## 运行机制（三个动作）
- **Ingest（随手记）**：说"整理一下 <项目>" → Ducc 更新项目文档 + 记 log.md
- **Distill（深沉淀）**：里程碑时说"给 <项目> 做一次沉淀" → 提炼通用经验到 knowledge/ 并建双链
- **Lint（体检）**：每两周说"做一次知识库体检" → 扫矛盾/过时/孤立页
- 人负责判断，Agent 负责记账。详见 [AGENTS.md](AGENTS.md)。

## 多 Agent 接管
本库工具无关，Codex 等任意 Agent 可随时接管：只需"读 AGENTS.md 和 INDEX.md"。
```

- [ ] **Step 3: 验证**

Run: `grep -c '运行机制' README.md && grep -c '多 Agent' README.md`
Expected: 两行均输出 ≥ `1`

- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "docs: README 补三层架构、运行机制、多 Agent 接管"
```

---

### Task 10: 全库结构收尾验证

- [ ] **Step 1: 验证所有新增文件就位**

Run:
```bash
for f in log.md AGENTS.md CLAUDE.md knowledge/INDEX.md _templates/lessons.md _templates/knowledge-page.md projects/aistock/lessons.md; do test -f "$f" && echo "OK $f" || echo "MISSING $f"; done
```
Expected: 全部 `OK`

- [ ] **Step 2: 验证 git 工作区干净**

Run: `git status --short`
Expected: 无输出（全部已提交）

- [ ] **Step 3: 提醒用户 push**

告知：本地已完成，运行 `git push` 同步到 GitHub 权威版；重要节点再同步飞书镜像。
```

---

## Self-Review

**Spec 覆盖检查：**
- 三层架构 → Task 1/4（log、knowledge）+ 现有 projects ✅
- lessons.md（项目层求全）→ Task 3/7 ✅
- 复利层组织与 frontmatter → Task 2/4 ✅
- 三个动作运行机制 → Task 5(AGENTS) + Task 9(README) ✅
- 链接规则 → Task 5 ✅
- 多 Agent 接管（AGENTS/CLAUDE 双份）→ Task 5/6 ✅
- INDEX/README 扩展 → Task 8/9 ✅
- Obsidian 定位 → 记录于 spec，无需建文件（属工具使用约定，AGENTS 已含链接语法）✅

**占位符扫描：** aistock lessons.md 的"待填充"是**有意的**——内容需下次 Distill 从真实项目提炼，非计划失误；已在 Task 7 注明。其余无占位。

**类型一致性：** frontmatter 字段（type/tags/sources/updated、project/updated）在 Task 2/3/5/7 中一致；`[[...]]` 链接语法全程一致。

无遗漏。
