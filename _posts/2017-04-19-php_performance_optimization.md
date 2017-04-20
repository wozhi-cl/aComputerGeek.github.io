---
layout: post
title:  "php性能优化"
date:   2017-4-20 9:15:08 +0800
tag: php
---

# php性能问题简析

### php性能问题，占整体项目性能问题的比例
![](/images/posts/php/proportion.png)

	性能优化项目，不要局限于优化PHP

### php性能问题的解决方向
> * php语言级的性能优化
> * php周边问题的性能优化(webserver、db、缓存等)
> * php语言自身分析、优化(php底层c)

### 压力测试工具简介
Apache Benchmark(ab)
    
    ab是由Apche提供的压力测试软件。安装apache服务器时会自带该压测软件


使用
```
    ab -n10 -c10 http://www.baidu.com
    //-n 请求数
            //-c 并发数
                    //url 目标压测地址
```

输出
```
Server Software:        BWS/1.1         //服务器
Server Hostname:        www.baidu.com   //域名
Server Port:            80              //端口

Document Path:          /               //访问目录
Document Length:        101932 bytes    //目标站点文档大小

Concurrency Level:      10              
Time taken for tests:   8.096 seconds
Complete requests:      10
Failed requests:        9
   (Connect: 0, Receive: 0, Length: 9, Exceptions: 0)
Total transferred:      1031215 bytes
HTML transferred:       1021625 bytes
Requests per second:    1.24 [#/sec] (mean)      //每秒接收请求数
Time per request:       8096.463 [ms] (mean)
Time per request:       809.646 [ms] (mean, across all concurrent requests)       //每个请求的耗时数
Transfer rate:          124.38 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        3  616 1259.7     31    3012
Processing:  1308 3436 2543.2   2076    7834
Waiting:       28 1969 2524.4    690    6155
Total:       1349 4052 2784.7   5003    7837

Percentage of the requests served within a certain time (ms)
  50%   5003
  66%   5088
  75%   7343
  80%   7429
  90%   7837
  95%   7837
  98%   7837
  99%   7837
 100%   7837 (longest request)

```
    