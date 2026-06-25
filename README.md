# 项目知识库（Knowledge Base）

我的跨项目知识中心：把每个项目的经验、过程、阶段性状态结构化记录下来，便于**换环境后快速复用**——人能快速回忆，AI（Ducc）也能秒读上下文接着干。

## 设计理念
- **本地优先 + 云同步**：内容是纯 Markdown，存在本地，靠 GitHub 同步，数据始终在自己手里。
- **单一数据源 + 单向分发**：
  - **GitHub 私有仓库 = 唯一权威版（source of truth）**，AI 写、我 push，有版本历史。
  - **飞书知识库 = 人类阅读/协作的镜像**，从 GitHub 单向同步过去。
  - 内容只改 GitHub，飞书按需同步，**不做双向同步**（避免冲突）。
- **人机两用**：每个文档带 frontmatter（项目/状态/更新时间/标签），人看清晰，AI 检索精准。

## 目录结构
```
knowledge-base/
├── README.md          # 本文件
├── INDEX.md           # 所有项目总索引（入口）
├── _templates/        # 5 个文档模板
└── projects/
    └── <代号>/
        ├── overview.md      # 是什么、目标、技术栈、约束、快速上手
        ├── status.md        # 当前状态、里程碑、关键指标
        ├── decisions.md     # 关键决策 + 为什么
        ├── progress-log.md  # 进展时间线（倒序）
        └── next-steps.md    # 下一步、待办、卡点
```

## 在新环境快速恢复（给 Ducc / 我自己）
1. `git clone <你的私有仓库>` 到新机器
2. 让 Ducc 读 `INDEX.md` → 选择项目 → 读 `projects/<代号>/` 全部 5 个文档
3. Ducc 即可「记起」该项目的目标、决策、进展、下一步，接着干

> **给 AI 的提示**：恢复某项目上下文时，先读 `INDEX.md` 定位，再按 overview → decisions → status → next-steps → progress-log 的顺序读，效率最高。

## 新增 / 更新项目（让 Ducc 自动整理）
- 新增项目：基于 `_templates/` 复制一套到 `projects/<代号>/`，让 Ducc 填充
- 阶段性更新：对 Ducc 说「整理一下 <项目> 的进展」，它会更新对应文档 + `INDEX.md` 的时间戳，并提醒 push

## 同步流程
1. 本地更新文档（Ducc 写 / 手动改）
2. `git add . && git commit -m "..." && git push`
3. （重要节点）从 GitHub 同步到飞书知识库供阅读/协作
