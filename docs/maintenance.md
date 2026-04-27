# 维护清单

## 定期维护

| 周期 | 任务 | 命令 |
|------|------|------|
| 每月 | 检查服务器状态 | `systemctl status xray` |
| 每季度 | 更新 Xray | 见下方更新命令 |
| 每月 | 检查流量使用 | VPS 控制台查看 |
| 随时 | 备份配置 | 见下方备份命令 |

## 更新 Xray

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
systemctl restart xray
```

## 备份配置

```bash
cp /usr/local/etc/xray/config.json ~/xray-config-backup-$(date +%Y%m%d).json
```

{% hint style="info" %}
密钥信息（UUID、Private Key、Public Key、shortId）务必保存到密码管理器，服务器被销毁后无法恢复。
{% endhint %}

## 监控服务器资源

```bash
# 查看内存使用
free -h

# 查看磁盘使用
df -h

# 查看 CPU 和进程
top

# 查看 Xray 进程资源占用
ps aux | grep xray
```

## 费用控制

| 项目 | 费用 | 注意事项 |
|------|------|---------|
| VPS | $6/月 | 注意月流量限制 |
| 超出流量 | 因厂商而异 | Vultr 默认 2TB/月够用 |

{% hint style="tip" %}
如果流量需求不大，可以考虑 Racknerd 等年付厂商（~$12/年），长期更划算。
{% endhint %}
