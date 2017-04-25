---
layout: post
title:  "PHP实现动态页面静态化"
date:   2017-4-20 9:15:08 +0800
tag: php
---

## 关于优化页面响应时间

> * 动态页面静态化
> * 优化数据库
> * 使用负载均衡
> * 使用缓存

# 动态页面静态化

    如果页面中一些内容不经常改动，动态页面静态化时非常有效的加速方法
    
实质：生成静态的html页面

好处：

*   减少服务器脚本的计算时间
*   降低服务器的响应时间

说明：

    不适用于内容经常变换的应用，例如：微博等；

### 动态页面
动态页面：

1.  连接数据库服务器或者缓存服务器
2.  获取数据 
3.  填充到模板
4.  呈现给用户

    php文件执行顺序：
```
graph LR
    a(语法分析) --> b(编译)
    b --> c(运行)

```
### 静态页面
静态页面：直接呈现
    
    静态html文件执行顺序：
```
graph TD
    a(运行)
```

## 静态化介绍

```
graph LR
    a(php静态化) --> b1(纯静态)
    a --> b2(伪静态)
    b1 --> c1(局部静态)
    b1 --> c2(全部纯净态)
```    

## buffer认知

    buffer其实就是缓冲区，一个内存地址空间，主要用于数据区域
    
我们平常在文件中输入一些字符，按下保存时，内容并不是直接保存到磁盘中，而是储存到一个buffer中，当buffer中的内容装满的时候，再存入磁盘中
    
php中的输出流程
```
graph LR
    a(输出语句) --> b(php buffer) 
    b --> c(tcp)
    c --> d(终端)
```
### 开启output_buffering (php5.6默认开启)

1.php.ini
    
    output_buffering = on
2.在php文件中开启
    
    ob_start();

与output_buffering的函数

    file_put_contents()          //将一个字符串写入文件，成功返回写入到文件的字节数，失败返回false
    filemtime()                 //获取文件最后修改时间
    
    fopen()                     //打开文件资源
    fwrite()                    //写入
    fclose()                    //关闭文件资源
    
    ob_start()                  //打开输出控制缓冲
    ob_get_contents()           //获取output_buffering中的内容
    ob_clean()                  //清空（擦掉）输出缓冲区
    ob_get_clean()              //得到当前缓冲区的内容并删除当前输出缓冲区
        
## 如何触发系统生成纯净态化页面
* 页面添加缓存时间
* 手动触发方式（直接手动生成）
* crontab定时任务

### 页面添加缓存时间

```
graph TD
    a(用户请求页面) --> b(页面时间是否过期)
    b -->|是| c(动态页面并生成一份新的静态页面)
    b -->|否| d(获取静态页面)
```
### crontab定时任务

linux系统工具
    
    */5 * * * * php webserver目录/显示的脚本


## 静态页面实现局部动态化

用ajax加载页面数据

# 伪静态

> * 让url美观
> * 最主要的是为了搜索引擎方便搜索引擎蜘蛛(Spider)来抓取主页上的相关内容; 

### 伪静态
```
graph TD
    a(http://state.com/newsList.php?type=2&category_id=1) -->|伪静态| b(http://state.com/newsList.php/2/1.html)
```  
### php通过正则处理为静态

nginx服务器默认情况下不支持path_info模式，需要配置
``` php

$url="http://state.com/newsList.php/2/1.html";
if(preg_match('/^\/(\d+)\/(\d+).html/',$_SERVER['PATH_INFO']，$arr))
{
    $type=$arr[1];
    $category_id=$arr[2];
}
```

### web服务器rewrite配置
> * apache下rewrite配置
> * nginx下rewrite配置

#### apache下的rewrite配置
1、httpd.conf文件中开启相关模式

开启vhosts配置，开启重写模块功能
```
LoadModule rewrite_module modules/mod_rewrite.so
Include conf/extra/httpd-vhosts.conf
```

2、httpd_vhosts.conf配置文件配置相关信息

```
<VirtualHost 127.0.0.1:80>
    ServerAdmin 管理员邮箱
    DocumentRoot "根目录"
    ServerName 域名
    ErrorLog "错误日志储存位置"
    CustomLog "成功访问日志"
    
    RewriteEngine on    //开启重写
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME}!-d      //如果服务器中存在这个url中的目录，就访问目录
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME}!-f      //如果服务器中存在这个url中的文件，就访问文件
    RewriteRule ^/detail/([0-9]*).html$ /detail.php?id=$1    //重写规则，/detail/([0-9]*).html  转化为 detail.php?id= ([0-9]*)
</VirtualHost>
```

#### nginx下的rewrite配置
```
rewrite ^/detail/(\d+)\.html$ /detail.php?id=$1 lasr;
```
