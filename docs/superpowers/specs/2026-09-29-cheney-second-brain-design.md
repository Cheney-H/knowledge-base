# Cheney 的第二大脑 · 设计文档（Spec）

- 日期：2026-09-29
- 作者：Cheney（哈成） + Ducc
- 状态：已对齐，待实现

## 一、目标与范围

在现有 `knowledge-base` 仓库基础上，搭建一套**面向工作/项目**的个人知识管理系统（"第二大脑"），融合两套理论：

- **《打造第二大脑》（Tiago Forte）**：CODE（Capture/Organize/Distill/Express）+ PARA，理念是"大脑用来思考不用来存储，为行动而组织"。
- **卡帕西 llm-wiki（知识复利理论）**：知识应在摄入时被 AI 编译进持久、互链的 Markdown，一次编译长期维护，越用越厚；AI 负责"记账苦力"，人负责判断。

**范围（方案 A）**：仅覆盖工作/项目，不涵盖个人生活/外部资料囤积。故意不建 `raw/` 原始来源层（YAGNI）。

**复利目标（方案 C）**：项目内记全 + 跨项目经验提炼成独立复利层，双向链接可追溯。

## 二、整体架构（三层）

```
knowledge-base/
├── INDEX.md              # 总入口（已有，扩展加 knowledge 区）
├── projects/             # 【项目层】记全、记细
│   └── <代号>/
│       ├── overview.md
│       ├── status.md
│       ├── decisions.md      # 本项目关键决策 + 为什么
│       ├── progress-log.md   # 完整过程时间线（倒序）
│       ├── next-steps.md
│       └── lessons.md        # 【新增】本项目踩坑与关键经验
├── knowledge/            # 【复利层】跨项目可复用资产（新增）
│   ├── INDEX.md
│   └── *.md                  # 一主题一页，扁平化
├── log.md                # 【记账层】倒序记录 ingest/distill/lint 事件（新增）
├── _templates/           # 模板（已有，补 lessons.md 和 knowledge 页模板）
├── AGENTS.md             # 【新增】schema 主文件（工具无关，Codex/任意 Agent 读）
└── CLAUDE.md             # 【新增】指向 AGENTS.md，供 Ducc/Claude 读
```

### 三层与双理论映射
| 层 | 目录 | 第二大脑 | llm-wiki |
|---|---|---|---|
| 项目层 | `projects/` | Projects + Capture/Organize | 原始上下文来源 |
| 复利层 | `knowledge/` | 中间产物（Intermediate Packets） | 实体/主题页，交叉引用 |
| 记账层 | `log.md` | —— | log.md 复利引擎、可追溯 |

## 三、两层职责（不重不漏）

- **项目层求全**：一个项目发生的一切（过程、决策、踩坑、经验）完整保留在此，`lessons.md` 记录哪怕只对本项目有用的经验。
- **复利层求通**：当某条经验**跨项目反复出现或具备普适性**时，抽象成 `knowledge/` 一页，用 `sources` 双链指回来源项目。项目专属经验不强行上升。
- **原则**：先在项目层记全，再按需向复利层提炼；双链保证可追溯、不丢失。

## 四、复利层组织与链接规则

### knowledge 页类型（用 frontmatter `type` 区分，规模 >30 页再拆子目录）
- `pattern`：可复用技术/架构方案
- `lesson`：跨项目踩坑 + 根因 + 规避
- `method`：决策模式、工作方法论

### knowledge 页标准结构
```markdown
---
type: pattern | lesson | method
tags: [回测, 数据]
sources: [aistock]        # 来自哪些项目
updated: 2026-09-29
---
# 标题
**一句话结论**（顶层摘要，扫一眼就懂）

## 背景 / 问题
## 结论 / 做法
## 反例 / 边界（何时不适用）
## 相关：[[aistock]] · [[另一个知识页]]
```

### 链接规则（Ducc 维护，人在 Obsidian 消费）
1. 项目 → 知识：项目文档引用经验时写 `[[知识页名]]`
2. 知识 → 项目：知识页 `sources` + "相关"段反向指回 `[[项目代号]]`
3. 知识 ↔ 知识：相关经验互链
4. 双向可追溯：任一经验可查来源项目；任一项目可见其贡献/引用的通用经验

## 五、运行机制（方案 D：三个动作）

| 动作 | 触发语 | Ducc 做什么 | 节奏 |
|---|---|---|---|
| **Ingest（随手记）** | "整理一下 <项目>" | 更新 progress-log/status/next-steps/lessons，更新 INDEX 时间戳，记 log.md，提醒 push | 平时随手 |
| **Distill（深沉淀）** | "给 <项目> 做一次沉淀" | 通读全项目 → 补全 decisions/lessons → 判断并提炼通用经验到 knowledge/ → 建双向链接 → 记 log.md | 里程碑/阶段结束 |
| **Lint（体检）** | "做一次知识库体检" | 扫全库找矛盾/过时/孤立页/缺失交叉引用/INDEX 不符 → 出体检报告待决 | 每两周 |

### 人机分工（卡帕西原则）
- **人（Cheney）**：挑什么值得记、拍板哪些经验够通用、提好问题、最终判断
- **Ducc/Agent**：总结、写文档、维护交叉引用、记账、体检

## 六、工具定位

- **写入/维护**：Ducc（或其他 Agent）+ Git（GitHub 私有仓库 = 唯一权威版）
- **人类阅读/导航**：Obsidian，本地打开同一文件夹，用双向链接 + 关系图谱 + 全文搜索
  - Obsidian 定位为"只读消费为主"的阅读层，写入仍走 Agent + Git，维护负担极低
  - 注意：`[[wiki链接]]` 是 Obsidian 方言，飞书镜像里可能显示为纯文本，不影响 Agent/Git
- **协作/分享**：飞书知识库镜像，从 GitHub 单向同步，不含双链

## 七、多 Agent 可接管（Codex 等）

- 系统为纯 Markdown + Git + schema，工具无关，任何能读写文件 + 跑 git 的 Agent 均可接管。
- **schema 双份、内容一致**：`AGENTS.md`（主文件，通用约定）+ `CLAUDE.md`（一行指向 AGENTS.md，避免漂移）。
- schema 内容需写清：三层结构、三个动作、链接规则、frontmatter 规范、push 流程。
- 交接指令示例："读 AGENTS.md 和 INDEX.md，接管这个知识库。"
- 风格差异由 schema 约束兜底，`log.md` 记录每次变更来源，可追溯。

## 八、需要新建/修改的产物清单

1. `knowledge/` 目录 + `knowledge/INDEX.md`
2. `log.md`（记账层，倒序）
3. `AGENTS.md`（schema 主文件）+ `CLAUDE.md`（指向 AGENTS.md）
4. `_templates/lessons.md`（项目踩坑经验模板）
5. `_templates/knowledge-page.md`（复利层页面模板）
6. 为现有项目 `projects/aistock/` 补 `lessons.md`
7. 扩展 `INDEX.md`：增加 knowledge 复利层区块
8. 更新 `README.md`：补充三层架构、运行机制、多 Agent 接管说明

## 九、非目标（明确不做）

- 不建 `raw/` 外部资料囤积层
- 不涵盖个人生活/健康/财务等 PARA Areas
- 不引入 Obsidian 插件写入或双向同步
- knowledge 层初期不建子目录（扁平化，>30 页再拆）
