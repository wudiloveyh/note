```bash
# 1 批量探活 + 拿 Title + 技术指纹
cat subs.txt | httpx -title -tech-detect -status-code -o alive.txt

# 2 指定端口 + 截图（无头浏览器）
httpx -l subs.txt -ports 80,443,8080,8443 -screenshot -sc

# 3 只返回 200/301 且跑 CDN 识别，供后续漏洞扫描
httpx -l subs.txt -mc 200,301 -cdn -csv-export alive.csv
```

一个批量探测域名是否存活的工具