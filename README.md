# contrack-lzcapp

[Contrack](https://github.com/arvarik/contrack) 的 LazyCat LPK 打包 —— 自托管联系人管理（单容器）。

## 形态

- 单容器镜像：`ghcr.io/arvarik/contrack`，端口 3210，数据在 `/app/data`
- 镜像交付：mirror 模式（`ghcr.1ms.run/arvarik/contrack:{tag}`），digest 校验开启
- 版本发现：GHCR 严格 SemVer tag（`^[0-9]+\.[0-9]+\.[0-9]+$`），排除 sha 短标签与 `X.Y` 别名
- 认证：开启 `AUTH_REQUIRED=true`（首次访问创建账号），`TRUST_PROXY_HOPS=1` 让反向代理下拿到真实客户端地址
- 密钥：`CONTRACK_SECRET_KEY` 用 `stable_secret` 自动生成并保持稳定
- 数据持久化：`/lzcapp/var/data` → `/app/data`（含数据库与 `secret.key`，需一起备份）

## 说明

上游 compose 默认把端口发布到 `127.0.0.1` 且关闭认证；这里在微服内部署，因此显式开启认证后对应用网开放。

## 结构

- `package.yml` — LPK v2 包元数据（`community.lazycat.app.contrack`）
- `lzc-manifest.yml` — 单服务 + upstreams + 环境变量
- `.github/lazycat-action.yml` — 镜像版本源，mirror 交付，仅私有商店
- `.github/workflows/lazycat.yml` — 每日自动跟版 + 发布喵喵商店
