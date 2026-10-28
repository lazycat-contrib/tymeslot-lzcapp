# tymeslot-lzcapp

[Tymeslot](https://github.com/Tymeslot/tymeslot) 的 LazyCat LPK 打包 —— 开源会议预约平台（Elixir / Phoenix LiveView）。

## 形态

- 单容器镜像：`luka1thb/tymeslot`，端口 4000
- **内嵌 PostgreSQL**，无需外部数据库
- 镜像交付：mirror 模式（`docker.1ms.run/luka1thb/tymeslot:{tag}`），digest 校验开启
- 版本发现：Docker Hub 严格三段式 SemVer tag，排除 `-slim` 变体、`latest`、`slim`
- 仅喵喵商店；仅 PC 平台

## 部署参数与密钥

| 项 | 说明 |
|------|------|
| `SECRET_KEY_BASE` | `stable_secret` 自动生成并保持稳定（>=64 字符）；改动会导致已存凭据无法解密 |

`PHX_HOST` 先设为 `localhost`，正式接入域名后可在应用设置里改。

账号为注册制（无预置管理员），用户自行注册即可使用。

## 邮件（重要）

上游默认 `EMAIL_ADAPTER=test` —— 会**静默丢弃**所有邮件（密码重置、预约确认、提醒）。
本包**不写死**该变量，由应用内设置控制：
**上线前请在 Admin 里配置 SMTP / Postmark / SendGrid / Mailgun / AhaSend**，否则通知全部丢失且无报错。

## 持久化

- `/lzcapp/var/data` → `/app/data`（应用数据、uploads）
- `/lzcapp/var/postgres` → `/var/lib/postgresql/data`（内嵌 PG 数据）

## 结构

- `package.yml` — LPK v2 包元数据 + 平台限制
- `lzc-manifest.yml` — 单服务 + 双卷持久化 + 文件选择器注入
- `.github/lazycat-action.yml` — 镜像版本源，mirror 交付，仅私有商店
- `.github/workflows/lazycat.yml` — 每日自动跟版 + 发布喵喵商店
