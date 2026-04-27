# 服务器初始化

## 连接服务器

```bash
# Windows Terminal / PowerShell
ssh root@你的服务器IP

# 输入密码（输入时不显示，正常现象）
```

{% hint style="info" %}
如果提示 `Are you sure you want to continue connecting?`，输入 `yes` 回车即可。
{% endhint %}

## 更新系统

```bash
apt update && apt upgrade -y
```

## 安装必要工具

```bash
apt install -y curl wget unzip
```

## 开启 BBR 加速

BBR 是 Google 开发的 TCP 拥塞控制算法，能显著提升网络传输性能。

```bash
# 幂等写入（重复执行不会重复添加）
grep -q "net.core.default_qdisc=fq" /etc/sysctl.conf || echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
grep -q "net.ipv4.tcp_congestion_control=bbr" /etc/sysctl.conf || echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

### 验证 BBR 已启用

```bash
sysctl net.ipv4.tcp_congestion_control
# 应输出: net.ipv4.tcp_congestion_control = bbr
```

## 下一步

系统初始化完成，接下来[安装与配置 Xray](install-xray.md)。
