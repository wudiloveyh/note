# spring-boot-scan

```bash
工具用法如下：
        对单一URL进行信息泄露扫描:         python3 SpringBoot-Scan.py -u http://example.com/
        读取目标TXT进行批量信息泄露扫描:   python3 SpringBoot-Scan.py -uf url.txt
        对单一URL进行漏洞扫描:             python3 SpringBoot-Scan.py -v http://example.com/
        读取目标TXT进行批量漏洞扫描：      python3 SpringBoot-Scan.py -vf url.txt
        扫描并下载SpringBoot敏感文件:      python3 SpringBoot-Scan.py -d http://example.com/
        读取目标TXT进行批量敏感文件扫描:   python3 SpringBoot-Scan.py -df url.txt
        使用HTTP代理并自动进行连通性测试:    python3 SpringBoot-Scan.py -p <代理IP:端口>
        从TXT文件中导入自定义HTTP头部:       python3 SpringBoot-Scan.py -t header.txt
        通过ZoomEye密钥进行API下载数据:      python3 SpringBoot-Scan.py -z <ZoomEye的API-KEY>
        通过Fofa密钥进行API下载数据:         python3 SpringBoot-Scan.py -f <Fofa的API-KEY>
        通过Hunter密钥进行API下载数据:       python3 SpringBoot-Scan.py -y <Hunter的API-KEY>
```

