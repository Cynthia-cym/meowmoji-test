# 小程序交叉测试 · 执行记录

> **用例规格（Codex 维护）：** [miniprogram-test.md](./miniprogram-test.md)  
> **策略（Live）：** [../docs/live/TEST.md](../docs/live/TEST.md)

---

## 说明

- **Codex** 编写/更新 MP-xxx 用例步骤与预期
- **Cursor** 在本文件记录每次执行结果
- 重大里程碑同步 [docs/live/CHANGELOG.md](../docs/live/CHANGELOG.md)

---

## 执行记录

| 日期 | 执行人 | 环境 | 通过 | 失败/待测编号 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 2026-05-31 | Cursor | staging API | 部分 | MP-142~145、MP-150~154 | MP-140/141/144/155 后端闭环；真机待测 |
| 2026-06-01 | — | — | — | — | 文档治理切换；用例仍见 miniprogram-test.md |

---

## 当前待执行（P0）

- MP-142、MP-143、MP-145（站内消息真机）
- MP-150~154（订阅/服务通知）
- MP-050~056、120~123（上架链路，依赖 Puppeteer live）
