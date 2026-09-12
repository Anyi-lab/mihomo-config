# mihomo-config

GitHub 托管的 mihomo 规则集 + 客户端配置模板（自动更新）。

## 文件说明

| 文件 | 条数 | 对应目标组 | 说明 |
|---|---|---|---|
| `jm-list.txt` | 12 | 自定 | 禁漫 JM 域名规则集 |
| `manual-list.txt` | 17 | `手动切换` | 常用站点，走手动选节点 |
| `direct-list.txt` | 3 | `DIRECT` | 直连 |
| `emby-list.txt` | 1 | `Emby` | 自有 Emby 服务 |
| `mihomo/Client/config.yaml` | — | — | 完整客户端配置模板（源自 [Repcz/Tool@X](https://github.com/Repcz/Tool/blob/X/mihomo/Client/config.yaml)，已并入下面 4 个规则集） |

`*-list.txt` 全部为 `classical` / `format: text` 规则集：**每行只写规则本身，不带策略**，走哪个组由 config.yaml 的 `RULE-SET,<名字>,<组>` 决定。

> ⚠️ 含真实机场订阅的 `config.yaml` 一律**不进本公开仓库**（`.gitignore` 已忽略；仅放行 `mihomo/Client/config.yaml` 这个占位模板）。

## 使用

### 方式 A：只引规则集

#### 1. 声明 rule-provider

```yaml
rule-providers:
  MyManual: {type: http, behavior: classical, format: text, interval: 86400, url: "https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/manual-list.txt"}
  MyDirect: {type: http, behavior: classical, format: text, interval: 86400, url: "https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/direct-list.txt"}
  MyEmby:   {type: http, behavior: classical, format: text, interval: 86400, url: "https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/emby-list.txt"}
  JM:       {type: http, behavior: classical, format: text, interval: 86400, url: "https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/jm-list.txt"}
```

jsDelivr CDN 备用（国内可直连，缓存约 24h）：

```
https://cdn.jsdelivr.net/gh/Anyi-lab/mihomo-config@main/manual-list.txt
```

#### 2. 引用规则集（rules，顺序靠前优先）

```yaml
rules:
  - RULE-SET,MyEmby,Emby
  - RULE-SET,MyDirect,DIRECT
  - RULE-SET,JM,手动切换
  - RULE-SET,MyManual,手动切换
  # 以下两条不适合放进规则集，保留在 rules 里
  - IP-CIDR,154.17.232.92/32,DIRECT,no-resolve
  - PROCESS-NAME,KikoFlu.exe,手动切换
```

> 规则集要放在 `RULE-SET,Proxy,国外网站` 之类的大表**之前**，否则这些域名会先被上游大表抓走。

### 方式 B：直接用完整配置模板

`mihomo/Client/config.yaml` 是完整可用的 mihomo 客户端配置（含 proxy-groups / dns / tun / 规则集）：

```
https://raw.githubusercontent.com/Anyi-lab/mihomo-config/main/mihomo/Client/config.yaml
```

下载后把 `proxy-providers.Subscribe.url` 的 `http://your-service-provider` 换成自己的订阅地址即可使用（**换好之后不要再 push 回本仓库**）。

> 注意：Clash Party 每次启动会重生 `work/config.yaml`，改动请落在 profile / `override/*.yaml`，别直接改 `work/config.yaml`。

## 增删规则

编辑对应 txt，一行一条，如：

```
DOMAIN-SUFFIX,新域名.com
```

push 后各设备 24h 内自动生效（可在面板手动刷新 provider）。
