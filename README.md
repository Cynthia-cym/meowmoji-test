# Meowmoji 交叉测试仓库

> 存放**可点击操作**的测试用例与统一 Issue 模板，不属于 CMS 或小程序代码仓库。

## 仓库关系

| 仓库 | 维护方 | 用途 |
| --- | --- | --- |
| [Meowmoji-CMS01](https://github.com/Cynthia-cym/Meowmoji-CMS01) | Cursor | CMS 后台 + API |
| [Meowmoji--mini-program](https://github.com/Cynthia-cym/Meowmoji--mini-program) | Codex | 小程序文档 + 云函数 |
| **meowmoji-test**（本仓） | 双方读写 | 测试用例 + 模板 |

## 文件说明

| 文件 | 编写 | 执行 |
| --- | --- | --- |
| [miniprogram-test.md](./miniprogram-test.md) | Cursor | Codex（微信开发者工具） |
| [cms-test.md](./cms-test.md) | Codex（Cursor 已写初稿） | Codex + Cursor |
| [issue-template.md](./issue-template.md) | 双方遵守 | 提交 Issues 时复制 |
| [environments.md](./environments.md) | Cursor | 测前必读 |

## 协作（开发契约）

开发/接口变更仍走代码仓 handoff，见 Meowmoji-CMS01：

- [cursor-codex-workflow-paradigm.md](https://github.com/Cynthia-cym/Meowmoji-CMS01/blob/main/docs/cursor-codex-workflow-paradigm.md)

本仓只负责**人工可点的功能测试**与**交叉验收**。

## 快速开始

```bash
git clone https://github.com/Cynthia-cym/meowmoji-test.git
# 测前更新三仓 main，并阅读 environments.md
```
