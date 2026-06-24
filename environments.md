# 测试环境（2026-06-23）

> 测前更新；用例不写技术术语，执行时对照本表。  
> **Gate3 后默认验收环境：** 正式版 + 生产 API（见下表「现行」）。

## 现行（Gate3/Gate4 · 默认）

| 用途 | 地址 / 版本 | 说明 |
| --- | --- | --- |
| 小程序 | 正式版 **v1.0.51+** | 产品真机 / 回归主环境 |
| C 端 API | `https://api.meowmoji.cn` | 正式用户数据 |
| CMS 后台 | `https://cms.meowmoji.cn` | 浏览器验收；直连 **生产 API** |
| UI CDN | `https://cdn.meowmoji.cn` | downloadFile 合法域名 |

## 联调（Gate0 / 发版前 · 非默认）

| 用途 | 地址 / 版本 | 说明 |
| --- | --- | --- |
| 小程序 | 体验版 + `stg-api` | 新功能联调、Gate0 证据 |
| Staging API | `https://stg-api.meowmoji.cn` | 与生产库隔离 |
| Staging CMS | `https://stg-cms.meowmoji.cn` | 可选 |

## 测试账号

| 角色 | 说明 |
| --- | --- |
| CMS 超管 | 运营提供，勿写入公开 Issue |
| C 端邮箱 | 由运营提供，须能收 SES 验证码 |

## Issues

| 场景 | 仓库 |
| --- | --- |
| 小程序 | [Meowmoji--mini-program/issues](https://github.com/Cynthia-cym/Meowmoji--mini-program/issues) |
| CMS | [Meowmoji-CMS01/issues](https://github.com/Cynthia-cym/Meowmoji-CMS01/issues) |
| 用例本身 | [meowmoji-test/issues](https://github.com/Cynthia-cym/meowmoji-test/issues) |
