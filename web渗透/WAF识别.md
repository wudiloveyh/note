# WAF识别

知道是否有waf，用的什么waf，我们才好对症下药

## 手动

一般来说waf都会拦截敏感文件下载，例如a.sql、b.mdb等文件，直接在url后面拼接就可以了

如果waf不防护这些的话可以用一些xsspayload或者sqlpayload测试

## 工具

工具就是重复手动的操作

好用的工具：wafw00f

https://github.com/EnableSecurity/wafw00f

```bash
waf00f https://www.baidu.com
```



