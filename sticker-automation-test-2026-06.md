# Meowmoji 表情上架自动化 · 测试归档（2026-06-14）

> **版本：** ADR-017 · CDP 主线  
> **关联用例：** TEST-SPEC-v3 §十三 CMS-C4-* · [cms-admin-test.md](./cms-admin-test.md)  
> **执行记录：** [EXECUTION.md](./EXECUTION.md)

---

## 1. 测试分工

| 角色 | 范围 |
| --- | --- |
| **Cursor** | CMS 浏览器验收 · 本机 `worker:cdp` 联调 · 单测 `admin-api npm test` |
| **产品（你）** | 微信表情平台草稿人工核对 · 「提交审核」人工操作 |
| **不参与** | CMS 后台点测由 Cursor 负责（ADR-011） |

---

## 2. 自动化专项用例（映射 TEST-SPEC-v3）

| 编号 | 名称 | 优先级 | 状态 | 结论 |
| --- | --- | --- | --- | --- |
| CMS-C4-001 | 上架任务列表加载 | P1 | ✅ | API/页面可用 |
| CMS-C4-002 | 审核通过后进入队列 | P1 | ✅ | 入队正常 |
| CMS-C4-003 | CDP 保存草稿成功 | P0 | 🟡 | 2026-06-14 修复假成功/只传1张；**待复验** |
| CMS-C4-004 | 任务失败可重试 | P2 | 🟡 | checkpoint 重置已加；待复验 |
| CMS-C4-005 | 状态与小程序同步 | P1 | ⬜ | 待端到端 |
| CMS-C4-007 | 三 Tab 列表加载 | P1 | ✅ | 2026-06-05 |
| CMS-C4-008 | Tab1 详情 Drawer | P2 | 🟡 | 待浏览器 |
| CMS-C4-009 | 插件指南浮层 | P2 | ✅ | 含完整 Runbook §2 |
| CMS-C4-010 | Tab3 刷新容错 | P2 | 🟡 | 待浏览器 |
| AUTO-CDP-001 | `automation:cdp-health` valid | P0 | ✅ | 本机通过 |
| AUTO-CDP-002 | `automation:test-cdp` 最小连通 | P0 | ✅ | 阶段 0–1 |
| AUTO-CDP-003 | 批量上传 N 张贴纸 | P0 | 🟡 | 代码已修；待复验 |
| AUTO-CDP-004 | 版权/横幅/封面/图标 | P0 | 🟡 | 待复验 |
| AUTO-CDP-005 | 保存须见「保存成功」 | P0 | 🟡 | 代码已修；待复验 |

**SKIP：** CMS-C4-006 自动提交微信（产品范围外）

---

## 3. 小程序 C 端相关（上架链路）

| 编号 | 名称 | 与自动化关系 | 状态 |
| --- | --- | --- | --- |
| MP-F3-014 | 上架状态与 CMS 一致 | 队列完成后状态同步 | ⬜ |
| MP-F3-001~016 | 一键上架 C 端 | 独立 C 端流程；自动化管平台草稿 | 见 TEST-SPEC-v3 §F3 |

C 端全套用例仍以 [TEST-SPEC-v3.md](./TEST-SPEC-v3.md)（269 条 · Gate 164）为准；本文件仅增量记录**表情平台上架自动化**专项。

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
| 架构 | CDP 主线代码与文档已收敛；云 headless 路径已证伪并禁用 |
| CMS 队列/UI | ✅ 可用 |
| 端到端保存草稿 | 🟡 **P0 待复验**（修复后需 1 套真实作品全链路） |
| Gate | 表情自动化不单独 Gate；归入 CMS-C4-003 P0 |

---

## 7. 下一步测试

1. 重启 `worker:cdp` 加载最新代码
2. 删微信后台不完整草稿
3. 执行 AUTO-CDP-003~005 + CMS-C4-003
4. 回填 [EXECUTION.md](./EXECUTION.md)
