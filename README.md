# Meowmoji 交叉测试仓库

> 存放**可点击操作**的测试用例与统一 Issue 模板，不属于 CMS 或小程序代码仓库。

## 当前版本

- 小程序真机验收真源：`miniprogram-test.md` 中 `MP-400~MP-416`
- CMS 运营验收真源：`cms-admin-test.md`
- 当前版本：`V2026.06.03`

## 仓库关系

| 仓库 | 维护方 | 用途 |
| --- | --- | --- |
| [Meowmoji-CMS01](https://github.com/Cynthia-cym/Meowmoji-CMS01) | Cursor | CMS 后台 + API |
| [Meowmoji--mini-program](https://github.com/Cynthia-cym/Meowmoji--mini-program) | Cursor | 小程序 + 云函数开发与部署 |
| **meowmoji-test**（本仓） | Codex 维护规格，Cursor 执行 | 测试用例真源 |

## 文件说明

| 文件 | 编写 | 执行 |
| --- | --- | --- |
| [miniprogram-test.md](./miniprogram-test.md) | Codex（规格真源） | Cursor 真机执行，产品扫码验收 |
| [cms-admin-test.md](./cms-admin-test.md) | Codex | Cursor / 运营按浏览器点击执行 |
| [cms-test.md](./cms-test.md) | 历史稿保留 | 仅供追溯 |
| [issue-template.md](./issue-template.md) | 双方遵守 | 提交 Issues 时复制 |
| [environments.md](./environments.md) | Cursor | 测前必读 |

## 协作（开发契约）

开发/接口变更仍走代码仓 handoff，见 Meowmoji-CMS01：

- [cursor-codex-workflow-paradigm.md](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/cursor-codex-workflow-paradigm.md)

本仓只负责**人工可点的功能测试**与**交叉验收**。

当前 live 索引见：

- [Meowmoji--mini-program/docs/live/STATUS.md](https://github.com/Cynthia-cym/Meowmoji--mini-program/blob/main/docs/live/STATUS.md)
- [Meowmoji--mini-program/docs/live/TEST.md](https://github.com/Cynthia-cym/Meowmoji--mini-program/blob/main/docs/live/TEST.md)

## 快速开始

```bash
git clone https://github.com/Cynthia-cym/meowmoji-test.git
# 测前更新三仓 main，并阅读 environments.md
```
