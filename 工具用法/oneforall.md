| 场景                           | 命令                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| 单域名快速收集                 | `python oneforall.py --target example.com run`               |
| 批量域名（一行一个）           | `python oneforall.py --targets domains.txt run`              |
| 带爆破 + 存活验证 + 输出 Excel | `python oneforall.py --target example.com --brute True --dns True --fmt xlsx run` |
