# 客户端配置

## 支持的客户端

| 平台 | 客户端 | 获取方式 |
|------|--------|---------|
| Windows / macOS | [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases) | GitHub 免费下载 |
| Android | [Clash Meta for Android](https://github.com/MetaCubeX/ClashMetaForAndroid/releases) | GitHub 下载 APK |
| iOS | Stash / Shadowrocket | App Store 付费 ($3-5) |

## 生成 Clash 配置文件

在本地创建 `selfvpn.yaml`：

```yaml
mixed-port: 7890
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - https://dns.alidns.com/dns-query
    - https://doh.pub/dns-query
  fallback:
    - https://1.1.1.1/dns-query
    - https://8.8.8.8/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN

proxies:
  - name: "SELFVPN"
    type: vless
    server: 你的服务器IP
    port: 443
    uuid: 你的UUID
    network: tcp
    tls: true
    udp: true
    flow: xtls-rprx-vision
    servername: www.microsoft.com
    reality-opts:
      public-key: 你的Public_Key
      short-id: "你的shortId"
    client-fingerprint: chrome

proxy-groups:
  - name: "代理"
    type: select
    proxies:
      - SELFVPN
      - DIRECT

  - name: "国外网站"
    type: select
    proxies:
      - 代理
      - DIRECT

  - name: "国内网站"
    type: select
    proxies:
      - DIRECT
      - 代理

rules:
  # 国内域名直连
  - GEOSITE,cn,国内网站
  - GEOIP,CN,国内网站

  # 常用国外网站走代理
  - GEOSITE,google,国外网站
  - GEOSITE,youtube,国外网站
  - GEOSITE,twitter,国外网站
  - GEOSITE,facebook,国外网站
  - GEOSITE,telegram,国外网站
  - GEOSITE,github,国外网站
  - GEOSITE,openai,国外网站

  # 其他默认走代理
  - MATCH,代理
```

**替换占位符：**

| 占位符 | 替换为 |
|--------|--------|
| `你的服务器IP` | VPS 的公网 IP |
| `你的UUID` | 服务端配置中的 UUID |
| `你的Public_Key` | 生成的 Public Key（注意是 **Public** 不是 Private） |
| `你的shortId` | 生成的 shortId |

## 导入配置

### 自己的电脑

1. 下载安装 [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev/releases)
2. 打开 Clash Verge → 左侧菜单 → **配置**（Profiles）
3. 点击 **新建** → **本地文件** → 选择 `selfvpn.yaml`
4. 点击启用该配置
5. 主界面**开启系统代理**

### 给家人配置

1. 把 `selfvpn.yaml` 发送给家人
2. 让他们安装对应平台的客户端
3. 导入配置文件，开启即可

{% hint style="warning" %}
**传输配置文件注意事项：**
- 微信可能拦截 `.yaml` 文件 — 建议压缩成 `.zip` 后发送
- 也可用邮件附件、AirDrop、网盘链接传输
- 配置文件包含敏感信息，不要发到公开群聊
{% endhint %}

## 分流规则说明

配置中的规则逻辑：

```
国内域名/IP → 直连（不走代理，不影响国内网速）
Google/YouTube/Twitter 等 → 走代理
其他所有流量 → 走代理
```

如需添加更多规则，在 `rules` 部分追加即可。

## 下一步

配置完成，接下来[验证连接](verify.md)是否正常。
