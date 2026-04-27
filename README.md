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

## 文档

完整部署文档已按章节整理在 `docs/` 目录下，可通过 GitBook 在线阅读：

| 章节 | 内容 |
|------|------|
| [方案概述](docs/overview.md) | 技术选型、架构、对比 |
| [购买 VPS](docs/buy-vps.md) | 厂商推荐、购买流程 |
| [服务器初始化](docs/server-init.md) | 系统更新、BBR 加速 |
| [安装配置 Xray](docs/install-xray.md) | 密钥生成、服务端配置 |
| [客户端配置](docs/client-config.md) | Clash 配置文件生成 |
| [验证连接](docs/verify.md) | 测试代理是否正常 |
| [多用户管理](docs/multi-user.md) | 给家人添加账号 |
| [安全加固](docs/security.md) | SSH Key、端口修改 |
| [故障排查](docs/troubleshooting.md) | 常见问题诊断 |
| [维护清单](docs/maintenance.md) | 定期更新、备份 |

### 其他文件

| 文件 | 说明 |
|------|------|
| `SELFVPN-部署方案.md` | 原始完整部署文档（单文件版） |
| `config` | 服务端密钥信息（不上传，见 .gitignore） |
| `selfvpn.yaml` | Clash 客户端配置（不上传，含敏感信息） |
| `test.json` | Xray 服务端配置（不上传，含敏感信息） |

## 注意事项

- 配置文件含服务器 IP、密钥等敏感信息，已通过 `.gitignore` 排除
- 部署方案中的示例均使用占位符，可安全分享
