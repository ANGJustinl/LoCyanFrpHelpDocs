# 使用 Systemd 管理和守护进程

# 若从面板获取配置文件
我们假设你的 Frpc 和配置文件在 `/opt/lcf/` 中，并在面板中配置了隧道

## 给与 Frpc 运行权限

```sh
chmod +x /opt/lcf/frpc
```

## 写入系统服务

将以下内容写入以下目录 `.service` 后缀文件中，比如 `lcf-frpc@.service`

- /etc/systemd/system/
- /usr/lib/systemd/system/

```ini
[Unit]
Description=LoCyanFrp Client
After=network.target


[Service]
Type=idle
#DynamicUser=yes
Restart=on-failure
RestartSec=60s
ExecStart=/bin/sh -c '/opt/lcf/frpc -u $(echo "%i" | cut -d ":" -f 1) -p $(echo "%i" | cut -d ":" -f 2)'
WorkingDirectory=/opt/lcf

[Install]
WantedBy=multi-user.target
```

请记住后续操作中用到的 Unit 名称 是 lcf-frpc@<启动参数>，例如 lcf-frpc@你的token:隧道号

## 启动并设置自动启动
例如你的token是1919810 想要启动的隧道号是114514则:

```sh
systemctl enable lcf-frpc@1919810:114514.service --now

# 查看运行状态
systemctl status lcf-frpc@1919810:114514.service
# 查看更详细的日志
#jouralctl -aeu lcf-frpc@1919810:114514.service
```

# 若使用自定义的frpc.ini配置文件

我们假设你的 Frpc 和配置文件在 `/opt/lcf/` 中，配置文件名为 `frpc.ini`

## 给与 Frpc 运行权限

```sh
chmod +x /opt/lcf/frpc
```

## 写入系统服务

将以下内容写入以下目录任意 `.service` 后缀文件中，比如 `lcf-frpc.service`

- /etc/systemd/system/
- /usr/lib/systemd/system/

```ini
[Unit]
Description=LoCyanFrp Client

[Service]
WorkingDirectory=/opt/lcf/
ExecStart=/opt/lcf/frpc
# 如果你的配置文件名不为 frpc.ini，请手动使用 -c 指定：
#ExecStart=/opt/lcf/frpc -c config.ini

[Install]
WantedBy=network.target
```

## 启动并设置自动启动

```sh
systemctl enable lcf-frpc.service --now

# 查看运行状态
systemctl status lcf-frpc.service
# 查看更详细的日志
#jouralctl -aeu lcf-frpc.service
```

更多管理命令，请参阅: [Linux 中国: systemctl 命令完全指南](https://linux.cn/article-5926-1.html)
