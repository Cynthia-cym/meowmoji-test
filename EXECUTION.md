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
| 2026-06-17 | Cursor | CDP + staging `af137e6` | 审核同步单测 ✅ | E2E 待新作品 | `sync-status-cdp.js`；见 [sticker-automation-test-2026-06.md](./sticker-automation-test-2026-06.md) §10 |
| 2026-06-18 | Cursor | 本机单测 | UI003 单测 ✅ | 真机待验 | 制作 hero / 上传样式 / 通知角标；见下 §小程序 UI003 |

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

- **代码+单测：** 插图尺寸规则、通知角标、上传结构已收敛；缓存版本 `20260618templateimg`。
- **待产品真机：** hero 视觉、上传 `#F8F7FF`、紫色角标+白勾、通知「新」读后消失（清开发者工具缓存或 bump 版本后预览）。

---

## 待执行（TEST-SCHEDULE-v3）

| 日 | 内容 | 负责人 |
| --- | --- | --- |
| D1 | P0 主路径 + E2E-001 | 产品真机 + Cursor CMS |
| D2 | P1 小程序逆向/边界/BND | Cursor |
| D3 | P1 CMS + E2E + NFR 冒烟 | Cursor |

**SKIP（P3）：** MP-F10-* · CMS-C4-006 自动提交微信
