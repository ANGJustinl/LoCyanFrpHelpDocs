# 使用 OpenRC 管理和守护进程

我们假设你的 Frpc 和配置文件在 `/opt/lcf/` 中，配置文件名为 `frpc.ini`

## 给与 Frpc 运行权限

```sh
chmod +x /opt/lcf/frpc
```

## 写入系统服务

在 `/etc/init.d/` 中新建一个文件，例如 `lcf-frpc`，并写入以下内容

```sh
#!/sbin/openrc-run

description="LoCyanFrp Client"
command="/opt/lcf/frpc"
command_args="-c /opt/lcf/frpc.ini"

pidfile="/run/${RC_SVCNAME}.pid"
```

## 赋予脚本执行权限

```sh
chmod +x /etc/init.d/lcf-frpc
```

## 启动并设置自动启动

```sh
service lcf-frpc start
rc-update add lcf-frpc
```