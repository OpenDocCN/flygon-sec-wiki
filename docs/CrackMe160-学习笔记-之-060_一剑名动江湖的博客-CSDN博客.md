<!--yml
category: crackme160
date: 2022-04-27 18:15:49
-->

# CrackMe160 学习笔记 之 060_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/105973818](https://blog.csdn.net/guaigle001/article/details/105973818)

## 前言

这是个VB程序，通过对输入字符串运算来判断。

## 思路

### 关键数据初始化

```
00402B1E   .  C745 B4 B96DA>mov     dword ptr [ebp-4C], 0AF6DB9
00402B25   .  C745 B8 FFFFF>mov     dword ptr [ebp-48], 7FFFFFFF
00402B2C   .  C745 E0 52000>mov     dword ptr [ebp-20], 52
00402B33   .  C745 BC 65000>mov     dword ptr [ebp-44], 65
00402B3A   .  C745 B0 76000>mov     dword ptr [ebp-50], 76
00402B41   .  C745 CC 72000>mov     dword ptr [ebp-34], 72
00402B48   .  C745 C4 73000>mov     dword ptr [ebp-3C], 73
00402B4F   .  C785 F4FEFFFF>mov     dword ptr [ebp-10C], 1
00402B59   .  C785 ECFEFFFF>mov     dword ptr [ebp-114], 2 
```

### 将输入字符串各字符累加和固定字符串"Reverse"各字符累加比较

```
00402D90   .  8B45 E0       mov     eax, dword ptr [ebp-20]          
00402D93   .  0345 BC       add     eax, dword ptr [ebp-44]
00402D96   .  0F80 910A0000 jo      0040382D
00402D9C   .  0345 B0       add     eax, dword ptr [ebp-50]
00402D9F   .  0F80 880A0000 jo      0040382D
00402DA5   .  0345 BC       add     eax, dword ptr [ebp-44]
00402DA8   .  0F80 7F0A0000 jo      0040382D
00402DAE   .  0345 CC       add     eax, dword ptr [ebp-34]
00402DB1   .  0F80 760A0000 jo      0040382D
00402DB7   .  0345 C4       add     eax, dword ptr [ebp-3C]
00402DBA   .  0F80 6D0A0000 jo      0040382D
00402DC0   .  0345 BC       add     eax, dword ptr [ebp-44]
00402DC3   .  0F80 640A0000 jo      0040382D
00402DC9   .  8945 C8       mov     dword ptr [ebp-38], eax
00402DCC   .  8B45 C0       mov     eax, dword ptr [ebp-40]
00402DCF   .  3B45 C8       cmp     eax, dword ptr [ebp-38]
00402DD2   .  0F85 B9060000 jnz     00403491 
```

### 验证字符串第2，4，7位是否为e

```
00402F65   .  50            push    eax                                          ; /Length8
00402F66   .  6A 02         push    2                                            ; |Start = 2
00402F68   .  8D85 7CFFFFFF lea     eax, dword ptr [ebp-84]                      ; |
00402F6E   .  50            push    eax                                          ; |dString8
00402F6F   .  8D85 5CFFFFFF lea     eax, dword ptr [ebp-A4]                      ; |
00402F75   .  50            push    eax                                          ; |RetBUFFER
00402F76   .  E8 C9E2FFFF   call    <jmp.&MSVBVM60.#632>                         ; \rtcMidCharVar
00402F7B   .  C785 44FFFFFF>mov     dword ptr [ebp-BC], 1
00402F85   .  C785 3CFFFFFF>mov     dword ptr [ebp-C4], 2
00402F8F   .  8B45 A4       mov     eax, dword ptr [ebp-5C]
00402F92   .  8985 70FEFFFF mov     dword ptr [ebp-190], eax
00402F98   .  8365 A4 00    and     dword ptr [ebp-5C], 0
00402F9C   .  8B85 70FEFFFF mov     eax, dword ptr [ebp-190]
00402FA2   .  8985 54FFFFFF mov     dword ptr [ebp-AC], eax
00402FA8   .  C785 4CFFFFFF>mov     dword ptr [ebp-B4], 8
00402FB2   .  8D85 3CFFFFFF lea     eax, dword ptr [ebp-C4]
00402FB8   .  50            push    eax                                          ; /Length8
00402FB9   .  6A 04         push    4                                            ; |Start = 4
00402FBB   .  8D85 4CFFFFFF lea     eax, dword ptr [ebp-B4]                      ; |
00402FC1   .  50            push    eax                                          ; |dString8
00402FC2   .  8D85 2CFFFFFF lea     eax, dword ptr [ebp-D4]                      ; |
00402FC8   .  50            push    eax                                          ; |RetBUFFER
00402FC9   .  E8 76E2FFFF   call    <jmp.&MSVBVM60.#632>                         ; \rtcMidCharVar
00402FCE   .  C785 14FFFFFF>mov     dword ptr [ebp-EC], 1
00402FD8   .  C785 0CFFFFFF>mov     dword ptr [ebp-F4], 2
00402FE2   .  8B45 9C       mov     eax, dword ptr [ebp-64]
00402FE5   .  8985 6CFEFFFF mov     dword ptr [ebp-194], eax
00402FEB   .  8365 9C 00    and     dword ptr [ebp-64], 0
00402FEF   .  8B85 6CFEFFFF mov     eax, dword ptr [ebp-194]
00402FF5   .  8985 24FFFFFF mov     dword ptr [ebp-DC], eax
00402FFB   .  C785 1CFFFFFF>mov     dword ptr [ebp-E4], 8
00403005   .  8D85 0CFFFFFF lea     eax, dword ptr [ebp-F4]
0040300B   .  50            push    eax                                          ; /Length8
0040300C   .  6A 07         push    7                                            ; |Start = 7
0040300E   .  8D85 1CFFFFFF lea     eax, dword ptr [ebp-E4]                      ; |
00403014   .  50            push    eax                                          ; |dString8
00403015   .  8D85 FCFEFFFF lea     eax, dword ptr [ebp-104]                     ; |
0040301B   .  50            push    eax                                          ; |RetBUFFER
0040301C   .  E8 23E2FFFF   call    <jmp.&MSVBVM60.#632>                         ; \rtcMidCharVar
00403021   .  8D85 5CFFFFFF lea     eax, dword ptr [ebp-A4]
00403027   .  50            push    eax                                          ; /String8
00403028   .  8D45 A8       lea     eax, dword ptr [ebp-58]                      ; |
0040302B   .  50            push    eax                                          ; |ARG2
0040302C   .  E8 19E2FFFF   call    <jmp.&MSVBVM60.__vbaStrVarVal>               ; \__vbaStrVarVal
00403031   .  50            push    eax                                          ; /String
00403032   .  E8 19E2FFFF   call    <jmp.&MSVBVM60.#516>                         ; \rtcAnsiValueBstr
00403037   .  0FBFF0        movsx   esi, ax
0040303A   .  2B75 BC       sub     esi, dword ptr [ebp-44]
0040303D   .  F7DE          neg     esi
0040303F   .  1BF6          sbb     esi, esi
00403041   .  46            inc     esi
00403042   .  F7DE          neg     esi                                          ;  对esi进行运算
00403044   .  8D85 2CFFFFFF lea     eax, dword ptr [ebp-D4]
0040304A   .  50            push    eax                                          ; /String8
0040304B   .  8D45 A0       lea     eax, dword ptr [ebp-60]                      ; |
0040304E   .  50            push    eax                                          ; |ARG2
0040304F   .  E8 F6E1FFFF   call    <jmp.&MSVBVM60.__vbaStrVarVal>               ; \__vbaStrVarVal
00403054   .  50            push    eax                                          ; /String
00403055   .  E8 F6E1FFFF   call    <jmp.&MSVBVM60.#516>                         ; \rtcAnsiValueBstr
0040305A   .  0FBFC0        movsx   eax, ax
0040305D   .  2B45 BC       sub     eax, dword ptr [ebp-44]
00403060   .  F7D8          neg     eax
00403062   .  1BC0          sbb     eax, eax
00403064   .  40            inc     eax
00403065   .  F7D8          neg     eax                                          ;  对eax进行运算
00403067   .  66:23F0       and     si, ax
0040306A   .  8D85 FCFEFFFF lea     eax, dword ptr [ebp-104]
00403070   .  50            push    eax                                          ; /String8 = 0012F404
00403071   .  8D45 98       lea     eax, dword ptr [ebp-68]                      ; |
00403074   .  50            push    eax                                          ; |ARG2
00403075   .  E8 D0E1FFFF   call    <jmp.&MSVBVM60.__vbaStrVarVal>               ; \__vbaStrVarVal
0040307A   .  50            push    eax                                          ; /String
0040307B   .  E8 D0E1FFFF   call    <jmp.&MSVBVM60.#516>                         ; \rtcAnsiValueBstr
00403080   .  0FBFC0        movsx   eax, ax
00403083   .  2B45 BC       sub     eax, dword ptr [ebp-44]
00403086   .  F7D8          neg     eax
00403088   .  1BC0          sbb     eax, eax
0040308A   .  40            inc     eax
0040308B   .  F7D8          neg     eax
0040308D   .  66:23F0       and     si, ax
00403090   .  66:89B5 B0FEF>mov     word ptr [ebp-150], si 
```

## 结尾

经测试此题目存在多解，满足字符串累加求和为**0x2DC**以及**2，4，7位为e**即可通过验证