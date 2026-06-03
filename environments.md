# 测试环境（2026-06-03）

> 测前由负责人更新 IP/账号；用例里不写技术术语，执行时对照本表。

## 地址

| 用途 | 地址 | 说明 |
| --- | --- | --- |
| CMS 运营后台（浏览器） | `https://cms.meowmoji.cn` 或台式机本地 `http://<台式机局域网IP>:5173` | 优先使用正式域名；本地联调时再切局域网 IP |
| CMS 接口（仅联调参考） | `http://1.14.147.241:18180` | staging，云函数已指向此处 |
| 小程序 | 微信开发者工具 · 体验版/开发版 | 工作区真源目录见 live 文档；真机验收以预览二维码为准 |

## 测试账号（示例，请替换为真实）

| 角色 | 账号 | 密码/说明 |
| --- | --- | --- |
| CMS 超管 | `admin` | 由运营提供，勿写入公开 Issue |
| C 端测试邮箱 | `accept-v2@meowmoji.test` | 邮箱验证码登录；如有专用白名单账号，以当前执行人手头账号为准 |

## 问题提交到哪里

| 发现场景 | GitHub Issues |
| --- | --- |
| 小程序页面/流程 | [Meowmoji--mini-program/issues](https://github.com/Cynthia-cym/Meowmoji--mini-program/issues) |
| CMS 后台页面/流程 | [Meowmoji-CMS01/issues](https://github.com/Cynthia-cym/Meowmoji-CMS01/issues) |
| 用例本身错误/遗漏 | [meowmoji-test/issues](https://github.com/Cynthia-cym/meowmoji-test/issues) |

## 跨设备访问 CMS（MacBook → 台式机）

1. 台式机终端查看局域网 IP（Mac：`ipconfig getifaddr en0` 或系统设置-网络）。  
2. 台式机启动：`npm --prefix admin-web run dev -- --host`（记下端口，通常 5173）。  
3. MacBook 浏览器打开 `http://<IP>:5173`，能登录即通过。
