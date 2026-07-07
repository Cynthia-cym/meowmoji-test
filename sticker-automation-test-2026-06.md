# Meowmoji 表情上架自动化 · 测试归档（2026-06-14 — 持续更新至 2026-06-18）

> **版本：** ADR-017 · CDP 主线 · submit-result-v2  
> **关联用例：** TEST-SPEC-v3 §十三 CMS-C4-* · [cms-admin-test.md](./cms-admin-test.md)  
> **执行记录：** [EXECUTION.md](./EXECUTION.md)

---

## 1. 测试分工

| 角色 | 范围 |
| --- | --- |
| **Cursor** | CMS 浏览器验收 · 本机 `worker:cdp` 联调 · 单测 `admin-api npm test` |
| **产品（你）** | 微信表情开放平台草稿人工核对 · 「提交审核」人工操作 |
| **不参与** | CMS 后台点测由 Cursor 负责（ADR-011） |

---

## 2. 自动化专项用例（映射 TEST-SPEC-v3）

| 编号 | 名称 | 优先级 | 状态 | 结论 |
| --- | --- | --- | --- | --- |
| CMS-C4-001 | 上架任务列表加载 | P1 | ✅ | API/页面可用 |
| CMS-C4-002 | 审核通过后进入队列 | P1 | ✅ | 入队正常 |
| CMS-C4-003 | CDP 自动提交成功（ADR-018） | P0 | 🟡 | submit-result-v2 已合入；生产库新作品 E2E 待复验 |
| CMS-C4-004 | 任务失败可重试 | P2 | 🟡 | 失败/中断统一「从头开始」；不再保留 checkpoint 续跑 |
| CMS-C4-005 | 状态与小程序同步 | P1 | ⬜ | 待端到端 |
| CMS-C4-007 | 三 Tab 列表加载 | P1 | ✅ | 2026-06-05 |
| CMS-C4-008 | Tab1 详情 Drawer | P2 | 🟡 | 待浏览器 |
| CMS-C4-009 | 插件指南浮层 | P2 | ✅ | 含完整 Runbook §2 |
| CMS-C4-010 | Tab3 刷新容错 | P2 | 🟡 | 待浏览器 |
| AUTO-CDP-001 | `automation:cdp-health` valid | P0 | ✅ | 本机通过 |
| AUTO-CDP-002 | `automation:test-cdp` 最小连通 | P0 | ✅ | 阶段 0–1 |
| AUTO-CDP-003 | 批量上传 N 张贴纸 | P0 | 🟡 | 代码已修；待复验 |
| AUTO-CDP-004 | 版权/横幅/封面/图标 | P0 | 🟡 | 2026-06-16 修 `uploader_bg` 假跳过；待 work_id=26 复跑 |
| AUTO-CDP-005 | 提交成功见 `stiker/result` | P0 | 🟡 | submit-result-v2；work_id=20 微信 ✅；待新作品 CMS `succeeded` |
| AUTO-CDP-006 | CDP 审核状态同步 CMS | P1 | 🟡 | 逻辑在 `sync-status-cdp.js`；**2026-06-23 改由 `platform-sync:cdp` 独立执行**，待复验 |

**SKIP：** CMS-C4-006 自动提交微信（产品范围外）

---

## 3. 小程序 C 端相关（上架链路）

| 编号 | 名称 | 与自动化关系 | 状态 |
| --- | --- | --- | --- |
| MP-F3-014 | 上架状态与 CMS 一致 | 队列完成后状态同步 | ⬜ |
| MP-F3-001~016 | 一键上架 C 端 | 独立 C 端流程；自动化管平台草稿 | 见 TEST-SPEC-v3 §F3 |

C 端全套用例仍以 [TEST-SPEC-v3.md](./TEST-SPEC-v3.md)（**275 条 · Gate 170**）为准；本文件仅增量记录**表情平台上架自动化**专项。

---

## 4. Bug 清单（2026-06-14 会话）

| ID | 现象 | 根因 | 修复 | 验证 |
| --- | --- | --- | --- | --- |
| BUG-AUTO-001 | 只上传 1 张贴纸 | 逐张写 `file input[0]` | 批量上传 + 分区定位 | 待复验 |
| BUG-AUTO-002 | CMS 已保存但平台未保存 | `stillOnDetail` 假阳性 | 须 `saveSuccess` | 待复验 |
| BUG-AUTO-003 | 版权/横幅未填 | 失败不抛错 | 阻断 + 校验 | 待复验 |
| BUG-AUTO-004 | 含义文案错误 | input 选择器过宽 | `sticker-form-helpers` | 待复验 |
| BUG-AUTO-005 | 重试仍跳过上传 | checkpoint 残留 | retry 清空 checkpoint | 待复验 |
| BUG-AUTO-006 | chrome:cdp 不打开平台 | 脚本未传 URL | `start-chrome-cdp.sh` 自动打开 | ✅ |
| BUG-AUTO-007 | 赞赏引导语/图不出现 | 未先勾选「接受赞赏」 | 拆 `tip_accepted` + `fillTipDetails` | ✅ 2026-06-16 |
| BUG-AUTO-008 | 横幅/封面/图标未传仍过步骤 | `plus_gone` / scope 过大误判已上传 | 收紧 scope + 仅信 `uploader_img` | 已修 `d6e1d8c` 待复验 |
| BUG-AUTO-009 | listing 20ms 全跳过 | `uploader_bg` 把空占位 CSS 当预览 | 移除 bg 检测；不可信模式强制重传 | 已修 `d6e1d8c` 待复验 |
| BUG-AUTO-010 | 失败行仍显示「提交上架」 | `listingRowActions.ts` 未 push/部署 | 三态按钮 + Vercel `d6e1d8c` | ✅ CMS 已部署 |
| BUG-AUTO-011 | 附加信息类型选错 | 两处「类型」混淆；label fallback | `platform-meta-type-section.js` | ✅ 2026-06-16 |
| BUG-AUTO-012 | 表情风格「日常」未选中 | WeUI checkbox 需 evaluate 点 label | `clickCheckboxViaEvaluate` | ✅ 2026-06-16 |
| BUG-AUTO-013 | 横幅 filechooser 15s 超时 | CDP 无原生 filechooser | `setInputFiles` 优先 | ✅ 2026-06-16 |
| BUG-AUTO-014 | 提交前 verify 假失败 listing | DOM 复检与 upload settled 不一致 | 移除 `verifyDraftFormComplete` 门禁 | ✅ submit-result-v2 |
| BUG-AUTO-015 | `legacyValidationHint` 报未填项 | 全页 innerText 命中说明文案 | 仅信可见校验控件 | ✅ submit-result-v2 |
| BUG-AUTO-016 | 微信成功 CMS 失败 | 跳转 `stiker/result` 后 evaluate 炸 | `clickSubmitAndWaitForResult` + `waitForURL` | ✅ 代码已合入；待明日 E2E |

---

## 5. 单元测试

```bash
cd Meowmoji-CMS01/admin-api && npm test
```

2026-06-14：**96 pass**（含 checkpoint、upload-state、sticker-form-helpers、cookie-bridge 历史用例）

---

## 6. 测试结论

| 维度 | 结论 |
| --- | --- |
| 架构 | CDP 主线代码与文档已收敛；云 headless / Puppeteer 主路径 **已证伪并禁用** |
| CMS 队列/UI | ✅ 可用 |
| 审核状态同步 | 🟡 **独立脚本** `platform-sync:cdp` 待复验（ADR-021） |
| 端到端直接提交 | 🟡 **P0 待复验**（submit-result-v2；生产库新作品） |
| Gate | 表情自动化不单独 Gate；归入 CMS-C4-003 P0 |

---

## 7. 下一步测试（2026-06-23 更新）

1. `worker:cdp` → `automationRev: 2026-06-23-cdp-submit-only-v1`（无启动 sync）
2. 生产库 **新作品** E2E → CMS `succeeded`
3. `platform-sync:cdp` 独立调试
4. 回填 [EXECUTION.md](./EXECUTION.md)

---

## 8. E2E 执行记录（2026-06-15 ~ 2026-06-16 · work_id=26 小野的日常2）

| 时间 | 步骤 | 结果 | 备注 |
| --- | --- | --- | --- |
| 06-15 | stickers_uploaded / meanings_filled | ✅ | 8 张 + 含义 |
| 06-15 | album_basic_filled / copyright_filled | ✅ | |
| 06-15 | listing_assets_uploaded | ❌ 假成功 | 日志 `listing upload skipped existing` · mode=`uploader_bg`；页面横幅/图标仍空 |
| 06-15 | platform_additional_filled | 🟡→✅ | 类型/角色曾失败；`platform-meta-type-section` 后通过 |
| 06-16 | platform_meta_style/theme/… | ✅ | 「日常」evaluate 点选 |
| 06-16 | tip_accepted / tip_filled | ✅ | 引导语+引导图+致谢图 |
| 06-16 | finalize | ❌ | `草稿表单未完成：横幅、封面、图标`（与 listing 假跳过一致） |
| 06-16 | CMS 按钮 | ❌→✅ | 失败行误显「提交上架」→ `d6e1d8c` Vercel 部署后应消失 |

**部署锚点：** commit `d6e1d8c` · Vercel `dpl_5ktn2LCvhHJuV469aKnVf4Fd13AQ` · staging healthz OK

**Trace（失败样例）：** `/tmp/meowmoji-automation/26/trace-1781591146934.zip`

---

## 9. E2E 执行记录（2026-06-16 · work_id=20 测试002 · submit-result-v2 调试）

| 时间 | 步骤 | 结果 | 备注 |
| --- | --- | --- | --- |
| 06-16 | listing 横幅/封面/图标 | ✅ | `upload_settled` · `no_preview` + plus gone |
| 06-16 | platform_additional_filled | ✅ | 卡通表情/其他 · 日常 · 万能通用 |
| 06-16 | tip_accepted / tip_filled | ✅ | 引导图+致谢图 |
| 06-16 | skip pre-submit verify | ✅ | 脚本不拦提交 |
| 06-16 | 微信提交 | ✅ | 页面 `stiker/result`「表情提交成功，请耐心等待审核」 |
| 06-16 | CMS worker | ❌→修 | `Execution context was destroyed`；submit-result-v2 修复 |

**留档：** [docs/archive/2026-06-sticker-listing-submit-debug/README.md](../docs/archive/2026-06-sticker-listing-submit-debug/README.md)  
**代码指纹：** `automationRev: 2026-06-16-submit-result-v2`  
**明日复验：** 新作品 + 确认 `[automation-worker] succeeded` 与「已提交表情商店」Tab

---

## 10. CDP 审核状态同步（2026-06-17 · cdp-review-sync-v1）

> **2026-06-23 起：** 周期同步已迁至 **`platform-sync:cdp`**（ADR-021）；本节 worker 顺序为**历史口径**。

| 项 | 结果 |
| --- | --- |
| 脚本 | `sync-status-cdp.js` · 现由 `platform-sync:cdp` 调用 |
| 历史 Worker 顺序 | ~~启动/周期 syncStatuses~~ → 已移除 |
| 指纹（现行） | `platformSyncRev: 2026-06-23-platform-sync-v1` |
| 验证（2026-06-17） | scraped 5/9 albums；`node --test test/automation-cdp-config.test.js` 3 pass |
| 关联 | [CHANGELOG](../docs/live/CHANGELOG.md) · AUTO-CDP-006 |

---

## 11. 2026-06-21 ~ 2026-06-23 会话增量（worker 拆分 · CMS · retry）

| ID | 现象 | 根因 | 处置 | 验证 |
| --- | --- | --- | --- | --- |
| BUG-AUTO-017 | CMS「从头开始」5000 | `retryAutomationTask` LATERAL + 裸 `FOR UPDATE` | `FOR UPDATE OF t`（`fb55fd6`） | ✅ 用户确认可点 |
| BUG-AUTO-018 | `worker:cdp` 启动 fatal `page closed` | worker 内嵌 `syncStatuses` 抢 CDP | **ADR-021** 拆 `platform-sync:cdp` | 🟡 worker 应可启动；同步待独立调试 |
| BUG-AUTO-019 | CDP mutex / 启动顺序修补 | 会话内实验未提交 | **已撤销**；勿回退 | — |
| BUG-AUTO-020 | 任务「假成功已降级」 | `platformVerified` 未过关 / 微信草稿不完整 | 删微信草稿 + 从头开始 | 运营流程 |

| 项 | 指纹 / 命令 |
| --- | --- |
| 上架 worker | `2026-06-23-cdp-submit-only-v1` · `npm run worker:cdp` |
| 微信表情数据同步 | `2026-06-23-platform-sync-v1` · `npm run platform-sync:cdp` |
| 已验证不可行 | worker 启动+周期 `syncStatuses`；云 headless；Puppeteer launch 主路径 |

**下一步测试：** ① `worker:cdp` 生产库新作品 E2E；② `platform-sync:cdp` 单 tab「我的表情」抓取；③ 回填 [EXECUTION.md](./EXECUTION.md)
