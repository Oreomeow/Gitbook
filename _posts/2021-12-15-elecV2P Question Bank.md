---
title: elecV2P Question Bank
author: Oreo
date: 2021-12-15
category: elecV2P
layout: post
---

## 01-overview

1. 下列哪项不属于elecV2P的基础功能？

    FEED/IFTTT/自定义 通知  
    EFSS 基础文件管理  
    查看/修改网络请求 (MITM)  
    ARIA2 下载文件  
    定时执行 JS/SHELL 脚本  

## 02-Docker

1. 以下哪条命令能正确进入刚安装好的elecv2p内部？假设你的容器名为elecv2p。

    docker exec -it elecv2p bash  
    docker logs elecv2p -f  
    docker pull elecv2/elecv2p  
    docker exec -it v2p ash  
    docker start elecv2p  
    docker exec -it v2p bash  
    docker exec -it elecv2p sh  
    docker exec -it elecv2p /bin/bash  

## 03-rules

1. 下列哪个不属于elecV2P的RULES匹配方式？

    repbody  
    resbody  
    useragent  
    host  
    cookie  
    url  
    resstatus  
    restype  
    reqmethod  

## 04-JS

1. elecV2P的JS运行环境是基于 vm 模块的虚拟环境，同时增加了一些默认的环境变量及函数。下列哪个不是？

    \$cheerio  
    \$done  
    \_\_efss  
    \_\_name  
    \$fend  
    \_\_dirname  
    \$log  
    \$feed  

## 06-task

1. 现需要在elecV2P中添加一个倒计时任务，要求：1~3小时内运行一次任务，总计重复8~15次。试求时间格式。

    1 3 8 15  
    1 2 8 7  
    3600 7200 8 7  
    3600 10800 8 15  
    3600 8 7200 15  
    3600 8 7 7200  
    3600 8 7200 7  
    3600 7200 8 15  
    3600 7200 7 8  
    3600 7200 7 15  
    3600 7 8 7200  

## 07-feed&notify

1. 下列哪个不是elecV2P自带通知方式？

    BARK 通知  
    FEED RSS 订阅  
    自定义通知  
    通知触发 JS  
    IFTTT WEBHOOK  
    邮件通知  

## 08-logger&efss

1. elecV2P版本3.4.6之前没有的日志分类项是什么？

    错误日志 errors.log  
    定时任务 shell 指令日志 任务名.task.log  
    访问日志 access.log  
    shell 命令执行日志 funcExec.log  
    JAVASCRIPT 运行日志 xxx.js.log  
    其他未处理类日志 elecV2Proc.log  
