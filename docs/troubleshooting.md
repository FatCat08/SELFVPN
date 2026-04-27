# 故障排查

## 快速诊断流程

```
能 SSH 连接服务器吗？
├── 不能 → VPS 宕机 或 IP 被封
│         → 去 VPS 控制台检查，或直接销毁重建
└── 能 → Xray 在运行吗？
         ├── 不在 → systemctl restart xray
         └── 在 → 端口通吗？（本地 telnet IP 443）
                  ├── 不通 → 检查防火墙 + 云安全组
                  └── 通 → 客户端配置问题，检查 UUID/Key/端口
```

## IP 被封

**症状**：服务器 ping 不通，SSH 连不上，但 VPS 控制台显示服务器正在运行。

**解决方案**：

1. 在 VPS 控制台销毁服务器
2. 重新创建新服务器（自动获得新 IP）
3. 重新执行部署步骤
4. 更新所有客户端配置文件中的 IP

{% hint style="tip" %}
**Vultr 按小时计费**，销毁重建几乎没有额外成本。
{% endhint %}

**给家人更新 IP 的简化流程**：
1. 修改 `selfvpn.yaml` 中的 `server` 字段
2. 重新发送给家人
3. 家人在 Clash 中重新导入即可

## Xray 服务异常

```bash
# 查看服务状态
systemctl status xray

# 查看最近日志
journalctl -u xray --no-pager -n 50

# 检查配置文件语法
xray run -test -c /usr/local/etc/xray/config.json

# 重启服务
systemctl restart xray
```

### 常见错误

| 错误信息 | 原因 | 解决 |
|---------|------|------|
| `invalid JSON` | 配置文件格式错误 | 检查 JSON 语法（注意逗号） |
| `address already in use` | 443 端口被占用 | `ss -tlnp \| grep 443` 查看占用进程 |
| `private key parse error` | Private Key 不正确 | 重新生成密钥对 |

## 连接不稳定

**排查方向**：

1. **服务器负载** — `top` 查看 CPU/内存是否满载
2. **网络质量** — `ping` 服务器看丢包率
3. **伪装目标问题** — 换一个 dest 试试
4. **时间同步** — 确保服务器时间准确：`timedatectl`

## 特定场景

### 能翻墙但部分网站打不开

可能是 DNS 污染问题，确认 Clash 配置中的 DNS 设置正确，`enhanced-mode` 为 `fake-ip`。

### 速度慢

1. 确认 BBR 已启用：`sysctl net.ipv4.tcp_congestion_control`
2. 测试服务器带宽：`apt install speedtest-cli && speedtest`
3. 考虑换更近的节点（日本 → 新加坡 或反之）

### 手机能连但电脑不行（或反之）

客户端配置问题，对比两端的 `uuid`、`public-key`、`short-id` 是否一致。
