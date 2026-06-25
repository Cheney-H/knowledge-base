---
project: aistock
doc: overview
status: active
updated: 2026-06-25
tags: [stock, a-share, value-investing, backtest, dividend, akshare, fastapi, react]
---

# AiStock · 项目总览

## 这是什么
AiStock 是一个**个人 A 股价值投资工作台**——本地运行的网页版分析与回测工具，帮助个人投资者基于低估值、财务稳健、安全边际和历史回测来筛股、做个股分析、验证买入策略。

源码位置：`~/Documents/Cursor/AiStock`

## 目标
- 帮个人投资者筛选 A 股、做个股分析、验证买入策略
- 围绕「分红 / 红利策略」做系统化研究与回测
- 提供本地工作台 UI（仪表盘 / DCF 监控 / 筛选器 / 实盘模拟）

## 边界 / 非目标
- 输出仅为**研究与决策辅助，不构成投资建议**
- 一期**不接券商、不自动交易**
- 不做高频网页抓取（数据授权边界）

## 技术栈
- **后端**：Python + FastAPI 0.111 + SQLAlchemy 2.0 + Pydantic 2 + uvicorn；SQLite（`backend/data/aistock.db`，已 gitignore）
- **前端**：React 19 + TypeScript + Vite 7 + Ant Design 5 + ECharts 6 + axios + react-router 6
- **数据**：akshare 1.16.98（**唯一可用数据源**）
- **测试**：pytest + pytest-asyncio（后端 12 个测试文件，约 33 个用例）

后端分层：`app/adapters/`（akshare / mootdx / tencent / base）、`app/api/`、`app/core/`（config、database）、`app/models/stock.py`、`app/services/`、`backend/scripts/`（研究/回测脚本）。

数据模型：`Stock`、`DailyBar`、`QuoteSnapshot`、`DataSyncJob`、`DcfSignal`。

API 路由：
- `GET /health`
- `GET /api/stocks`、`GET /api/stocks/{symbol}`
- `POST /api/data/sync`、`GET /api/data/status`（sync 类型：stock_list / daily_bars / quote_snapshots）
- `GET /api/dcf/summary|monitor|{symbol}`、`POST /api/dcf/recalculate`
- `GET /api/sim/result`、`POST /api/sim/run`、`GET /api/sim/status`

前端 10 个页面路由：**真实页面**= Backtest（实盘模拟，491 行）、DcfDashboard（252 行）、StockScreener（137 行）、Dashboard、DataCenter；**占位页**= Strategy、Watchlist、StockDetail、StockList、Settings。

## 关键约束
- **akshare-only（硬约束）**：所有股票数据只能从 akshare 获取，不做随意网页抓取。详见 [[akshare-only-stock-data]]（Ducc memory）。在 `research_diversified_dividend_strategy.md:4`、`strategy.md:13`、`EXPERIMENTS.md:3` 多处声明。
- `mootdx_adapter` / `tencent_adapter` 仍是 stub（抛 `NotImplementedError`）——akshare 实际上是唯一可用 adapter。

## 目录结构速览
```
AiStock/
├── README.md
├── backend/                         # FastAPI 后端
│   ├── app/{adapters,api,core,models,services}/
│   ├── scripts/                     # 研究/回测脚本
│   ├── data/aistock.db              # SQLite（gitignore）
│   └── requirements.txt
├── frontend/                        # React + Vite 前端
├── docs/
│   ├── specs/value-investing-workbench.md   # 914 行产品规格
│   └── superpowers/plans/2026-06-15-m1-market-data.md
├── research_dividend_strategy_100k/ # ⭐ 研究核心（5 个实验 + 实盘模拟）
├── research_diversified_dividend_strategy.md
├── research_dividend_backtest_100k/ # 计划 stub
├── research_moutai_dividend/        # 计划 stub
├── research_dividend_stocks/        # 空
└── research_stable_dividends/       # 空
```

## 在新环境快速上手
1. 克隆代码仓库，进入 `backend/`，`pip install -r requirements.txt`
2. 启动后端：`uvicorn app.main:app`（启动时 `init_db()`，M0 会插入约 5528 行 A 股）
3. 前端 `frontend/`：`npm install && npm run dev`
4. 验证：访问「实盘模拟」页 → 触发 `/api/sim/run`（首次约 1–2 分钟拉 akshare 数据）
5. 研究核心看 `research_dividend_strategy_100k/EXPERIMENTS.md`（实验总账本）
