<!--yml
category: crackme160
date: 2022-04-27 18:16:53
-->

# CrackMe160 学习笔记 之 022_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/104223639](https://blog.csdn.net/guaigle001/article/details/104223639)

## 前言

这个程序和**017**有点差不多，都不是明文字符串比较。

虽然作者尝试用冗长的程序来恶心人，在我看来，其实没什么效果。

## 思路

输入字符，来到验证函数，发现是用数字1和0比较(输入错误时)。

说明这里不是真正的验证函数。

因为有过调试这种程序的经验，所以很快找到了真正的验证函数。

然后开始分析。

如果你一开始直接修改跳转，注册成功。你会发现接下来注册不了了。

其实删除**c:\windows\MTR.dat**文件就行了。

## 分析

### 打开程序时

```
00402B38   .  68 40224000   push    00402240                         ;  UNICODE "c:\windows\MTR.dat"
00402B3D   .  56            push    esi
00402B3E   .  FF15 90614000 call    dword ptr [<&MSVBVM50.__vbaI2Var>;  MSVBVM50.__vbaI2Var
00402B44   .  50            push    eax
00402B45   .  6A FF         push    -1
00402B47   .  6A 20         push    20
00402B49   .  FF15 98614000 call    dword ptr [<&MSVBVM50.__vbaFileO>;  MSVBVM50.__vbaFileOpen
00402B4F   .  56            push    esi
00402B50   .  FF15 90614000 call    dword ptr [<&MSVBVM50.__vbaI2Var>;  MSVBVM50.__vbaI2Var
00402B56   .  50            push    eax
00402B57   .  8D47 54       lea     eax, dword ptr [edi+54]
00402B5A   .  6A 2D         push    2D
00402B5C   .  50            push    eax
00402B5D   .  6A 0A         push    0A
00402B5F   .  FF15 2C614000 call    dword ptr [<&MSVBVM50.__vbaGetFx>;  MSVBVM50.__vbaGetFxStr4
00402B65   .  56            push    esi
00402B66   .  FF15 90614000 call    dword ptr [<&MSVBVM50.__vbaI2Var>;  MSVBVM50.__vbaI2Var
00402B6C   .  50            push    eax
00402B6D   .  FF15 60614000 call    dword ptr [<&MSVBVM50.__vbaFileC>;  MSVBVM50.__vbaFileClose
00402B73   .  8D47 54       lea     eax, dword ptr [edi+54]
00402B76   .  50            push    eax
00402B77   .  6A 0A         push    0A
00402B79   .  FF15 50614000 call    dword ptr [<&MSVBVM50.__vbaStrFi>;  MSVBVM50.__vbaStrFixstr
00402B7F   .  8BD0          mov     edx, eax
00402B81   .  8D4D E8       lea     ecx, dword ptr [ebp-18]
00402B84   .  FF15 CC614000 call    dword ptr [<&MSVBVM50.__vbaStrMo>;  MSVBVM50.__vbaStrMove
00402B8A   .  50            push    eax
00402B8B   .  68 6C224000   push    0040226C                         ;  UNICODE "trv2156j0e"
00402B90   .  FF15 68614000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
00402B96   .  8BF0          mov     esi, eax
00402B98   .  8D4D E8       lea     ecx, dword ptr [ebp-18]
00402B9B   .  F7DE          neg     esi
00402B9D   .  1BF6          sbb     esi, esi
00402B9F   .  46            inc     esi
00402BA0   .  F7DE          neg     esi
00402BA2   .  FF15 DC614000 call    dword ptr [<&MSVBVM50.__vbaFreeS>;  MSVBVM50.__vbaFreeStr
00402BA8   .  66:85F6       test    si, si
00402BAB   .  0F84 B5000000 je      00402C66
00402BB1   .  57            push    edi
00402BB2   .  FF93 0C030000 call    dword ptr [ebx+30C]
00402BB8   .  8D55 E4       lea     edx, dword ptr [ebp-1C]
00402BBB   .  50            push    eax
00402BBC   .  52            push    edx
00402BBD   .  FF15 3C614000 call    dword ptr [<&MSVBVM50.__vbaObjSe>;  MSVBVM50.__vbaObjSet
00402BC3   .  8BF0          mov     esi, eax
00402BC5   .  68 88224000   push    00402288                         ;  UNICODE "REGISTERED" 
```

打开程序时首先会读取**c盘windows目录下的MTR.dat**文件。如果里面的内容和"**trv2156j0e**"相等，即为注册成功。

如果注册成功，该文件会被写入"**trv2156j0e**“。

### 点击注册按钮

```
00402D20   > \55            push    ebp
00402D21   .  8BEC          mov     ebp, esp
00402D7E   .  FF93 F8060000 call    dword ptr [ebx+6F8]              ;  真正的验证函数
00402D9A   > \8D4E 34       lea     ecx, dword ptr [esi+34]
00402D9D   .  8D55 94       lea     edx, dword ptr [ebp-6C]
00402DA0   .  51            push    ecx                              ; /var18
00402DA1   .  52            push    edx                              ; |var28
00402DA2   .  C745 9C 01000>mov     dword ptr [ebp-64], 1            ; |
00402DA9   .  C745 94 02800>mov     dword ptr [ebp-6C], 8002         ; |最后验证得到的返回值和1比较，如果相等则成功
00402DB0   .  FF15 6C614000 call    dword ptr [<&MSVBVM50.__vbaVarTs>; \__vbaVarTstEq
00402DB6   .  8B3D C4614000 mov     edi, dword ptr [<&MSVBVM50.__vba>;  MSVBVM50.__vbaVarDup
00402DBC   .  B9 04000280   mov     ecx, 80020004
00402DC1   .  66:85C0       test    ax, ax
00402DC4   .  B8 0A000000   mov     eax, 0A
00402DC9   .  894D AC       mov     dword ptr [ebp-54], ecx
00402DCC   .  894D BC       mov     dword ptr [ebp-44], ecx
00402DCF   .  8945 A4       mov     dword ptr [ebp-5C], eax
00402DD2   .  8945 B4       mov     dword ptr [ebp-4C], eax
00402DD5   .  C745 8C 08234>mov     dword ptr [ebp-74], 00402308     ;  UNICODE "CrackMe v1.0"
00402DDC   .  C745 84 08000>mov     dword ptr [ebp-7C], 8
00402DE3   .  8D55 84       lea     edx, dword ptr [ebp-7C]
00402DE6   .  8D4D C4       lea     ecx, dword ptr [ebp-3C]
00402DE9   .  0F84 5A010000 je      00402F49                         ;  关键跳 
```

### 真正的验证函数

```
00403230   > \55            push    ebp
00403231   .  8BEC          mov     ebp, esp
004033A9   .  FF50 50       call    dword ptr [eax+50]               ;  固定字符串地址移动到ebp-1C中
004035FA   .  FF90 A0000000 call    dword ptr [eax+A0]               ;  输入字符串移动到ebp-18中
00403616   > \8B45 E8       mov     eax, dword ptr [ebp-18]          ;  eax保存输入的字符串地址
00403619   .  8B3D 58614000 mov     edi, dword ptr [<&MSVBVM50.#632>>;  MSVBVM50.rtcMidCharVar
0040361F   .  8985 ACFDFFFF mov     dword ptr [ebp-254], eax         ;  ebp-254保存输入字符串地址
00403625   .  8B45 E4       mov     eax, dword ptr [ebp-1C]          ;  固定字符串地址保存到eax中
00403628   .  8D55 84       lea     edx, dword ptr [ebp-7C]
0040362B   .  8945 9C       mov     dword ptr [ebp-64], eax
0040362E   .  52            push    edx                              ; /长度为1
0040362F   .  8D45 94       lea     eax, dword ptr [ebp-6C]          ; |
00403632   .  6A 06         push    6                                ; |Start = 6
00403634   .  8D8D 74FFFFFF lea     ecx, dword ptr [ebp-8C]          ; |
0040363A   .  BB 02000000   mov     ebx, 2                           ; |
0040363F   .  50            push    eax                              ; |固定字符串"b.P.e. .C.r.a.c.k.M.e. . . .v.1...0..........
00403640   .  51            push    ecx                              ; |RetBUFFER
00403641   .  8975 E8       mov     dword ptr [ebp-18], esi          ; |
00403644   .  C785 A4FDFFFF>mov     dword ptr [ebp-25C], 8008        ; |
0040364E   .  C745 8C 01000>mov     dword ptr [ebp-74], 1            ; |
00403655   .  895D 84       mov     dword ptr [ebp-7C], ebx          ; |
00403658   .  8975 E4       mov     dword ptr [ebp-1C], esi          ; |
0040365B   .  C745 94 08000>mov     dword ptr [ebp-6C], 8            ; |
00403662   .  FFD7          call    edi                              ; \取固定字符串第0x06个字符‘r’保存到ebp-84
00403664   .  8B45 E0       mov     eax, dword ptr [ebp-20]          ;  再取固定字符串
00403667   .  8D95 54FFFFFF lea     edx, dword ptr [ebp-AC]
0040366D   .  8985 6CFFFFFF mov     dword ptr [ebp-94], eax
00403673   .  52            push    edx                              ; /长度为1
00403674   .  8D85 64FFFFFF lea     eax, dword ptr [ebp-9C]          ; |
0040367A   .  6A 09         push    9                                ; |Start = 9
0040367C   .  8D8D 44FFFFFF lea     ecx, dword ptr [ebp-BC]          ; |
00403682   .  50            push    eax                              ; |dString8
00403683   .  51            push    ecx                              ; |RetBUFFER
00403684   .  C785 5CFFFFFF>mov     dword ptr [ebp-A4], 1            ; |
0040368E   .  899D 54FFFFFF mov     dword ptr [ebp-AC], ebx          ; |
00403694   .  8975 E0       mov     dword ptr [ebp-20], esi          ; |
00403697   .  C785 64FFFFFF>mov     dword ptr [ebp-9C], 8            ; |
004036A1   .  FFD7          call    edi                              ; \取固定字符串第0x09个字符‘k’保存到ebp-B4中
004036A3   .  8B45 DC       mov     eax, dword ptr [ebp-24]
004036A6   .  8D95 14FFFFFF lea     edx, dword ptr [ebp-EC]
004036AC   .  8985 2CFFFFFF mov     dword ptr [ebp-D4], eax
004036B2   .  52            push    edx                              ; /长度为1
004036B3   .  8D85 24FFFFFF lea     eax, dword ptr [ebp-DC]          ; |
004036B9   .  68 8F000000   push    8F                               ; |Start = 8F
004036BE   .  8D8D 04FFFFFF lea     ecx, dword ptr [ebp-FC]          ; |
004036C4   .  50            push    eax                              ; |再取固定字符串
004036C5   .  51            push    ecx                              ; |RetBUFFER
004036C6   .  C785 1CFFFFFF>mov     dword ptr [ebp-E4], 1            ; |
004036D0   .  899D 14FFFFFF mov     dword ptr [ebp-EC], ebx          ; |
004036D6   .  8975 DC       mov     dword ptr [ebp-24], esi          ; |
004036D9   .  C785 24FFFFFF>mov     dword ptr [ebp-DC], 8            ; |
004036E3   .  FFD7          call    edi                              ; \取固定字符串第0x8F个字符‘h’保存到ebp-F4
004036E5   .  8B45 D8       mov     eax, dword ptr [ebp-28]          ;  再取固定字符串
004036E8   .  8D95 D4FEFFFF lea     edx, dword ptr [ebp-12C]
004036EE   .  8985 ECFEFFFF mov     dword ptr [ebp-114], eax
004036F4   .  52            push    edx                              ; /长度为1
004036F5   .  8D85 E4FEFFFF lea     eax, dword ptr [ebp-11C]         ; |
004036FB   .  6A 10         push    10                               ; |Start = 10
004036FD   .  8D8D C4FEFFFF lea     ecx, dword ptr [ebp-13C]         ; |
00403703   .  50            push    eax                              ; |dString8
00403704   .  51            push    ecx                              ; |RetBUFFER
00403705   .  C785 DCFEFFFF>mov     dword ptr [ebp-124], 1           ; |
0040370F   .  899D D4FEFFFF mov     dword ptr [ebp-12C], ebx         ; |
00403715   .  8975 D8       mov     dword ptr [ebp-28], esi          ; |
00403718   .  C785 E4FEFFFF>mov     dword ptr [ebp-11C], 8           ; |
00403722   .  FFD7          call    edi                              ; \取固定字符串第0x10个字符‘1’保存到ebp-134
00403724   .  8B45 D4       mov     eax, dword ptr [ebp-2C]          ;  再取固定字符串
00403727   .  8D95 94FEFFFF lea     edx, dword ptr [ebp-16C]
0040372D   .  8985 ACFEFFFF mov     dword ptr [ebp-154], eax
00403733   .  52            push    edx                              ; /Length8
00403734   .  8D85 A4FEFFFF lea     eax, dword ptr [ebp-15C]         ; |
0040373A   .  68 A1000000   push    0A1                              ; |Start = A1
0040373F   .  8D8D 84FEFFFF lea     ecx, dword ptr [ebp-17C]         ; |
00403745   .  50            push    eax                              ; |dString8
00403746   .  51            push    ecx                              ; |RetBUFFER
00403747   .  C785 9CFEFFFF>mov     dword ptr [ebp-164], 1           ; |
00403751   .  899D 94FEFFFF mov     dword ptr [ebp-16C], ebx         ; |
00403757   .  8975 D4       mov     dword ptr [ebp-2C], esi          ; |
0040375A   .  C785 A4FEFFFF>mov     dword ptr [ebp-15C], 8           ; |
00403764   .  FFD7          call    edi                              ; \取固定字符串第0xA1个字符‘o’保存到ebp-174
00403766   .  8B45 D0       mov     eax, dword ptr [ebp-30]
00403769   .  C785 5CFEFFFF>mov     dword ptr [ebp-1A4], 1
00403773   .  899D 54FEFFFF mov     dword ptr [ebp-1AC], ebx
00403779   .  8975 D0       mov     dword ptr [ebp-30], esi
0040377C   .  8985 6CFEFFFF mov     dword ptr [ebp-194], eax
00403782   .  8D95 54FEFFFF lea     edx, dword ptr [ebp-1AC]
00403788   .  8D85 64FEFFFF lea     eax, dword ptr [ebp-19C]
0040378E   .  52            push    edx                              ; /长度为1
0040378F   .  68 AB000000   push    0AB                              ; |Start = AB
00403794   .  8D8D 44FEFFFF lea     ecx, dword ptr [ebp-1BC]         ; |
0040379A   .  50            push    eax                              ; |dString8
0040379B   .  51            push    ecx                              ; |RetBUFFER
0040379C   .  C785 64FEFFFF>mov     dword ptr [ebp-19C], 8           ; |
004037A6   .  FFD7          call    edi                              ; \取固定字符串第0xAB个字符‘y’保存到ebp-1B4
004037A8   .  8B45 CC       mov     eax, dword ptr [ebp-34]
004037AB   .  8D95 14FEFFFF lea     edx, dword ptr [ebp-1EC]
004037B1   .  8985 2CFEFFFF mov     dword ptr [ebp-1D4], eax
004037B7   .  52            push    edx                              ; /长度为1
004037B8   .  8D85 24FEFFFF lea     eax, dword ptr [ebp-1DC]         ; |
004037BE   .  68 A6000000   push    0A6                              ; |Start = A6
004037C3   .  8D8D 04FEFFFF lea     ecx, dword ptr [ebp-1FC]         ; |
004037C9   .  50            push    eax                              ; |dString8
004037CA   .  51            push    ecx                              ; |RetBUFFER
004037CB   .  C785 1CFEFFFF>mov     dword ptr [ebp-1E4], 1           ; |
004037D5   .  899D 14FEFFFF mov     dword ptr [ebp-1EC], ebx         ; |
004037DB   .  8975 CC       mov     dword ptr [ebp-34], esi          ; |
004037DE   .  C785 24FEFFFF>mov     dword ptr [ebp-1DC], 8           ; |
004037E8   .  FFD7          call    edi                              ; \取固定字符串第0xA6个字符‘i’保存到ebp-1F4
004037EA   .  8B45 C8       mov     eax, dword ptr [ebp-38]
004037ED   .  8D95 D4FDFFFF lea     edx, dword ptr [ebp-22C]
004037F3   .  8985 ECFDFFFF mov     dword ptr [ebp-214], eax
004037F9   .  52            push    edx                              ; /长度为1
004037FA   .  8D85 E4FDFFFF lea     eax, dword ptr [ebp-21C]         ; |
00403800   .  68 A8000000   push    0A8                              ; |Start = A8
00403805   .  8D8D C4FDFFFF lea     ecx, dword ptr [ebp-23C]         ; |
0040380B   .  50            push    eax                              ; |dString8
0040380C   .  51            push    ecx                              ; |RetBUFFER
0040380D   .  C785 DCFDFFFF>mov     dword ptr [ebp-224], 1           ; |
00403817   .  899D D4FDFFFF mov     dword ptr [ebp-22C], ebx         ; |
0040381D   .  8975 C8       mov     dword ptr [ebp-38], esi          ; |
00403820   .  C785 E4FDFFFF>mov     dword ptr [ebp-21C], 8           ; |
0040382A   .  FFD7          call    edi                              ; \取固定字符串第0xA8个字符‘e’保存到ebp-234
0040382C   .  8B3D C0614000 mov     edi, dword ptr [<&MSVBVM50.__vba>;  MSVBVM50.__vbaVarAdd
00403832   .  8D95 A4FDFFFF lea     edx, dword ptr [ebp-25C]
00403838   .  8D85 74FFFFFF lea     eax, dword ptr [ebp-8C]
0040383E   .  52            push    edx                              ; /输入的字符串地址
0040383F   .  8D8D 44FFFFFF lea     ecx, dword ptr [ebp-BC]          ; |
00403845   .  50            push    eax                              ; |/保存的第1个字符
00403846   .  8D95 34FFFFFF lea     edx, dword ptr [ebp-CC]          ; ||
0040384C   .  51            push    ecx                              ; ||保存的第2个字符
0040384D   .  52            push    edx                              ; ||加法结果保存到ebp-C4中
0040384E   .  FFD7          call    edi                              ; |\__vbaVarAdd
00403850   .  50            push    eax                              ; |/加法结果
00403851   .  8D85 04FFFFFF lea     eax, dword ptr [ebp-FC]          ; ||
00403857   .  8D8D F4FEFFFF lea     ecx, dword ptr [ebp-10C]         ; ||
0040385D   .  50            push    eax                              ; ||保存的第3个字符
0040385E   .  51            push    ecx                              ; ||加法结果保存到ebp-104中
0040385F   .  FFD7          call    edi                              ; |\__vbaVarAdd
00403861   .  50            push    eax                              ; |/加法结果
00403862   .  8D95 C4FEFFFF lea     edx, dword ptr [ebp-13C]         ; ||
00403868   .  8D85 B4FEFFFF lea     eax, dword ptr [ebp-14C]         ; ||
0040386E   .  52            push    edx                              ; ||保存的第4个字符
0040386F   .  50            push    eax                              ; ||加法结果保存到ebp-144中
00403870   .  FFD7          call    edi                              ; |\__vbaVarAdd
00403872   .  8D8D 84FEFFFF lea     ecx, dword ptr [ebp-17C]         ; |
00403878   .  50            push    eax                              ; |/加法结果
00403879   .  8D95 74FEFFFF lea     edx, dword ptr [ebp-18C]         ; ||
0040387F   .  51            push    ecx                              ; ||保存的第5个字符
00403880   .  52            push    edx                              ; ||加法结果保存到ebp-184中
00403881   .  FFD7          call    edi                              ; |\__vbaVarAdd
00403883   .  50            push    eax                              ; |/加法结果
00403884   .  8D85 44FEFFFF lea     eax, dword ptr [ebp-1BC]         ; ||
0040388A   .  8D8D 34FEFFFF lea     ecx, dword ptr [ebp-1CC]         ; ||
00403890   .  50            push    eax                              ; ||保存的第6个字符
00403891   .  51            push    ecx                              ; ||加法结果保存到ebp-1C4中
00403892   .  FFD7          call    edi                              ; |\__vbaVarAdd
00403894   .  50            push    eax                              ; |/加法结果
00403895   .  8D95 04FEFFFF lea     edx, dword ptr [ebp-1FC]         ; ||
0040389B   .  8D85 F4FDFFFF lea     eax, dword ptr [ebp-20C]         ; ||
004038A1   .  52            push    edx                              ; ||保存的第7个字符
004038A2   .  50            push    eax                              ; ||加法结果保存到ebp-204中
004038A3   .  FFD7          call    edi                              ; |\__vbaVarAdd
004038A5   .  8D8D C4FDFFFF lea     ecx, dword ptr [ebp-23C]         ; |
004038AB   .  50            push    eax                              ; |/加法结果
004038AC   .  51            push    ecx                              ; ||保存的第8个字符
004038AD   .  8D95 B4FDFFFF lea     edx, dword ptr [ebp-24C]         ; ||
004038B3   .  52            push    edx                              ; ||加法结果保存到ebp-244中
004038B4   .  FFD7          call    edi                              ; |\__vbaVarAdd
004038B6   .  50            push    eax                              ; |加法结果
004038B7   .  FF15 6C614000 call    dword ptr [<&MSVBVM50.__vbaVarTs>; \__vbaVarTstEq
004038BD   .  8BF8          mov     edi, eax
004039D6   .  66:3BFE       cmp     di, si
004039D9   .  74 22         je      short 004039FD
004039DB   .  8B45 08       mov     eax, dword ptr [ebp+8]
004039DE   .  8D95 84FDFFFF lea     edx, dword ptr [ebp-27C]
004039E4   .  C785 8CFDFFFF>mov     dword ptr [ebp-274], 1
004039EE   .  899D 84FDFFFF mov     dword ptr [ebp-27C], ebx
004039F4   .  8D48 34       lea     ecx, dword ptr [eax+34]
004039F7   .  FF15 10614000 call    dword ptr [<&MSVBVM50.__vbaVarMo>;  要验证的地址的值赋值为1，只有相等才会走这里
00403B72   .  C2 0400       retn    4 
```

这里的加法其实就是把字符连接起来和输入字符串比较。

最后验证码为 **rkh1oyie** 。

### call dword ptr [eax+A0] 这个函数

```
7403EB7B    8901            mov     dword ptr [ecx], eax 
```

这一行是关键赋值。