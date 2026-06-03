# Codex 必读（另一台电脑 · 仅 GitHub）

> **你不是 Cursor。** 你没有 `/Users/xxll/Desktop/CMS` 一体工作区。  
> **你只能** `git clone` / 浏览 **GitHub 上 `main` 分支** 的文件。  
> **你只能写** 本仓库 `Cynthia-cym/meowmoji-test` 的 md 文件。

---

## 每次会话第一步

1. `git pull origin main`（本仓）
2. 读本文件 + [README.md](./README.md) + [TEST-PLAN-2026-06-03.md](./TEST-PLAN-2026-06-03.md)
3. 再读任务里列出的其他 **github.com/blob/main** 链接

---

## 现行分工（ADR-004）

| 你（Codex） | Cursor |
| --- | --- |
| **只写**本仓用例规格 | 开发 + 部署 + **执行全部测试** |

勿读 archived handoff 定分工。以 Live DECISIONS 为准。

---

## 必读 GitHub 链接

| 文档 | URL |
| --- | --- |
| Live 状态 | https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/STATUS.md |
| Live 决策 | https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/DECISIONS.md |
| Live 测试策略 | https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/TEST.md |
| Live 变更日志 | https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/live/CHANGELOG.md |
| 小程序分工 | https://github.com/Cynthia-cym/Meowmoji--mini-program/blob/main/ai.md |
| 小程序 config | https://github.com/Cynthia-cym/Meowmoji/blob/main/config.js |

---

## 本仓真源编号（2026-06）

| 类型 | 文件 | 编号 |
| --- | --- | --- |
| 小程序真机 | [miniprogram-test.md](./miniprogram-test.md) | **MP-400~416**（收口） |
| CMS 浏览器 | [cms-admin-test.md](./cms-admin-test.md) | **CMS-400~409** |
| 历史追溯 | cms-test.md、MP-001~302 | 非本轮唯一依据 |

冲突时 **以 MP-400+ / CMS-400+ 为准**。

---

## 你应改的文件

- [miniprogram-test.md](./miniprogram-test.md)
- [cms-admin-test.md](./cms-admin-test.md)
- [environments.md](./environments.md)
- [README.md](./README.md)（如有变）

**不要改：** [EXECUTION.md](./EXECUTION.md)（Cursor 执行记录）

---

## 交付

1. `git push origin main`
2. 回复 **commit SHA** + 改了哪些用例编号 + 5 行内摘要
3. 用例问题开 [meowmoji-test Issues](https://github.com/Cynthia-cym/meowmoji-test/issues)

---

## 禁止

- 改 Meowmoji / Meowmoji-CMS01 / Meowmoji--mini-program **代码**
- 执行真机或 CMS 点测
- 在提示词或文档中引用 Cursor 本机路径
