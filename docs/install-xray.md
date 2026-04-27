# 安装与配置 Xray

## 安装 Xray

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

安装完成后：
- 程序位置：`/usr/local/bin/xray`
- 配置文件：`/usr/local/etc/xray/config.json`

## 生成密钥

### 生成 X25519 密钥对

```bash
xray x25519
```

输出示例：
```
Private key: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Public key:  YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
```

### 生成 UUID

```bash
xray uuid
```

输出示例：`a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### 生成 shortId

```bash
openssl rand -hex 8
```

输出示例：`6ba85179e30d4fc2`

{% hint style="danger" %}
**立即保存以下信息到密码管理器：**
- Private Key（服务端用）
- Public Key（客户端用）
- UUID（服务端 + 客户端都要用）
- shortId（服务端 + 客户端都要用）
{% endhint %}

## 配置服务端

```bash
cat > /usr/local/etc/xray/config.json << 'EOF'
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "你的UUID",
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "dest": "www.microsoft.com:443",
          "xver": 0,
          "serverNames": [
            "www.microsoft.com"
          ],
          "privateKey": "你的Private_Key",
          "shortIds": [
            "",
            "你的shortId"
          ]
        }
      },
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls", "quic"]
      }
    }
  ],
  "outbounds": [
    { "protocol": "freedom", "tag": "direct" },
    { "protocol": "blackhole", "tag": "block" }
  ]
}
EOF
```

**替换三处占位符：**

| 占位符 | 替换为 |
|--------|--------|
| `你的UUID` | 上一步生成的 UUID |
| `你的Private_Key` | 上一步生成的 Private Key |
| `你的shortId` | 上一步生成的 shortId |

## 伪装目标选择

`dest` 和 `serverNames` 是伪装目标 — GFW 看到的是你在访问该网站。

**推荐伪装目标：**

| 域名 | 说明 |
|------|------|
| `www.microsoft.com` | 默认推荐 |
| `www.apple.com` | 备选 |
| `gateway.icloud.com` | 备选 |
| `www.amazon.com` | 备选 |

**选择要求：**
- 支持 TLSv1.3 + H2
- 非 CDN 域名（避免与你的 IP 地理位置不匹配）

**验证方法：**
```bash
curl -I --http2 -sS https://目标网站 | head -5
# 看到 HTTP/2 200 即可
```

{% hint style="tip" %}
避免使用过于热门的伪装目标（如 `dl.google.com`），因为大量代理用户都在用同一个目标可能引起注意。
{% endhint %}

## 启动服务

```bash
# 启动
systemctl restart xray

# 开机自启
systemctl enable xray

# 检查状态
systemctl status xray
```

### 验证配置正确

```bash
# 检查 JSON 配置语法
xray run -test -c /usr/local/etc/xray/config.json
```

如果有问题，查看日志：
```bash
journalctl -u xray -f
```

## 配置防火墙

{% hint style="danger" %}
**务必先放行 SSH 端口！** 否则 `ufw enable` 后你将被锁在服务器外面。
{% endhint %}

```bash
# 先放行 SSH
ufw allow 22/tcp

# 放行 Xray 端口
ufw allow 443/tcp

# 启用防火墙
ufw enable

# 确认规则
ufw status
```

{% hint style="warning" %}
如果使用 Oracle Cloud、AWS 等云厂商，除了 ufw 之外还需要在云控制台的 **安全组 / Security List** 中放行端口。
{% endhint %}

## 下一步

服务端配置完成，接下来配置[客户端](client-config.md)。
