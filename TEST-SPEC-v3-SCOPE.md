# Meowmoji 测试用例 v3 · 工作范围

> **修订（2026-06-24）：** 广场 v2 增补 +6 条（MP-F4-011～015、MP-F5-009）→ **275 条 · Gate 170**  
> **修订（2026-06-23）：** 模板 B 贴纸 MVP 不交付（ADR-022）；Gate 已关闭，后续增量见 [TEST-DOC-DRIFT-REMEDIATION.md](./TEST-DOC-DRIFT-REMEDIATION.md)  
> **用例：** [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) — **275 条**  
> **优先级：** [TEST-PRIORITY-v3.md](./TEST-PRIORITY-v3.md) — **Gate P0+P1 = 170**

---

## catalog 与统计

| catalog | 条数 |
| --- | --- |
| 功能 MP-F* + CMS-C* | 176 |
| 边界 BND-* | 28 |
| 异常 EXC-* | 10 |
| 路径 PATH-* | 6 |
| 场景 SCN-* | 12 |
| 联调 E2E-* | 5 |
| 非功能 NFR-* | 38 |
| **合计** | **275** |

| 优先级 | 条数 |
| --- | --- |
| P0 | 62 |
| P1 | 108 |
| **Gate** | **170** |
| P2 | 98 |
| P3 | 7 |

---

## 交付物（已执行）

| 文档 | 状态 |
| --- | --- |
| [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) | ✅ 定稿 + 优先级 |
| [TEST-PRIORITY-v3.md](./TEST-PRIORITY-v3.md) | ✅ |
| [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md) | ✅ 产品 P0 路径 |
| [TEST-SCHEDULE-v3.md](./TEST-SCHEDULE-v3.md) | ✅ D1–D3 排期 |
| [feishu-gate-v3.md](./feishu-gate-v3.md) | ✅ Gate 170 镜像 |
| 飞书 Wiki | ✅ [第七节 v3 摘要](https://qcnp7uavxd21.feishu.cn/wiki/DqxmwME1giejvgkauWycNXqanRc) + P0 主路径表 |

---

## 范围外

客服工单 · 微信 API 上架 · 移动 CMS · **云 headless / Puppeteer 主路径上架**（ADR-017 已证伪）· F10 微信已上架通知（P3）
