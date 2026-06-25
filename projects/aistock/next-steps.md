---
project: aistock
doc: next-steps
status: active
updated: 2026-06-25
tags: [stock, todo, backtest]
---

# AiStock · 下一步 & 卡点

## 下一步（按优先级）
- [ ] **P0**：持续跟踪 2026 实盘模拟（sim_live_2026），观察纪律在真实行情下的表现
- [ ] **P1**：用真实指数成分股池（`index_stock_cons_csindex('000922')` 中证红利）替换硬编码股票池，消除幸存者偏差
- [ ] **P1**：加入熊市样本（2008 / 2015 / 2018）回测，检验行业估值标尺在周期反转时的有效性

## 待办池
- [ ] 复利 / 滚动回测（rolling backtest）
- [ ] 分红再投资（dividend reinvestment）建模
- [ ] 税后口径核算（after-tax accounting）
- [ ] 核心长持 + 卫星动态的混合策略（core-long-hold + satellite-dynamic）
- [ ] 实现 mootdx / tencent adapter（当前为 stub）
- [ ] 产品化：M2 筛选器、M4 数据质量、M5 财务扩展
- [ ] 同步 README 阶段表，使其反映实际已实现的 DCF/行情/分红能力

## 卡点 / 开放问题
- 当前股票池硬编码（`dcf_monitor_service.py`），存在幸存者偏差
- 2026 年估值普遍偏贵，DCF 严格执行会导致零持仓——纪律与机会的平衡仍需观察
- 动态轮动在牛市中跑输持有，其「保险价值」尚未在真实回撤中被验证

## 后续可探索方向（EXPERIMENTS.md:156-164）
- 实验 5 已完成 ✅；其余方向：复利回测、熊市样本、分红再投资、税后口径、真实指数成分池
