# Meowmoji 交叉测试仓库

> **测试原则：** [test.md](./test.md)（Cursor 执行基准 · ADR-011）  
> **分工：** Cursor 负责全部测试；产品仅真机验收

## 当前版本（V2026.06.03 · v2.1 演进中）

| 真源 | 文件 |
| --- | --- |
| **工作原则** | [test.md](./test.md) |
| **用例 v3** | [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) · Gate [feishu-gate-v3.md](./feishu-gate-v3.md) |
| **产品路径** | [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md) |
| 真机路径 | [TEST-ACCEPTANCE-PATHS.md](./TEST-ACCEPTANCE-PATHS.md)（v3 定稿后更新） |
| 飞书填表 | [Wiki 验收清单](https://qcnp7uavxd21.feishu.cn/wiki/DqxmwME1giejvgkauWycNXqanRc) |
| 改进待办（审查） | [TEST-IMPROVEMENT-BACKLOG.md](./TEST-IMPROVEMENT-BACKLOG.md) |
| 追溯矩阵 | [TRACEABILITY.md](./TRACEABILITY.md) |
| 执行记录 | [EXECUTION.md](./EXECUTION.md) |
| **文档漂移整改** | [TEST-DOC-DRIFT-REMEDIATION.md](./TEST-DOC-DRIFT-REMEDIATION.md) |
| **表情上架自动化测试** | [sticker-automation-test-2026-06.md](./sticker-automation-test-2026-06.md) |
| 索引（legacy） | `miniprogram-test.md` · `cms-admin-test.md` |

历史 MP-001~302 / `cms-test.md` 仅追溯。

## 分工（ADR-011）

| 角色 | 职责 |
| --- | --- |
| **Cursor** | 用例设计 · 开发者工具/CMS 测试 · 自动化 · Bug · 文档 |
| **产品** | **真机验收** + 飞书填表 |
| **Codex** | 不再维护用例（[CODEX.md](./CODEX.md) 已归档说明） |

## 仓库关系

| 仓库 | 用途 |
| --- | --- |
| [meowmoji-test](https://github.com/Cynthia-cym/meowmoji-test) | 本仓 · 测试真源 |
| [Meowmoji-CMS01](https://github.com/Cynthia-cym/Meowmoji-CMS01) | CMS · Live 见 `docs/live/` |
| [Meowmoji](https://github.com/Cynthia-cym/Meowmoji) | 小程序运行代码 |
| [Meowmoji--mini-program](https://github.com/Cynthia-cym/Meowmoji--mini-program) | 小程序文档 |

Live：[STATUS](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/STATUS.md) · [TEST](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/TEST.md) · [DECISIONS ADR-011](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/DECISIONS.md)
