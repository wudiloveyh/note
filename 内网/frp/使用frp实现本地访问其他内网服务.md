# 利用frp建立隧道

## 环境

```diff
- 公网-kali-frps
192.168.10.129
192.168.49.134
- 内网-centos-frpc
192.168.49.233
```

## 软件位置

```diff
+ 服务器
/etc/frp/frps.ini
/usr/local/bin/frps
/etc/systemd/system/frps.service
+ 客户端
/etc/frp/frpc.ini
/usr/local/bin/frpc
/etc/systemd/system/frpc.service
```

## 内网服务器配置

/etc/frp/frpc.ini

```diff
[common]
server_addr = 192.168.49.134
server_port = 7000
token = chuxue_token

[socks_proxy]
type = tcp
remote_port = 1080
plugin = socks5
plugin_user = chuxue
plugin_passwd = chuxue
```

/etc/systemd/system/frpc.service

```diff
[Unit]
Description=frpc service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/frpc -c /etc/frp/frpc.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## 公网服务器配置

/etc/frp/frps.ini

```diff
[common]
bind_port = 7000
token = chuxue_token
```

/etc/systemd/system/frps.service

```diff
[Unit]
Description=frps service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/frps -c /etc/frp/frps.ini
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## 运行服务

服务器

```bash
sudo systemctl daemon-reload 
sudo systemctl enable --now frps
```

客户端

```bash
sudo systemctl daemon-reload 
sudo systemctl enable --now frpc
```

## 防火墙

```diff
sudo ufw allow 7000/tcp
sudo ufw allow 1080/tcp
sudo ufw enable 
sudo ufw status
```

## 利用proxifier验证

创建socks5代理

打开proxifier→profile→Proxy Server

点击add

![image-20251114171304617](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251114184736152.png)

点击check确认这个服务器有没有用

![image-20251114171241381](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251114184742961.png)

成功

设置proxifier规则，强制某个程序走代理

proxifier→profile→proxification Rules

点击add

![image-20251114182708687](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251114184747807.png)

这里的bvssh就是ssh的连接工具，强制这个工具的流量走kali的代理

然后测试能不能直接连接ssh

![image-20251114182824924](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251114184753951.png)

![image-20251114182842481](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251114184758143.png)

成功

proxifier抓不到ssh，所以只能抓工具的流量