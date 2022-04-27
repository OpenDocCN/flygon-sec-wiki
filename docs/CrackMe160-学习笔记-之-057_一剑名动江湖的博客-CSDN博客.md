<!--yml
category: crackme160
date: 2022-04-27 18:15:53
-->

# CrackMe160 学习笔记 之 057_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/104580316](https://blog.csdn.net/guaigle001/article/details/104580316)

## 前言

简单题，字符串都是明文比较。

## 思路

通过

```
00403682   .  E8 1FDDFFFF   call    <jmp.&MSVBVM60.__vbaVarTstEq>    ; \__vbaVarTstEq 
```

判断是选择了哪个弹窗

再通过

```
00403696   .  FF90 F8060000 call    dword ptr [eax+6F8] 
```

进入真正的判断

最后再通过真正判断后赋的值判断。

因为太简单了，所以直接写答案。

这题感觉像是在复习vb的数据类型，虽然我不会vb。

*   String : String
*   Variant: Empty
*   Long: 2895790
*   Currency: 13579.2468
*   Single: 9764318000000(科学计数法)
*   Double: 147258369789456000(科学计数法)
*   Integer: 23535
*   Byte: 239