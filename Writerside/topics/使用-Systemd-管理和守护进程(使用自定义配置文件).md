# 使用 Systemd 管理和守护进程 (使用自定义 `frpc.ini` 配置文件)

此方案适用于使用本地 `frpc.ini` 完整配置文件的用户。

### 1\. 假设路径

假设 `frpc` 可执行文件和配置文件（例如 `frpc.ini`）均位于 `/opt/lcf/` 目录中。

### 2\. 授予执行权限

```sh
chmod +x /opt/lcf/frpc
```

### 3\. 创建 Systemd 服务

将以下内容保存为 `.service` 文件（例如 `lcf-frpc.service`），存放于以下任一目录：

  * `/etc/systemd/system/`
  * `/usr/lib/systemd/system/`

<!-- end list -->

```ini
[Unit]
Description=LoCyanFrp Client
After=network.target

[Service]
Type=simple
Restart=on-failure
RestartSec=5s
WorkingDirectory=/opt/lcf/
ExecStart=/opt/lcf/frpc -c ./frpc.ini

# 如果你的配置文件名不是 frpc.ini，请修改上面一行的 -c 参数，例如：
# ExecStart=/opt/lcf/frpc -c ./config.ini

[Install]
WantedBy=multi-user.target
```

### 4\. 启动并设置开机自启

```sh
# 启动并立即启用开机自启
systemctl enable lcf-frpc.service --now

# 查看运行状态
systemctl status lcf-frpc.service

# 查看详细日志
# journalctl -aeu lcf-frpc.service
```

-----

更多管理命令，请参阅: [Linux 中国: systemctl 命令完全指南](https://linux.cn/article-5926-1.html)
