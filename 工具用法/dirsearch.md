# dirsearch

| 需求                    | 命令示例                                                     | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ① **轻度限**            | `python3 dirsearch.py -u https://site.com -t 10 --delay 0.5` | 线程降到 10，每条间隔 0.5 s，**QPS≈20**，小站足够安全。      |
| ② **中度限**            | `python3 dirsearch.py -u https://site.com -t 5 --delay 1 --random-agent` | 再砍一半线程，**随机 UA**，把特征打散。                      |
| ③ **重度限（高防/CF）** | `python3 dirsearch.py -u https://site.com -t 2 --delay 2 --proxy http://127.0.0.1:8080 --max-rate 30` | 并发 2，延迟 2 s，**代理池**轮换，硬上限 30 r/min，基本无视 CC 策略。 |
| ④ **“扫一周”模式**      | `python3 dirsearch.py -u https://site.com -t 1 --delay 5 --timeout 20 --retries 2 --skip-on-status 429` | 单线程 5 s 间隔，**遇 429 自动跳目录**，适合 SRC 长线打点。  |