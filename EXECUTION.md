# 小程序交叉测试 · 执行记录

> **用例 v3：** [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) · **Gate 164** · [TEST-PRIORITY-v3.md](./TEST-PRIORITY-v3.md)  
> **排期：** [TEST-SCHEDULE-v3.md](./TEST-SCHEDULE-v3.md) · **产品路径：** [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md)  
> **飞书：** [feishu-gate-v3.md](./feishu-gate-v3.md) · [Wiki](https://qcnp7uavxd21.feishu.cn/wiki/DqxmwME1giejvgkauWycNXqanRc)

---

## 说明（v3 · 2026-06-03）

- **Gate：** P0(61) + P1(103) = **164**
- **产品：** 真机 P0 一日路径（TEST-ACCEPTANCE-PATHS-v3）
- **Cursor：** 全部 Gate · 开发者工具 + CMS + CI

---

## 执行记录

| 日期 | 执行人 | 环境 | Gate 通过 | 失败/待测 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 2026-05-31 | Cursor | staging | — | v2 编号 | 历史 |
| 2026-06-03 | — | v1.0.10 | — | — | v3 定稿；待按 Gate 重跑 |
| 2026-06-04 | Cursor | staging + CDN | 部分 | 真机待测 | admin-api 49/49；gallery API 3×catalog+4 preview；CDN 200；广场/贴纸 B 待产品真机 |
| 2026-06-05 | Cursor | staging + cms.meowmoji.cn | API ✅ | CMS-C4-007~010 待浏览器 | 三 Tab 后端 rsync+deploy；listing GET 200；Vercel prod；单测 65+30 |
| 2026-06-14 | Cursor | 本机 CDP + staging | 单测 96 ✅ | E2E 待复验 | ADR-017 收敛；修复假成功/只传1张；见 [sticker-automation-test-2026-06.md](./sticker-automation-test-2026-06.md) |
| 2026-06-16 | Cursor | CDP + staging `d6e1d8c` + Vercel prod | listing 单测 ✅ | E2E 待复跑 | work_id=26 附加信息/赞赏 ✅；listing `uploader_bg` 假跳过已修并部署；见 §8 |

---

## 待执行（TEST-SCHEDULE-v3）

| 日 | 内容 | 负责人 |
| --- | --- | --- |
| D1 | P0 主路径 + E2E-001 | 产品真机 + Cursor CMS |
| D2 | P1 小程序逆向/边界/BND | Cursor |
| D3 | P1 CMS + E2E + NFR 冒烟 | Cursor |

**SKIP（P3）：** MP-F10-* · CMS-C4-006 自动提交微信
