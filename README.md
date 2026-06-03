# Meowmoji 交叉测试仓库

> 可点击操作测试用例 + Issue 模板。Codex 写规格，Cursor 执行。

## 当前版本（V2026.06.03）

| 真源 | 文件 |
| --- | --- |
| 小程序真机 | `miniprogram-test.md` → **MP-400~MP-416** |
| CMS 浏览器 | `cms-admin-test.md` → **CMS-400~CMS-409** |
| 执行计划 | [TEST-PLAN-2026-06-03.md](./TEST-PLAN-2026-06-03.md) |
| 执行记录 | [EXECUTION.md](./EXECUTION.md)（Cursor） |

历史 MP-001~302 / `cms-test.md` 仅追溯。

## 分工（ADR-004）

| 角色 | 职责 |
| --- | --- |
| **Codex** | 仅维护本仓用例规格 |
| **Cursor** | 开发 + **执行全部测试** + `EXECUTION.md` |

## 仓库关系

| 仓库 | 用途 |
| --- | --- |
| [meowmoji-test](https://github.com/Cynthia-cym/meowmoji-test) | 本仓 · 用例真源 |
| [Meowmoji-CMS01](https://github.com/Cynthia-cym/Meowmoji-CMS01) | CMS · Live 见 `docs/live/` |
| [Meowmoji](https://github.com/Cynthia-cym/Meowmoji) | 小程序运行代码 |
| [Meowmoji--mini-program](https://github.com/Cynthia-cym/Meowmoji--mini-program) | 小程序文档 |

Live 索引（Codex 只读）：[STATUS](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/STATUS.md) · [TEST](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/TEST.md) · [DECISIONS](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/DECISIONS.md)

```bash
git clone https://github.com/Cynthia-cym/meowmoji-test.git
```
