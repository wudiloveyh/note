# ssh密钥登录

## 搞清楚对象是谁

服务器：你要登录的服务器

客户端：你用什么去登录的服务器

假设服务器是centos，我想要用kali免密登录centos，那客户端就是kali，服务器就是centos

## 客户端生成ssh密钥对

```bash
ssh-keygen -t rsa -b 4096 -C "注释"
# 这里的注释写好点，可以区分不同的客户端
```

系统会询问：

```bash
Enter file in which to save the key (/home/user/.ssh/id_rsa):
```

直接回车就好了

它会生成两份文件：

| 文件                          | 说明                     |
| ----------------------------- | ------------------------ |
| `~/.ssh/id_rsa`               | **私钥，秘密，不可泄露** |
| `C:\Users\用户名/.ssh/id_rsa` | **私钥，秘密，不可泄露** |
| `~/.ssh/id_rsa.pub`           | 公钥，可以给服务器       |

## 把公钥上传到服务器

### 自动上传

```bash
ssh-copy-id user@server_ip
```

需要你输入yes确认保存凭据，然后输入服务器的密码回车，接下来就可以不用密码登录服务器了

### 手动上传（安全）

- 查看公钥内容

  ```bash
  cat ~/.ssh/id_rsa.pub
  ```

- 把内容复制到服务器文件

  ```bash
  vim ~/.ssh/authorized_keys
  ```

- 设置权限，不然ssh会拒绝

  ```bash
  chmod 600 ~/.ssh/authorized_keys
  chmod 700 ~/.ssh
  ```

## 检查服务器ssh配置

编辑ssh配置文件

```bash
sudo vim /etc/ssh/sshd_config
```

确保以下内容为yes：

```bash
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

如果修改过，重启 SSH：

```bash
sudo systemctl restart sshd
```

## 使用私钥登录服务器

客户端输入

```bash
ssh user@server-ip
```

如果不需要密码就可以登录则成功

![image-20251115191454733](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251115191454804.png)

## 撤销授权

只需要服务器把公钥的内容去掉就可以了

```bash
vim ~/.ssh/authorized_keys
# 删除对应的公钥就可以了
```

![image-20251115191625624](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251115191625674.png)





