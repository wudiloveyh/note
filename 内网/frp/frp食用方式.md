[toc]

# frp食用方式

## 项目地址

https://github.com/fatedier/frp.git

linux-amd64：

https://github.com/fatedier/frp/releases/download/v0.52.0/frp_0.52.0_linux_amd64.tar.gz

下载命令：

```bash
curl -L -O https://github.com/fatedier/frp/releases/download/v0.52.0/frp_0.52.0_linux_amd64.tar.gz
```

windows-amd64：

https://github.com/fatedier/frp/releases/download/v0.52.0/frp_0.52.0_windows_amd64.zip

下载命令：

```bash
curl -L -O https://github.com/fatedier/frp/releases/download/v0.52.0/frp_0.52.0_windows_amd64.zip
```



## 环境

内网服务器-centos7

192.168.49.233



公网服务器-kali

192.168.10.129

192.168.49.134



windows11（本机）

192.168.10.1

无法访问centos7



## 配置

公网服务器-kali

frps.ini

```bash
[common]
bind_addr = 0.0.0.0
bind_port = 7000            # frpc 将连接这个端口
token = my_secret_token     # 必须与 frpc 的 token 一致，增强安全

# 下面是不带注释版本
[common]
bind_addr = 0.0.0.0
bind_port = 7000            
token = chuxue_token     
```

内网服务器-centos7

frpc.ini

```bash
[common]
server_addr = <UBUNTU_PUBLIC_IP>   # 把 <> 换成你的公网 Ubuntu IP 或域名
server_port = 7000
token = my_secret_token            # 必须和 frps 一致

[ssh_to_centos]
type = tcp
local_ip = 127.0.0.1
local_port = 22                     # 要映射的内网 CentOS SSH 服务端口
remote_port = 6000                  # 在公网 Ubuntu 上对外暴露的端口，Windows 连接该端口即到内网 SSH
# 注意：remote_port 也可以设为 0，让 frps 随机分配，但通常为了易用设固定端口

# 下面是不带注释版本

[common]
server_addr = 192.168.49.134   
server_port = 7000
token = chuxue_token            

[ssh_to_centos]
type = tcp
local_ip = 127.0.0.1
local_port = 22                     
remote_port = 6000                  
```

## 测试连接

公网运行服务-kali：

```bash
./frps -c frps.ini
```

内网运行客户端-centos：

```bash
./frpc -c frpc.ini
```

成功建立连接之后，windows连接kali的6000端口就可以ssh连接centos内网服务器的22端口

windows：

```bash
ssh 192.168.10.129 -p 6000 -l <内网服务器用户>
```

效果：

![image-20251113230054550](C:%5CUsers%5Cchuxue%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20251113230054550.png)

## 添加为系统服务

建议把frps.ini和frpc.ini放在/etc/frp，并用systemd管理

公网服务器-kali

```bash
mkdir /etc/frp
cp frps.ini /etc/frp/frps.ini
cp frps /usr/local/bin/frps
```

内网服务器-centos

```bash
mkdir /etc/frp
cp frpc.ini /etc/frp/frpc.ini
cp frpc /usr/local/bin/frpc
```

### 创建单元

公网-kali

创建文件

/etc/systemd/system/frps.service

内容：

```bash
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

内网-centos

创建文件

/etc/systemd/system/frpc.service

内容：

```bash
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

公网-kali：

运行

```bash
sudo systemctl daemon-reload 
sudo systemctl enable --now frps
```

内网-centos：

运行

```bash
sudo systemctl daemon-reload 
sudo systemctl enable --now frpc
```

## 注意事项

如果是公网服务器，那就得开放对应的端口：

```bash
# kali 使用 ufw： 
sudo ufw allow 7000/tcp 
sudo ufw allow 6000/tcp 
# 如果启用了 dashboard
sudo ufw allow 7500/tcp 
sudo ufw reload
```

然后还得在安全组开放对应的端口（7000、6000、7500）





