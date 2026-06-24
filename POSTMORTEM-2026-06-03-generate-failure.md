# 排查复盘：体验版 v1.0.10 生成失败（2026-06-03）

> 关联缺陷：`feishu-test-issue-log.md` #2 · 修复版本 **v1.0.11**

---

## 1. 现象

- 体验版 v1.0.10 真机：上传 8 张猫照 → 点「开始生成」→ 转圈约 2 秒 →「小工厂暂时卡壳啦 / 服务器这会儿有点忙」
- 模板 A 预览图显示灰色占位猫头（B/C 有图）

---

## 2. 真实根因（证据链）

| 层级 | 事实 | 证据 |
| --- | --- | --- |
| **用户侧** | 失败发生在「生成 processing 页刚开始」，不是长时间等待 | 用户反馈 + Nginx 同一秒 8 条 401 |
| **服务端** | `/api/mini/upload` 全部 **401**，`jscode2session` 报 **invalid appsecret** | Nginx `20:01:34` ×8 POST 401；Secret 修复后变为 `invalid code` |
| **云函数** | **未被调用** | `tcb fn log qualityCheck/generateStickers` 该时段无记录；Nginx 无 `/internal/generation/*` |
| **客户端** | 上传错误 `WX_CODE2SESSION_FAILED` 被 catch 映射成 `GENERATE_FAILED` | `processing/index.js` 仅识别 `UPLOAD_TO_CLOUD_FAILED` |
| **并发** | v1.0.10 8 张图 **并行复用同一个 wx.login code**（code 单次有效） | `upload.js` 原 `Promise.all` + `sharedWxCode` |

**一句话：staging 的 `WX_APP_SECRET` 错误导致直连上传立刻 401；文案把「上传失败」伪装成「服务器忙」；云函数与生图 API 从未参与。**

---

## 3. 排查为何走偏（过程根因）

> **通用方法论：** [docs/live/BUG-INVESTIGATION-PRINCIPLES.md](../docs/live/BUG-INVESTIGATION-PRINCIPLES.md)（五阶段排查法 + 错误文案分类原则）

| 偏差 | 说明 |
| --- | --- |
| **先假设后取证** | 早期用「云函数 15s 超时」「队列满」解释，未先查 Nginx / 云函数日志 |
| **误读 healthz** | API `healthz` 正常 ≠ 上传链路（依赖 `WX_APP_SECRET`）正常 |
| **混淆体验版与预览** | 体验版与预览共用云函数，但 v1.0.10 失败点在 **直连 upload**，与是否体验版无关 |
| **空 DB 误导** | 曾查错 Postgres 实例（`meowmoji-admin-api` vs `meowmoji-cms-api`），得到「无 openid 用户」假象 |
| **UI 文案不可信** | 「服务器忙」是多种错误的统一兜底，不能作为排障依据 |

**正确路径（本次最终采用）：**

1. 对齐用户操作时间 → **Nginx access.log**（`mini/upload` / `internal/generation`）
2. 对齐同一时段 → **CloudBase 云函数 log**（`emailOtp` 有、`generateStickers` 无）
3. 容器内验证 `exchangeWxLoginCode` → 确认 Secret 问题
4. 再改代码 / 配置

---

## 4. 已实施修复（v1.0.11）

| 项 | 动作 |
| --- | --- |
| P0 | staging `WX_APP_SECRET` 更新 + 重启 `cms-api` / `automation-worker` |
| P0 | 直连上传改为 **逐张串行 + 每张新 wx.login code** |
| P1 | 上传类错误统一走「照片上传失败」文案，不再误报「服务器忙」 |
| P1 | `generateStickers` 云函数 internal HTTP 超时 15s → 55s |
| P2 | 模板 A/B/C 本地占位图三份同源，待设计替换后 `npm run cos:upload-static` |

---

## 5. 预防方案（避免再犯）

### 5.1 真机失败标准排障顺序（强制）

```
用户报「生成失败」
  ① Nginx：该时段是否有 POST /api/mini/upload（状态码？）
  ② Nginx：是否有 /internal/generation/quality-check | generate
  ③ CloudBase：qualityCheck / generateStickers 同时间段 log
  ④ 若 ① 失败 → 查 WX_APP_SECRET、wxCode 并发、uploadFile 域名
  ⑤ 若 ③ 有、④ 无 → 再查生图超时、队列、贴纸校验
  ⑥ 不以失败页文案、healthz 单独作为结论
```

**命令备忘（staging 服务器）：**

```bash
# 按 IP / 时间筛上传
sudo grep "mini/upload" /var/log/nginx/access.log | tail -30

# 云函数（替换时间窗）
cd WeChatProjects/meowmoji
npx tcb fn log generateStickers -e meowmoji-d5gnscog5e6e87ad2 \
  --startTime "YYYY-MM-DD HH:MM:SS" --endTime "YYYY-MM-DD HH:MM:SS" --limit 20
```

### 5.2 发布 / 密钥检查清单（ADR-006 直连上传）

发布体验版前 **Cursor 必跑**：

- [ ] staging `docker exec meowmoji-cms-api`：`WX_APP_SECRET` 存在且 `jscode2session` 测为 `invalid code`（非 `invalid appsecret`）
- [ ] 微信公众平台 **uploadFile** 合法域名含 `stg-api.meowmoji.cn`
- [ ] 小程序包 `config.api.useDirectUpload === true` 且 `baseUrl` 指向 staging
- [ ] 云函数 `INTERNAL_API_SECRET` 与 staging `CMS_INTERNAL_API_SECRET` 一致（登录/emailOtp 冒烟）
- [ ] 体验版 upload 后：Nginx 至少 1 条 `mini/upload` **200**（可用测试账号真机 1 张图）

**Secret 重置后必做：**

```bash
export WX_APP_SECRET='...'   # 仅服务器 shell，勿提交 git
./deploy/cms/patch-wx-app-secret.sh
cd deploy/cms && docker compose --env-file .env up -d cms-api cms-automation-worker
```

### 5.3 可观测性改进（建议排期）

| 改进 | 目的 |
| --- | --- |
| admin-api 对 `/api/mini/upload` 打结构化 log（openid 摘要、失败 code） | 不依赖 Nginx 猜 body |
| 小程序 processing 页 `mm_generation_failure` 写入 `requestId` + 原始 `err.code` | 用户截图即可定位 |
| `npm run check:staging-wx-secret` 脚本（curl + 假 code 断言） | 发布门禁 |
| 监控：Nginx 5xx/401 on `mini/upload` 告警 | Secret 失效早发现 |

### 5.4 文案与测试

- 失败页 **禁止** 把 upload / wx 鉴权 / 未识别错误映射为「服务器忙」
- 分类真源：`errors.js` · `FailureCategory` · [BUG-INVESTIGATION-PRINCIPLES.md §3](../docs/live/BUG-INVESTIGATION-PRINCIPLES.md#3-错误文案分类映射原则长期)
- 用例补充：**MP-402 子项**「直连 upload 401 → 应提示照片上传失败」
- 用例补充：**未知 err.code** → 应提示「出了点问题」，不得显示「服务器忙」
- 回归：**8 张串行 upload** 在 staging 产生 8 条 200（可自动化 curl + mock 或真机）

### 5.5 模板预览占位规范

- 本地：`static/ui003/template-{job,cute,funny}.png` 三文件 **可同源占位**
- 替换设计稿后：`npm run cos:upload-static` 覆盖 CDN 三 key（manifest 已固定路径）
- 验收：制作页三模板均能看到图（非灰色猫头占位）

---

## 6. 分工口诀

| 谁 | 何时 |
| --- | --- |
| **你** | Secret 重置 → 通知 Cursor 更新 staging；设计稿就绪 → 替换三 PNG 并告知上传 COS |
| **Cursor** | 体验版 upload 前跑 §5.2 清单；真机失败先 §5.1 日志再改代码 |
| **Codex** | —（ADR-011 起不再维护用例） |

---

## 7. 链接

- 问题表：[feishu-test-issue-log.md](./feishu-test-issue-log.md)
- Runbook：[TEST-RUNBOOK-2026-06-03.md](./TEST-RUNBOOK-2026-06-03.md)
- 上传 CLI：[wechat-devtools-preview.md](../WeChatProjects/meowmoji/docs/wechat-devtools-preview.md)
