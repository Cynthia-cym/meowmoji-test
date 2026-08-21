# 小程序交叉测试 · 执行记录（只追加）

> **现行策略：** ADR-026 上线后增量测试；[README.md](./README.md) 与 [test.md](./test.md) 为现行入口。
> **历史：** v3 Gate、排期和飞书镜像已冻结；下方既有记录不可改写。

---

## 新增迭代记录格式（只追加到本文末尾）

```markdown
## YYYY-MM-DD · <版本/迭代>

- **改动范围：** …
- **自动化：** `<命令>` → <结果>
- **真实链路证据：** <环境、时间窗、请求/日志/数据证据；不适用须说明>
- **产品真机结论：** 通过 | 不通过 | 不适用（原因）
- **未覆盖项：** 无 | …
- **负责人：** …
- **结论：** 通过（改动测试全绿 + 无未关闭受影响 P0 + 必要时产品确认） | 未通过
```

## v3 历史说明（冻结，勿改）

- **Gate：** P0(62) + P1(108) = **170**
- **产品：** 真机 P0 一日路径（TEST-ACCEPTANCE-PATHS-v3）
- **Cursor：** 全部 Gate · 开发者工具 + CMS + CI

---

## v3 历史执行记录（冻结，勿改）

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

---

## 2026-08-11 · 上线后埋点恢复预发布增量验证

- **版本/迭代基线：** Mini 体验版基线 `v1.0.59` @ `0c8558a`；生产 API 基线 `b44ae51`；本地恢复 branches 均为 `codex/post-launch-analytics-20260811`（CMS `07cfa38`、Mini `40431ec`、Test `0526fd5`，本记录提交前）。
- **改动范围：** 小程序埋点稳定 `eventId`、脱敏诊断、有限重试与批次确认；API 幂等接收/结构化计数/90 天保留；Dashboard 过时测试维护；CMS/Mini 增量 CI；五份 live smoke/恢复脚本移除弱默认密码。本轮没有版本 bump、部署、体验版上传或公众平台配置变更。
- **路径占位：** `<workspace>` 为 CMS 根工作区；`<cms-worktree>` 为本轮 CMS feature checkout（提升后对应 `<workspace>/Meowmoji-CMS01`）；`<mini-worktree>` 为本轮 Mini feature checkout（提升后对应 `<workspace>/WeChatProjects/meowmoji`）。
- **自动化：**
  - `cd <cms-worktree>/admin-api && npm test` → 259/259，exit 0。
  - `cd <cms-worktree>/admin-web && npm test && npm run build` → 57/57，build exit 0。
  - `cd <mini-worktree> && npm run check:all` → runtime/assets/mini/cloud checks 通过，194/194，exit 0。
  - `cd <mini-worktree> && git ls-files -z '*.js' '*.mjs' '*.cjs' | xargs -0 -n1 node --check` → 全部 tracked JS syntax exit 0。
  - `cd <cms-worktree>/admin-api && node --test test/analytics-sanitize.test.js test/mini-analytics-route.test.js test/analytics-retention.test.js` → API analytics 23/23。
  - `cd <mini-worktree> && node --test tests/analytics.test.mjs` → Mini analytics 23/23。
  - `cd <cms-worktree>/admin-api && node --test test/credential-script-contract.test.js` → credential contract 19/19。
  - `cd <cms-worktree> && ruby -e 'require "yaml"; ARGV.each { |path| YAML.load_file(path) }' .github/workflows/ci.yml <mini-worktree>/.github/workflows/ci.yml` → workflow YAML 2/2 parsed。
  - `cd <cms-worktree> && bash -n deploy/cms/smoke-staging.sh deploy/cms/smoke-vercel.sh deploy/cms/smoke-error-handling.sh deploy/cms/orca-staging-update.sh` → Bash syntax 4/4。
  - `cd <workspace> && ./scripts/doc-consistency-check.sh` → PASS。
- **真实链路证据：** 2026-08-11 只读 SSH 确认正确生产主机/容器与 `current_database()=meowmoji`，`analytics_events=0`；全部保留生产 Nginx access log 对 `/api/mini/analytics` 命中 0；fresh production healthz 返回 `code=0`、`deployTarget=production`、`status=ok`。该基线仅排除 H6 与保留历史窗口内 H5，不证明埋点恢复。
- **产品真机结论：** **pending**；尚未上传包含本轮诊断的新体验版，也未执行新版本“冷启动→首页→上传页”。
- **未覆盖项：** 用户授权后三仓 feature branches 提升/push `main`；从 CMS `main` 部署兼容 API/schema；从 Mini `main` bump 并上传新体验版；确认公众平台 `request` 合法域名含 `https://api.meowmoji.cn`；同一 `session_id + 时间窗` 串联客户端诊断、生产 Nginx 2xx、DB 精确事件与 Dashboard 增量。
- **负责人：** Cursor（本地实现/自动化/运行时取证）· 产品（授权后的真机操作与确认）。
- **结论：** **PARTIAL / local ready**。自动化与本地构建已绿，但埋点 P0 仍是唯一发布阻塞；本记录不得标为 PASS 或 closed。

## 2026-08-12 · 埋点恢复发布进度（真机前）

- **版本/迭代：** CMS production `b4878ac`；Mini v1.0.60 `e8b9af8`；Test `8d4918b`。
- **改动范围：** 三仓 clean/non-diverged 检查后 `git merge --ff-only` 提升并 push `main`；生产 API/schema 标准部署；v1.0.60 体验版上传。本条不包含产品真机结论。
- **自动化与部署验证：** CMS API 259/259；Mini `check:all` 194/194 + tracked JS/MJS/CJS syntax；production healthz=`production/ok`；`event_id` 列、部分唯一索引 `valid/ready/unique`、retention 启动均通过。
- **真实链路基线：** 空请求路由返回受控 HTTP 400 且未写入事件；2026-08-12 11:12 +08:00 真机操作前 `current_database()=meowmoji`、`analytics_events=0`。CLI upload v1.0.60 成功，TOTAL 411.0 KB、main 288.3 KB。
- **产品真机结论：** **pending**；等待产品在 v1.0.60 执行“冷启动→首页→上传页”，提供分钟级时间、设备/网络和脱敏 `[analytics-diag]`。
- **未覆盖项：** 公众平台 `request` 合法域名确认；同一 session/time window 的 Nginx 2xx、DB 精确事件、Dashboard 可解释增量。
- **负责人：** Cursor（部署/取证）· 产品（真机操作/域名确认）。
- **结论：** **PARTIAL / deployed + trial ready**。P0 保持打开，不得标为 PASS 或 closed。

## 2026-08-14 · 底部 Tab 未选中图标原路径更新

- **版本/迭代：** Mini `main@684b169`；包版本仍为 **1.0.60**，本轮未重新上传小程序版本。
- **改动范围：** 仅替换「广场 / 制作 / 我的」3 个 inactive SVG，并增加 fail-closed COS 上传、全量备份、歧义失败回滚与脱敏错误契约；3 个 active SVG、manifest、查询参数 `20260625tab` 与运行绑定不变。
- **路径占位：** `<workspace>` 为 CMS 根工作区；`<mini>` 为 `<workspace>/WeChatProjects/meowmoji`。
- **自动化：**
  - `cd <mini> && node --test tests/custom-tab-bar.test.mjs tests/tab-icon-upload-contract.test.mjs tests/tab-icon-upload-cli.test.mjs` → 54/54，exit 0。
  - `cd <mini> && npm run check:all` → 246/246，exit 0。
  - `cd <mini> && git ls-files -z -- '*.js' '*.mjs' '*.cjs' | xargs -0 -n1 node --check` → tracked JS/MJS/CJS syntax exit 0。
  - `cd <mini> && bash -n scripts/cos-upload-tab-icons.sh` → exit 0。
  - `cd <mini> && shasum -a 256 custom-tab-bar/icon-*.svg` → 3 inactive 为 `0b0cf725…030e` / `ed72a0ef…2c09` / `f57b461f…a2cd`；3 active 保持 `c931103a…64f4` / `29be233a…635c` / `038f8184…6efd`。
- **真实链路证据：** 首次 COS 备份因 SecretId / SecretKey 配对错误返回 403，且零写；匹配后 3 个对象备份/PUT 成功。COS 源站逐对象 HTTP 200 且哈希精确匹配；首次 CDN 为 2 旧 / 1 新，精确 URL 刷新任务 `632809032536591411` 提交后，3 条 CDN 当前查询 URL 均 HTTP 200 且匹配批准哈希。
- **产品真机结论：** **pending**；需完全关闭并重开小程序，依次切换「广场 / 制作 / 我的」，确认未选中图标已替换、选中图标未变，且无裁切、偏移或文字变化。
- **未覆盖项：** 产品真机视觉验收；本轮不需要新版本上传或微信审核。
- **负责人：** Cursor（实现、自动化、COS/CDN 发布与取证）· 产品（真机目视确认）。
- **结论：** **PARTIAL / infrastructure propagated**。本地、COS 源站与 CDN 证据已闭环；用户可见行为仍待产品真机确认。

## 2026-08-21 · v1.0.61 游客浏览与广场首屏加载

- **版本/迭代：** production API `9358ca2`；Mini `ba52c08`；体验版 **v1.0.61**。
- **改动范围：** 广场 loading/empty 状态分离；游客访问三个 Tab 首页；点赞弹窗登录、制作按钮登录、“我的”二级入口登录；API 游客只读广场列表。
- **自动化：** `admin-api npm test` → 276 pass、2 skip；Mini `npm run check:all` → 251/251；针对性 Mini 67/67、API 7/7。
- **真实链路证据：** 标准生产脚本部署成功，production healthz 正常；未注册 OpenID 请求 `/internal/gallery/works` 返回 `guest_gallery=ok`；CLI upload v1.0.61 成功，TOTAL 411.7 KB、main 289.0 KB。
- **产品真机结论：** pending；等待验证游客冷启动、三 Tab 浏览、点赞弹窗及登录跳转。
- **未覆盖项：** 产品真机视觉与交互确认；未提交微信审核或发布正式版。
- **负责人：** Codex（实现、自动化、部署、体验版上传）· 产品（真机验收）。
- **结论：** **PARTIAL / deployed + trial ready**。自动化和生产 API 已通过，用户可见行为待产品确认。

## 2026-08-21 · v1.0.62 广场首屏 Skeleton

- **版本/迭代：** Mini `3f20061`；体验版 **v1.0.62**。
- **改动范围：** 广场首次 loading 增加 4 张 2×2 同构卡片骨架与低对比度扫光；继续保留「正在加载中」，且与成功空态互斥；补充 Mini `docs/design.md` Skeleton 规范。
- **自动化：** `node --test tests/guest-access.test.mjs` → 5/5；`npm run check:all` → 251/251；微信开发者工具 CLI preview 编译成功。
- **真实链路证据：** CLI upload v1.0.62 成功，TOTAL 414.4 KB、main 291.7 KB；本轮无 API、生产服务或 COS 变更。
- **产品真机结论：** pending；需在弱网或首次进入广场时确认骨架可见、扫光自然、数据返回后无明显跳动，且空结果只展示空态。
- **未覆盖项：** 产品真机视觉与状态切换确认；未提交微信审核或发布正式版。
- **负责人：** Codex（实现、自动化、体验版上传）· 产品（真机验收）。
- **结论：** **PARTIAL / trial ready**。自动化与上传通过，用户可见动效待产品确认。

## 2026-08-21 · v1.0.63 欢迎页、登录返回与配额

- **版本/迭代：** Mini **v1.0.63**；CMS/API 本轮发布提交待生成。
- **改动范围：** 欢迎页背景与按钮同步显示；登录页透明导航返回；普通用户每日生成 10 次、提交上架 5 次；隐藏剩余次数说明，达限使用指定弹窗文案。
- **自动化：** Mini `npm run check:all` → 251/251；API `npm test` → 276 pass、2 skip。
- **数据迁移：** 仅把生产已确认的旧默认 `daily_submit_limit=3/version=1` 更新为 5，并同步未降额且旧值为 5 的用户到 10；部署后需复核聚合值。
- **产品真机结论：** pending；体验版上传后验证欢迎页同步出现、登录返回，以及生成/提交达限弹窗。
- **结论：** **LOCAL PASS / deployment pending**。
