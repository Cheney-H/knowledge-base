# 知识库操作约定（AGENTS.md）

> 任意 Agent（Ducc/Claude/Codex/…）接管本库前必读。交接指令："读 AGENTS.md 和 INDEX.md，接管这个知识库。"

## 三层结构
- `projects/<代号>/`：项目层，记全（overview/status/decisions/progress-log/next-steps/lessons）
- `knowledge/`：复利层，跨项目可复用资产，一主题一页
- `log.md`：记账层，倒序记录所有事件

## 三个动作
- **Ingest（"整理一下 <项目>"）**：更新项目文档 + INDEX 时间戳 + 记 log.md + 提醒 push
- **Distill（"给 <项目> 做一次沉淀"）**：通读全项目 → 补 decisions/lessons → 提炼通用经验到 knowledge/ → 建双向链接 → 记 log.md
  - **回写铁律**：凡是给某条 knowledge 页标了 `sources`/来源项目，就必须回写该项目层（progress-log 或 lessons），保证项目层与复利层对齐，不能只单方面长复利层。
  - 例外：纯外部阅读、无归属工作项目的通用方法论，只落 knowledge/ 即可（来源标为外部素材，不强行挂项目）。
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
