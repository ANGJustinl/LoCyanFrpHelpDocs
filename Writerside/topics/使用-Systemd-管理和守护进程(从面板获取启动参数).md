# 使用 Systemd 管理和守护进程 (从面板获取启动参数)

此方案适用于从 Web 面板获取 Token 和隧道 ID 的用户。

### 1. 假设路径
我们假设你的 `frpc` 可执行文件位于 `/opt/lcf/` 目录中，并在面板中配置了隧道。

### 2. 授予执行权限
```sh
chmod +x /opt/lcf/frpc
````

### 3. 创建 Systemd 服务

将以下内容保存为 `.service` 文件（例如 `lcf-frpc@.service`），存放于以下任一目录：

  * `/etc/systemd/system/`
  * `/usr/lib/systemd/system/`

<!-- end list -->

```ini
[Unit]
Description=LoCyanFrp Client
After=network.target

[Service]
Type=idle
Restart=on-failure
RestartSec=60s
WorkingDirectory=/opt/lcf
ExecStart=/bin/sh -c '/opt/lcf/frpc -u $(echo "%i" | cut -d ":" -f 1) -p $(echo "%i" | cut -d ":" -f 2)'

[Install]
WantedBy=multi-user.target
```

> **注意**：后续操作中，Systemd Unit 名称的格式为 `lcf-frpc@<Token>:<隧道ID>.service`。

### 4\. 启动并设置开机自启

例如，若 Token 为 `1919810`，隧道号为 `114514`，则执行：

```sh
# 启动并立即启用开机自启
systemctl enable lcf-frpc@1919810:114514.service --now

# 查看运行状态
systemctl status lcf-frpc@1919810:114514.service

# 查看详细日志
# journalctl -aeu lcf-frpc@1919810:114514.service
```
