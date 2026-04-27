# 验证连接

## 基本验证

开启 Clash Verge 系统代理后，逐一测试：

| 测试 | 访问地址 | 预期结果 |
|------|---------|---------|
| 代理是否生效 | [google.com](https://www.google.com) | 能正常打开 |
| IP 是否为服务器 | [whatismyipaddress.com](https://whatismyipaddress.com) | 显示为 VPS 的 IP |
| 直连是否正常 | [baidu.com](https://www.baidu.com) | 正常打开且速度不受影响 |
| 视频是否流畅 | [youtube.com](https://www.youtube.com) | 能播放视频 |

## 服务端验证

在服务器上执行：

```bash
# Xray 运行状态
systemctl status xray

# 端口是否在监听
ss -tlnp | grep 443

# 实时日志（连接时观察）
journalctl -u xray -f
```

## 分流验证

确认分流规则正确工作：

1. 访问 [ip.sb](https://ip.sb) — 应显示 VPS 的 IP（走代理）
2. 访问 [myip.ipip.net](https://myip.ipip.net) — 应显示你的本地 IP（直连）

{% hint style="tip" %}
在 Clash Verge 的 **日志** 页面可以实时看到每个请求匹配了哪条规则，方便排查分流问题。
{% endhint %}

## 常见连接问题

| 现象 | 可能原因 | 解决方法 |
|------|---------|---------|
| 完全无法连接 | IP 被封 / 端口未放行 | 检查 `telnet IP 443`，或换 IP |
| 连接超时 | 防火墙未放行 | 检查 ufw + 云控制台安全组 |
| 能连但很慢 | 服务器地区较远 | 考虑换到日本/新加坡节点 |
| 国内网站变慢 | 分流规则问题 | 检查 Clash 日志确认走的是直连 |

## 全部通过？

恭喜！你的代理已经配置完成。接下来可以查看：

- [给家人添加账号](multi-user.md)
- [安全加固服务器](security.md)
