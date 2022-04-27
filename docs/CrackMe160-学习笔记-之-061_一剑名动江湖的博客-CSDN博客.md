<!--yml
category: crackme160
date: 2022-04-27 18:15:48
-->

# CrackMe160 学习笔记 之 061_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/105990519](https://blog.csdn.net/guaigle001/article/details/105990519)

## 前言

这个题目太依赖日期格式了，只有yy/mm/dd格式的短日期才能通过验证。

## 分析

### 判断输入

```
00402FD0   .  E8 0DE3FFFF   call    <jmp.&MSVBVM60.__vbaStrCmp>      ;  判断输入不为空
00402FD5   .  8BF0          mov     esi, eax
00402FD7   .  F7DE          neg     esi
00402FD9   .  1BF6          sbb     esi, esi
00402FDB   .  46            inc     esi
00402FDC   .  F7DE          neg     esi
00402FDE   .  FF75 DC       push    dword ptr [ebp-24]
00402FE1   .  68 10264000   push    00402610
00402FE6   .  E8 F7E2FFFF   call    <jmp.&MSVBVM60.__vbaStrCmp>      ;  判断输入不为空
00402FEB   .  F7D8          neg     eax
00402FED   .  1BC0          sbb     eax, eax
00402FEF   .  40            inc     eax
00402FF0   .  F7D8          neg     eax
00402FF2   .  66:0BF0       or      si, ax
00402FF5   .  FF75 D8       push    dword ptr [ebp-28]
00402FF8   .  68 10264000   push    00402610
00402FFD   .  E8 E0E2FFFF   call    <jmp.&MSVBVM60.__vbaStrCmp>      ;  判断输入不为空 
```

### 关键比较

```
004030E9   .  FF90 F8060000 call    dword ptr [eax+6F8]              ;  年份函数
004030EF   .  8985 48FFFFFF mov     dword ptr [ebp-B8], eax
004030F5   .  83BD 48FFFFFF>cmp     dword ptr [ebp-B8], 0
004030FC   .  7D 20         jge     short 0040311E
004030FE   .  68 F8060000   push    6F8
00403103   .  68 BC244000   push    004024BC
00403108   .  FF75 08       push    dword ptr [ebp+8]
0040310B   .  FFB5 48FFFFFF push    dword ptr [ebp-B8]
00403111   .  E8 C0E1FFFF   call    <jmp.&MSVBVM60.__vbaHresultCheck>
00403116   .  8985 10FFFFFF mov     dword ptr [ebp-F0], eax
0040311C   .  EB 07         jmp     short 00403125
0040311E   >  83A5 10FFFFFF>and     dword ptr [ebp-F0], 0
00403125   >  8B45 08       mov     eax, dword ptr [ebp+8]
00403128   .  8B4D 08       mov     ecx, dword ptr [ebp+8]
0040312B   .  8B40 78       mov     eax, dword ptr [eax+78]          ;  返回的值当前年份
0040312E   .  2B81 84000000 sub     eax, dword ptr [ecx+84]          ;  输入的年份
00403134   .  0F80 B7020000 jo      004033F1
0040313A   .  85C0          test    eax, eax
0040313C   .  75 46         jnz     short 00403184                   ;  关键跳 
```

同理，剩下的是对月份和日期计算进行验证

## 注册机代码

```
#include<stdio.h>
int main(int argc ,char ** argv)
{
  if(argc!=4)
    return 0;
  int year,month,day;
  year=atoi(argv[1]);
  month=atoi(argv[2]);
  day=atoi(argv[3]);
  printf("%d %d %d,",year,month,day);
  printf("Part1:%d\n",year);
  printf("Part2:%d\n",(year+month)*month);
  printf("Part3:%d\n",((year+month)*month+day)*day);
  return 0;
} 
```