# SELFVPN

Xray + VLESS + REALITY 自建代理方案，简单可靠、抗封锁能力强。

## 特点

- **VLESS + REALITY 协议**：伪装成正常 HTTPS 流量，GFW 几乎无法识别
- **无需域名和证书**：利用已有网站做 TLS 伪装，部署门槛最低
- **Clash 分流规则**：国内网站直连，国外网站自动走代理
- **全家可用**：配置文件一发即用，支持 Windows / macOS / Android / iOS

## 快速开始

详见 [SELFVPN-部署方案.md](SELFVPN-部署方案.md)，包含从购买 VPS 到全家上网的完整步骤。

## 客户端

| 平台 | 推荐客户端 |
|------|-----------|
| Windows / macOS | [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev) |
| Android | [Clash Meta for Android](https://github.com/MetaCubeX/ClashMetaForAndroid) |
| iOS | Stash / Shadowrocket (App Store 付费) |

## 文件说明

| 文件 | 说明 |
|------|------|
| `SELFVPN-部署方案.md` | 完整部署教程 |
| `config` | 服务端密钥信息（不上传，见 .gitignore） |
| `selfvpn.yaml` | Clash 客户端配置（不上传，含敏感信息） |
| `test.json` | Xray 服务端配置（不上传，含敏感信息） |

## 注意事项

- 配置文件含服务器 IP、密钥等敏感信息，已通过 `.gitignore` 排除
- 部署方案中的示例均使用占位符，可安全分享
