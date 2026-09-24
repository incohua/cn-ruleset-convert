# cn-ruleset-convert

GitHub Actions 每日构建的中国直连分流规则集（多格式）。

## 数据源

[Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules) `direct.txt` ——四源聚合：

- v2fly/domain-list-community
- dnsmasq-china-list（长尾国内域名主力，10 万+）
- apple-cn / google-cn

## 产物（`release` 分支根目录）

| 文件 | 格式 | 用途 |
|---|---|---|
| `ls-direct.srs` | sing-box rule-set (binary v3) | sing-box `route.rule_set` / `dns.rules` |
| `ls-direct.domains` | Surge DOMAIN-SET 明文 | Surge `DOMAIN-SET` |

约 11.1 万域名，每日 05:30 CST 自动更新。

## 使用

### sing-box（TUN 网关）

```json
{
  "type": "remote",
  "tag": "ls-direct",
  "format": "binary",
  "url": "https://raw.githubusercontent.com/incohua/cn-ruleset-convert/release/ls-direct.srs",
  "download_detour": "direct",
  "update_interval": "24h"
}
```

### Surge

```ini
DOMAIN-SET,https://raw.githubusercontent.com/incohua/cn-ruleset-convert/release/ls-direct.domains,DIRECT
```

## 构建安全

- 上游域名数 < 1000 时构建直接失败，不会发布空规则集
- `release` 分支仅含产物文件，`git push -f` 原子覆盖
