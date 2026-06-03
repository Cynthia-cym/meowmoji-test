# 小程序交叉测试 · 执行记录

> **用例：** [miniprogram-test.md](./miniprogram-test.md)（MP-400~416）· [cms-admin-test.md](./cms-admin-test.md)（CMS-400~409）  
> **计划：** [TEST-PLAN-2026-06-03.md](./TEST-PLAN-2026-06-03.md)  
> **策略：** [Meowmoji-CMS01/docs/live/TEST.md](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/TEST.md)

---

## 说明

- **Codex** 维护用例规格（不写代码、不执行）
- **Cursor** 执行全部测试并记录本文件
- **2026-06-03：** 跳过 Puppeteer 自动上架；真源 MP-400+ / CMS-400+

---

## 执行记录

| 日期 | 执行人 | 环境 | 通过 | 失败/待测/SKIP | 备注 |
| --- | --- | --- | --- | --- | --- |
| 2026-05-31 | Cursor | staging API | 部分 | MP-142~145、MP-150~154 | 后端闭环；真机待测 |
| 2026-06-01 | — | — | — | — | 文档治理切换 |
| 2026-06-03 | — | v1.0.10 体验版 | — | — | 计划已 push；待 Phase 0 |

---

## 待执行（TEST-PLAN-2026-06-03）

| Phase | 内容 | 负责人 |
| --- | --- | --- |
| 0 | check:all + admin-api test + healthz | Cursor |
| 1 | MP-400~410 真机 P0 | Cursor |
| 2 | MP-414~415 + CMS-403/404/407 | Cursor |
| 3 | CMS-400~409（405 草稿模式留意） | Cursor |
| 4 | MP-411~413、416 + CMS 其余 P1 | Cursor |

**SKIP：** CMS-030/031 · MP-123 · MP-153 全量 · Puppeteer 自动提交微信
