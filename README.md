# Meowmoji 交叉测试仓库

> **状态：** live · **更新：** 2026-08-11 · **现行策略：** ADR-026 上线后增量测试

本仓现行入口是本文件、[test.md](./test.md) 与只追加的 [EXECUTION.md](./EXECUTION.md)。正式版 v1.0.51 的 v3 全量 Gate 已完成使命；后续迭代不重跑、不改写历史计数或结果。

## 每次迭代怎么测

1. 为新增或修改行为补/跑自动化测试；Bug 必须先见回归测试失败。
2. 运行改动仓与直接调用方的受影响单元/集成回归。
3. 跨端、环境或第三方改动验证最短真实链路并保留证据。
4. 仅用户可见的新功能或行为变化请求产品真机验收。

**通过：** 改动测试全绿 + 无未关闭的受影响 P0 +（需要真机时）产品明确确认。

## 分工

| 角色 | 后续迭代职责 |
| --- | --- |
| **Cursor** | 改动用例、自动化/影响回归、真实链路取证、Bug 与测试文档维护。 |
| **产品** | 仅用户可见行为变化的真机验收与结论。 |
| **Codex** | 维持 ADR-011 限制：不维护测试用例。 |

## 记录与历史

| 文档 | 用途 |
| --- | --- |
| [test.md](./test.md) | 增量测试原则与记录字段 |
| [EXECUTION.md](./EXECUTION.md) | 只追加的迭代执行记录 |
| [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) · [TEST-SPEC-v3-SCOPE.md](./TEST-SPEC-v3-SCOPE.md) | 已归档的正式版用例与范围历史 |
| [TEST-PRIORITY-v3.md](./TEST-PRIORITY-v3.md) · [TEST-SCHEDULE-v3.md](./TEST-SCHEDULE-v3.md) | 已归档的优先级与排期历史 |
| [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md) · [feishu-gate-v3.md](./feishu-gate-v3.md) | 已归档的产品路径与飞书 Gate 历史 |

冻结文件均标记 `status: archived`、`frozen_at: 2026-08-11`、`superseded_by: README.md`。它们保留追溯价值，但不能作为新迭代的日常 Gate 或发布准入。

Live：[STATUS](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/STATUS.md) · [TEST](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/TEST.md) · [ADR-026](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/DECISIONS.md#adr-026-正式版上线后切换为增量测试2026-08-11)
