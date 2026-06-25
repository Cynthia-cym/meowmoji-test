# 测试文档与产品动态漂移 · 排查与整改计划

> **状态：** live · **更新：** 2026-06-23（R1–R4 已执行）  
> **真源：** [docs/live/STATUS.md](../docs/live/STATUS.md) · [DECISIONS.md](../docs/live/DECISIONS.md)  
> **原则：** [test.md](./test.md)

---

## 0. 当前产品基线（测试文档应对齐）

| 项 | 现行 |
| --- | --- |
| 小程序 | 正式版 **v1.0.51** 已全量（Gate3 关闭） |
| API | 正式 `api.meowmoji.cn`；体验版联调 `stg-api` |
| Gate | **Gate4** 上线观测；Gate3 **164 已关闭**（2026-06-20）；广场 v2 增补后规格 **170**（2026-06-24，**不走 Gate 表逐项**） |
| 上架自动化 | 本机 **Chrome CDP** + `worker:cdp`（ADR-017/018/021）；云 Worker **禁用** |
| 提交策略 | **自动点【提交】**（ADR-018）；非 ADR-005 草稿 |
| 模板 B 贴纸 | **MVP 不交付**（ADR-022）；`TEMPLATE_B_DECORATIONS_MVP_ENABLED=false` |
| 订阅 | `WX_SUBSCRIBE_MODE=live` 真机 ✅ |
| 协议 PDF | 法务复核 ✅ |

---

## 1. 已整改（2026-06-23）

| 批次 | ID | 状态 |
| --- | --- | --- |
| Phase 1–5 | ADR-022 · 贴纸用例 · 代码开关 | ✅ |
| R1 | D-01 · D-02 · D-04 · environments | ✅ |
| R2 | D-06 · D-07 · D-08 · product-scope §2.5 | ✅ |
| R3 | D-11 · TRACEABILITY 增量 | ✅ |
| R4 | D-03 feishu-gate Wiki 同步说明 | ✅（线下真源；Wiki 待产品点表） |

---

## 2. 剩余低优先级

| ID | 说明 |
| --- | --- |
| D-03 | 飞书 Wiki 表行需产品在浏览器更新 |
| D-13 | `项目进度.md` 已有 deprecated 横幅 |
| D-15～17 | 历史 CHANGELOG/WORKLOG/archive — 不改写 |

---

## 3. 维护规则

1. 行为/产品决策 → `DECISIONS` + `STATUS` → 修订 `TEST-SPEC-v3` 受影响条目  
2. Gate 关闭后 → `EXECUTION` 追加 + 本文件 §1  
3. 环境默认 **正式版+api**；staging 仅联调（[environments.md](./environments.md)）  
4. 自动化增量 → `sticker-automation-test-2026-06.md`

---

## 4. 变更记录

| 日期 | 说明 |
| --- | --- |
| 2026-06-23 | R1–R4 执行；admin-api 生产部署 ADR-022 |
| 2026-06-23 | 初版 Phase 1–5 |
