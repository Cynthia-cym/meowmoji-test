# 测试追溯矩阵（P0 验收 ↔ 自动化）

> **维护：** Cursor · **原则：** [test.md](./test.md)  
> **状态：** 初稿 · 2026-06-03 · 随用例/单测变更同步

---

## 说明

| 符号 | 含义 |
| --- | --- |
| ✅ | 已有自动化覆盖 |
| 🟡 | 部分覆盖（需手工补证） |
| ❌ | 仅验收层，无自动化 |
| — | 不适用（纯 UI/真机） |

---

## 小程序 P0（节选 · 待扩充）

| 验收 ID | 名称 | 单元/集成 | 文件 |
| --- | --- | --- | --- |
| MP-AUTH-001 | 邮箱登录 | 🟡 | admin-api OTP 相关 test |
| MP-UPL-001~006 | 上传 8–16 | 🟡 | `admin-api/test/user-upload.test.js` |
| MP-GEN-001~003 | 三模板生成 | ✅ | `admin-api/test/generation.test.js` |
| MP-GEN-005 | 文案轮换 | ✅ | `admin-api/test/captions.test.js` |
| MP-PUB-001 | 上架提交 | 🟡 | listing/submit 相关 |
| MP-E2E-001 | 全链路 | ❌ | 仅真机+CMS 手工 |

---

## CMS P0（节选）

| 验收 ID | 名称 | 单元/集成 | 文件 |
| --- | --- | --- | --- |
| CMS-AUTH-001 | 管理员登录 | 🟡 | admin-api auth test |
| CMS-APP-003 | 审核通过 | ❌ | 浏览器手工 |
| CMS-PUB-001 | 上架任务 | 🟡 | `puppeteer.provider.test.js` |

---

## 待办

- [ ] 补全 44 条 P0 全表
- [ ] CI 门禁：P0 有 ✅ 的模块 PR 不得降低覆盖率
