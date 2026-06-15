# CMS 运营后台测试用例（Cursor 维护）

> **维护：** Cursor（规格真源 · ADR-011）  
> **执行：** Cursor（浏览器）· **产品不参与 CMS 点测**  
> **原则：** [test.md](./test.md)  
> **地址：** [https://cms.meowmoji.cn](https://cms.meowmoji.cn)  
> **更新：** 2026-06-03（v2.1 演进）  
> **v2 完整规格：** [TEST-SPEC-v2.md](./TEST-SPEC-v2.md) · [飞书验收清单](https://qcnp7uavxd21.feishu.cn/wiki/DqxmwME1giejvgkauWycNXqanRc)

**通过标准：** 每步实际结果与「预期」一致；失败按 [issue-template.md](./issue-template.md) 提交到 `Meowmoji-CMS01` Issues。

---

## 当前执行口径（2026-06-04 · v2）

- **Gate 必跑：** [TEST-SPEC-v2.md](./TEST-SPEC-v2.md) 中全部 **P0 + P1**（模块前缀 `CMS-AUTH-001` 等，CMS 侧 **28 条**；全仓 Gate **93 条**）。
- **P2/P3：** backlog；**预留坑位**（`CMS-RESV-001` Puppeteer 自动提交）测试时间待定。
- 历史 `cms-test.md` 保留追溯；**CMS-400~409 已由 v2 拆分覆盖**（见下文映射）。
- 本文步骤只描述浏览器页面操作，不写接口或数据库术语。

### v2 模块索引（CMS）

| 模块 | 前缀 | Gate（P0+P1） |
| --- | --- | --- |
| 登录与账号 | CMS-AUTH | 4 |
| 首页与导航 | CMS-DASH | 3 |
| 申请管理 | CMS-APP | 5 |
| 上架任务 | CMS-PUB / CMS-C4 | 3 + 007~010（三 Tab）· [CDP 专项](./sticker-automation-test-2026-06.md) |
| 用户管理 | CMS-USR | 3 |
| 反馈管理 | CMS-FBK | 4 |
| 公告管理 | CMS-ANN | 3 |
| 账号权限 | CMS-ADM | 1 |
| 系统设置 | CMS-SYS | 0 |
| 联调回看 | CMS-E2E | 2 |
| **预留** | CMS-RESV | 0（待定） |

### 旧编号 → v2 映射

| 旧 | v2 |
| --- | --- |
| CMS-400 | CMS-AUTH-001/003/006 |
| CMS-401 | CMS-DASH-001~003 |
| CMS-402~404 | CMS-APP-001~004 |
| CMS-405 | CMS-PUB-001/002 |
| CMS-406 | CMS-USR-001~003 |
| CMS-407 | CMS-FBK-001~003 |
| CMS-408 | CMS-ANN-001~003 |
| CMS-409 | CMS-E2E-001/002 |

---

## 用例详述

> **完整 6 字段规格（编号 / 名称 / 前置 / 步骤 / 预期 / 优先级）见 [TEST-SPEC-v2.md §7](./TEST-SPEC-v2.md#7-cms-用例详述)。**

---

## 历史收口用例（CMS-400~409 · 已 superseded）

以下条目保留供 Runbook 追溯；执行请以 v2 为准。

<details>
<summary>展开 CMS-400~409 原文</summary>

### CMS-400 后台登录

**前置条件：** 已拿到可用管理员账号与密码。  
**步骤：** 1. 浏览器打开 CMS 2. 输入账号密码 3. 登录  
**预期：** 进入后台首页；无白屏。  
**优先级：** P0

### CMS-401 首页与基础导航

**前置条件：** 已登录。  
**步骤：** 1. 查看首页 2. 遍历主导航 3. 返回  
**预期：** 导航可用；有数据或空态。  
**优先级：** P1

### CMS-402 申请管理列表与筛选

**前置条件：** 有申请记录。  
**步骤：** 1. 进入申请管理 2. 筛选 3. 确认 4. 重置  
**预期：** 筛选生效；重置恢复默认。  
**优先级：** P0

### CMS-403 审核通过 · ### CMS-404 驳回 · ### CMS-405 上架任务 · ### CMS-406 用户管理 · ### CMS-407 反馈 · ### CMS-408 公告 · ### CMS-409 联调回看

（原文见 git 历史 2026-06-02 版；v2 已拆分逆向/边界/异常场景。）

</details>
