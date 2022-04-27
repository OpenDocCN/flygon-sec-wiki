<!--yml
category: crackme160
date: 2022-04-27 18:16:46
-->

# CrackMe160 学习笔记 之 025_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/104251521](https://blog.csdn.net/guaigle001/article/details/104251521)

## 前言

偶尔做点简单的题放松一下。

## 思路

### 去弹窗

搜索字符串，找到弹窗初始化的函数。

```
00405905      55            push    ebp 
```

改为**ret**指令即可。

搜索字符串，看到“**55555**”，心想，这别是验证码吧。

输入，验证通过。

没想到还就真的是。

太简单都不想截图了。

水博客不需要理由。

![在这里插入图片描述](img/1f39c4b78d7314f45809cacc8da64fdf.png)