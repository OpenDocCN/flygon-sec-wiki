<!--yml
category: crackme160
date: 2022-04-27 18:15:52
-->

# CrackMe160 学习笔记 之 058_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/105891546](https://blog.csdn.net/guaigle001/article/details/105891546)

## 前言

这是个VB程序。虽然VB的反汇编代码看起来有点麻烦，这个却不复杂。

## 思路

程序的逻辑大致如下。

输入 **“use hexeditor to look for hardcoded codes”** -> **"Yes! You have solved it!!"**

输入 **“Use bpx __vbastrcomp to break with Softice” “Use hexeditor to look for hardcoded codes”
“Use SmartCheck to look for the code”** 三个中的一个 -> **"It’s not that easy!!"**

所以，偷懒的话，直接把第一个字符串改小写就能通过验证了。

## 分析

### 关键判断

```
00403129   .  FF15 10614000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
0040312F   .  F7D8          neg     eax
00403131   .  1BC0          sbb     eax, eax
00403133   .  F7D8          neg     eax
00403135   .  F7D8          neg     eax
00403137   .  8945 C0       mov     dword ptr [ebp-40], eax
0040314C   .  66:837D C0 00 cmp     word ptr [ebp-40], 0
00403152   .  0F84 FD030000 je      00403555 
```

其中

```
0040312F   .  F7D8          neg     eax
00403131   .  1BC0          sbb     eax, eax 
```

这两条指令是限制返回值在-1到0之间的，虽然我觉得stcrmp本来返回范围就在这之内，用不着加这个指令。

```
004031F4   .  68 DC264000   push    004026DC                         ;  UNICODE "Use bpx __vbastrcomp to break with Softice"
004031F9   .  FF15 10614000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
004031FF   .  F7D8          neg     eax
00403201   .  1BC0          sbb     eax, eax
00403203   .  40            inc     eax
00403204   .  F7D8          neg     eax
00403206   .  66:8945 9C    mov     word ptr [ebp-64], ax
0040320A   .  8B45 DC       mov     eax, dword ptr [ebp-24]
0040320D   .  50            push    eax
0040320E   .  68 38274000   push    00402738                         ;  UNICODE "Use hexeditor to look for hardcoded codes"
00403213   .  FF15 10614000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
00403219   .  F7D8          neg     eax
0040321B   .  1BC0          sbb     eax, eax
0040321D   .  40            inc     eax
0040321E   .  F7D8          neg     eax
00403220   .  8B4D 9C       mov     ecx, dword ptr [ebp-64]
00403223   .  0BC8          or      ecx, eax
00403225   .  66:894D 9C    mov     word ptr [ebp-64], cx
00403229   .  8B55 D8       mov     edx, dword ptr [ebp-28]
0040322C   .  52            push    edx
0040322D   .  68 90274000   push    00402790                         ;  UNICODE "Use SmartCheck to look for the code"
00403232   .  FF15 10614000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
00403238   .  F7D8          neg     eax
0040323A   .  1BC0          sbb     eax, eax
0040323C   .  40            inc     eax
0040323D   .  F7D8          neg     eax
0040323F   .  8B4D 9C       mov     ecx, dword ptr [ebp-64]
00403242   .  0BC8          or      ecx, eax
00403244   .  894D B0       mov     dword ptr [ebp-50], ecx 
```

通过异或保存最后比较的值。