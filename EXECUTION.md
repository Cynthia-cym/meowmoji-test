# 小程序交叉测试 · 执行记录

> **用例 v3：** [TEST-SPEC-v3.md](./TEST-SPEC-v3.md) · **Gate 170** · [TEST-PRIORITY-v3.md](./TEST-PRIORITY-v3.md)  
> **排期：** [TEST-SCHEDULE-v3.md](./TEST-SCHEDULE-v3.md) · **产品路径：** [TEST-ACCEPTANCE-PATHS-v3.md](./TEST-ACCEPTANCE-PATHS-v3.md)  
> **上线跟进：** [LAUNCH-TRACKING.md](../docs/live/LAUNCH-TRACKING.md)（**Gate3/Gate4** · v1.0.51 已全量）  
> **文档漂移整改：** [TEST-DOC-DRIFT-REMEDIATION.md](./TEST-DOC-DRIFT-REMEDIATION.md)  
> **飞书：** [feishu-gate-v3.md](./feishu-gate-v3.md) · [Wiki](https://qcnp7uavxd21.feishu.cn/wiki/DqxmwME1giejvgkauWycNXqanRc)

---

## 说明（v3 · 2026-06-03）

- **Gate：** P0(62) + P1(108) = **170**
- **产品：** 真机 P0 一日路径（TEST-ACCEPTANCE-PATHS-v3）
- **Cursor：** 全部 Gate · 开发者工具 + CMS + CI

---

## 执行记录

| 日期 | 执行人 | 环境 | Gate 通过 | 失败/待测 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 2026-05-31 | Cursor | staging | — | v2 编号 | 历史 |
| 2026-06-03 | — | v1.0.10 | — | — | v3 定稿；待按 Gate 重跑 |
| 2026-06-23 | Cursor | production+staging deploy | ✅ | — | `041b788` ADR-022 · api/stg-api healthz OK |
| 2026-06-04 | Cursor | staging + CDN | 部分 | 真机待测 | admin-api 49/49；gallery API 3×catalog+4 preview；CDN 200；~~广场/贴纸 B 待产品真机~~ → 贴纸 ADR-022 关闭 MVP 验收 |
| 2026-06-05 | Cursor | staging + cms.meowmoji.cn | API ✅ | CMS-C4-007~010 待浏览器 | 三 Tab 后端 rsync+deploy；listing GET 200；Vercel prod；单测 65+30 |
| 2026-06-14 | Cursor | 本机 CDP + staging | 单测 96 ✅ | E2E 待复验 | ADR-017 收敛；修复假成功/只传1张；见 [sticker-automation-test-2026-06.md](./sticker-automation-test-2026-06.md) |
| 2026-06-16 | Cursor | CDP + staging `d6e1d8c` + Vercel prod | listing 单测 ✅ | E2E 待复跑 | work_id=26 附加信息/赞赏 ✅；listing `uploader_bg` 假跳过已修并部署；见 §8 |
| 2026-06-17 | Cursor | CDP + staging `af137e6` | 审核同步单测 ✅ | E2E 待新作品 | `sync-status-cdp.js`；见 [sticker-automation-test-2026-06.md](./sticker-automation-test-2026-06.md) §10 |
| 2026-06-18 | Cursor | 本机单测 + **产品真机** | UI003 全项 ✅ | — | 制作 Figma 对齐 · 模板圆角/角标 · 软萌可爱 COS；见 CHANGELOG ui003-figma-template-v2 |
| 2026-06-20 | 产品 | 飞书 Wiki | — | — | **TEST-SPEC v3 + 飞书 Gate P0 主路径** 产品确认完成 · 工作项关闭 |
| 2026-06-18 | Cursor | 体验版 **v1.0.29** upload | check:all ✅ | 真机待测 | ADR-019+C5+UI003 收敛 `88c6337`；见 [archive](../docs/archive/2026-06-18-session-end/README.md) |
| 2026-06-18 | Cursor | staging `45609f0` | **MP-F3-014 API ✅** | C 端真机待拉代码 | ADR-019 `userDisplayStatus`；A1→A2→B2 联调 PASS（workId=39）；附 fix 审核后清 `works:list` 缓存 |

---

## MP-F3-014 · 上架状态与 CMS 一致（2026-06-18）

> **真源：** [listing-status-model.md](../Meowmoji-CMS01/docs/ai/listing-status-model.md) §9.6 · **脚本：** `meowmoji-test/scripts/mp-f3-014-user-display-status.mjs`

| 步骤 | CMS 操作 | 期望 `userDisplayStatus` | 期望文案 | staging API |
| --- | --- | --- | --- | --- |
| A1 | 用户提交 | `pending_review` | 审核中 | ✅ workId=39 |
| A2 | 管理员通过 | `approved_waiting` | 待上架 | ✅ |
| B2 | 批量提交上架 | `submitting` | 提交中 | ✅ |

- **部署：** `a37bb04`（ADR-019 API）+ `5aaf923`（审核/入队后失效 `works:list` Redis 缓存）
- **C 端：** `WeChatProjects/meowmoji` 已改 `userDisplay*` 展示（本地未 push）；体验版接 staging 后需产品真机刷新「我的作品」复核 UI

---

## 小程序 UI003 插图专项（2026-06-18）

> **功能 spec：** [Meowmoji--mini-program/ai.md](../Meowmoji--mini-program/ai.md) §3.1 · **变更：** [CHANGELOG](../docs/live/CHANGELOG.md) ui003-create-templates-upload-v1

### 用例映射（增量 · 非 Gate）

| 编号 | 场景 | 单测文件 | 状态 |
| --- | --- | --- | --- |
| MP-UI-001 | 状态/空态插图 400rpx + widthFix | `state-illustrations.test.mjs` | ✅ |
| MP-UI-002 | cover-image 等比高度 | `illustration-size.test.mjs` | ✅ |
| MP-UI-003 | 制作页 hero 680rpx CDN | `home-create-layout.test.mjs` | ✅ |
| MP-UI-004 | 上传页文案/底色/角标结构 | `upload-ui003-layout.test.mjs` | ✅ |
| MP-UI-005 | 通知「新」读后 sync | `notification-badge.test.mjs` | ✅ |

### Bug 清单

| ID | 现象 | 根因 | 修复 | 验证 |
| --- | --- | --- | --- | --- |
| MP-UI-B01 | 插图被压扁 | 固定高 + aspectFit | widthFix，仅写宽度 | 单测 ✅ |
| MP-UI-B02 | 制作 hero 太小 | 紫色小图标 preview | `createHomeHero` 680rpx | 单测 ✅ · 真机 🟡 |
| MP-UI-B03 | 上传页灰底 | `git checkout HEAD` 误回退 | `#F8F7FF` 拖放区 + 白页面 | 单测 ✅ · 真机 🟡 |
| MP-UI-B04 | 模板角标错/不显示 | v1 SVG `<mask>` | v2 无 mask · `template-selected.svg` | 单测 ✅ · COS+真机 🟡 |
| MP-UI-B05 | 通知「新」不消失 | 读后未写 snapshot | `syncNotificationSnapshot` | 单测 ✅ · 真机 🟡 |

### 执行命令

```bash
cd /Users/xxll/Desktop/CMS/WeChatProjects/meowmoji
node --test tests/illustration-size.test.mjs \
  tests/state-illustrations.test.mjs \
  tests/notification-badge.test.mjs \
  tests/upload-ui003-layout.test.mjs \
  tests/home-create-layout.test.mjs
```

### 结论

- **代码+单测+真机：** 制作 Figma 对齐、模板圆角/角标、软萌可爱预览均已验收（2026-06-18 · CHANGELOG `ui003-figma-template-v2`）。
- **缓存：** `20260618templatecute`（含软萌可爱预览图 bump）。

---

## 待执行（TEST-SCHEDULE-v3）

| 日 | 内容 | 负责人 |
| --- | --- | --- |
| D1 | P0 主路径 + E2E-001 | 产品真机 + Cursor CMS |
| D2 | P1 小程序逆向/边界/BND | Cursor |
| D3 | P1 CMS + E2E + NFR 冒烟 | Cursor |

**SKIP（P3）：** MP-F10-* · CMS-C4-006 自动提交微信
