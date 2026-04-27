# 多用户管理

## 添加新用户

为每位家人创建独立的 UUID，方便单独管理和撤销。

### 1. 生成新 UUID

```bash
xray uuid
```

### 2. 编辑服务端配置

```bash
nano /usr/local/etc/xray/config.json
```

在 `clients` 数组中添加新用户：

```json
"clients": [
  { "id": "uuid-自己用", "flow": "xtls-rprx-vision" },
  { "id": "uuid-爸爸用", "flow": "xtls-rprx-vision" },
  { "id": "uuid-妈妈用", "flow": "xtls-rprx-vision" }
]
```

### 3. 重启 Xray

```bash
systemctl restart xray
```

### 4. 生成对应的客户端配置

为每位家人复制一份 `selfvpn.yaml`，只需要修改其中的 `uuid` 字段为对应的 UUID。

## 撤销用户

从 `clients` 数组中删除对应的 UUID 条目，然后重启：

```bash
systemctl restart xray
```

该用户的配置文件将立即失效。

## 用户管理建议

| 建议 | 说明 |
|------|------|
| 一人一 UUID | 方便单独撤销，互不影响 |
| 记录对应关系 | 记下哪个 UUID 分配给了谁 |
| 不要共用 UUID | 共用时无法区分和管理 |
