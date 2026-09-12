# mihomo-config

GitHub 托管的 mihomo 配置与规则集（自动更新）。

## 文件说明

| 文件 | 内容 | 更新方式 |
|---|---|---|
| `jm-list.txt` | 禁漫 JM 域名规则集（classical format） | 换域名时编辑本文件，设备每 24h 自动拉取 |
| `config.yaml` | ⚠️ 含机场订阅，**保持私有，勿 push 到本公开仓库** | 本地自行维护 |

## 使用

规则集 URL（mihomo rule-provider）：

```
https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/jm-list.txt
```

或 jsDelivr CDN（国内可直连，缓存约 24h）：

```
https://cdn.jsdelivr.net/gh/Anyi-lab/mihomo-config@main/jm-list.txt
```

## 更新 JM 域名

编辑 `jm-list.txt`，每行一条，如：

```
DOMAIN-SUFFIX,新域名.com
```

push 后所有设备自动生效。