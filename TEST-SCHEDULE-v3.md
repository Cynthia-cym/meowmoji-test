---
status: archived
frozen_at: 2026-08-11
superseded_by: README.md
---

# Meowmoji 测试排期 v3

> **Gate：** P0+P1 = 170 条 · **原则：** [test.md](./test.md)  
> **真机路径：** [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md)

---

## 分工

| 角色 | 执行面 | Gate 范围 |
| --- | --- | --- |
| **产品** | 体验版真机 | **P0 主路径**（~1 日）+ 抽测 BND/NFR-COMPAT |
| **Cursor** | 开发者工具 + CMS 浏览器 + CI | **全部 P0+P1** |
| P2/P3 | backlog | 不阻塞 No-Go |

---

## D1 · 产品真机 + Cursor 并行

| 时段 | 产品（真机） | Cursor |
| --- | --- | --- |
| 上午 | F1 登录 P0 · F2 制作 P0（模板 B + 扣次） | CMS P0：C1/C3/C4 · 开发者工具复跑 F2 |
| 下午 | F3 上架 P0 · F6 消息 · F4 广场 E2E 抽查 | CMS 审核配合 · E2E-001 闭环 · 填 EXECUTION |
| 抽测 | NFR-COMPAT-001 或 002 · BND 003/004/006 | P1 逆向/边界 BND 批量 · admin-api CI |

**D1 出口：** P0 真机主路径表填完；Cursor P0 CMS+小程序 100%；E2E-001 至少 1 次全绿。

---

## D2–D3 · Cursor Gate 扩展（P1）

| 日 | 内容 |
| --- | --- |
| D2 | 小程序 P1 逆向/边界/联调（MP-F* + BND-MP） |
| D2 | CMS P1（C1/C3/C5/C6/C8 + BND-CMS） |
| D3 | E2E-002–005 · NFR-SEC/UX/PERF 冒烟 · EXC/PATH P1 |

**D3 出口：** Gate 170 条 EXECUTION 有结果；失败项进飞书 + Issue。

---

## D4+ · backlog

- P2：SCN 全量场景、深度 EXC、扩展 NFR
- P3：F10 预留、环境专项

---

## 环境

| 项 | 值 |
| --- | --- |
| 小程序 | 体验版 / 编译版 |
| API | stg-api.meowmoji.cn |
| CMS | cms.meowmoji.cn |
| 跳过 | Puppeteer 自动提交微信（P3） |

---

## 记录

- Cursor：[EXECUTION.md](./EXECUTION.md)
- 产品：飞书 Wiki 验收清单
- 镜像：[feishu-gate-v3.md](./feishu-gate-v3.md)
