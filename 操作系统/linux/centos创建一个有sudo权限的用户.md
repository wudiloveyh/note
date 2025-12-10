##### centos创建一个有sudo权限的用户

```bash
adduser admin
passwd admin
usermod -aG wheel admin
```

