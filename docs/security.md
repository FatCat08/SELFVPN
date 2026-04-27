# 安全加固

{% hint style="warning" %}
以下步骤建议在首次部署后**尽快完成**，降低被攻击风险。
{% endhint %}

## 1. 修改 SSH 端口

默认 22 端口是暴力破解的首要目标。

```bash
# 选一个不常用的端口（如 58222）
sed -i 's/#Port 22/Port 58222/' /etc/ssh/sshd_config

# 防火墙放行新端口
ufw allow 58222/tcp

# 重启 SSH
systemctl restart sshd
```

{% hint style="danger" %}
**先放行新端口再重启 sshd！** 否则你会被锁在外面。修改后用新端口重新连接测试：`ssh -p 58222 root@你的IP`
{% endhint %}

验证新端口可连接后，关闭旧端口：
```bash
ufw delete allow 22/tcp
```

## 2. 禁用密码登录，改用 SSH Key

### 在本地电脑生成密钥

```bash
# 如果还没有 SSH Key
ssh-keygen -t ed25519 -C "selfvpn"
```

### 复制公钥到服务器

```bash
ssh-copy-id -p 58222 root@你的服务器IP
```

### 测试 Key 登录

```bash
ssh -p 58222 root@你的服务器IP
# 应该无需输入密码即可登录
```

### 禁用密码登录

确认 Key 登录成功后：

```bash
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
systemctl restart sshd
```

## 3. 设置自动安全更新

```bash
apt install -y unattended-upgrades
dpkg-reconfigure -plow unattended-upgrades
```

## 4. 安装 Fail2Ban（可选）

自动封禁暴力破解 IP：

```bash
apt install -y fail2ban
systemctl enable fail2ban
systemctl start fail2ban
```

## 安全检查清单

- [ ] SSH 端口已修改（非 22）
- [ ] 密码登录已禁用
- [ ] SSH Key 登录已验证
- [ ] 防火墙已启用，仅开放必要端口
- [ ] 自动更新已配置
- [ ] 密钥信息已保存到密码管理器
