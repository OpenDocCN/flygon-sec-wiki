<!--yml
category: crackme160
date: 2022-04-27 18:17:27
-->

# CrackMe160 学习笔记 之 006_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/104092196](https://blog.csdn.net/guaigle001/article/details/104092196)

## 前言

根据帮助按钮的提示，这个程序需要我们消除两个按钮“OK”和“Cancella”，显现出被按钮遮住的部分。

每个按钮都需要不同的算法来验证，所以有两个验证函数。

攻破后的程序如图。
![在这里插入图片描述](img/a531f6b3052ac9c0b6c70819f59b3b57.png)

## 思路

找到验证函数。

对Delphi写的程序不熟，但是我们知道帮助弹窗的位置。

猜测验证函数在其附近。

在帮助弹窗上个函数位置处下断点，果不其然，在**0x442EA8**处断下，由此可知是第一个验证函数。

绕过验证后继续下断点，用同样的方法找到第二个验证函数。

## 分析

### 验证函数1

```
00442EA8  /.  55            push    ebp                              ;  第一个验证函数
00442EA9  |.  8BEC          mov     ebp, esp
00442EC7  |.  E8 F403FEFF   call    004232C0                         ;  获取codice
00442ECC  |.  8B45 FC       mov     eax, dword ptr [ebp-4]
00442ECF  |.  E8 9C47FCFF   call    00407670                         ;  字符串转十进制数
00442ED4  |.  50            push    eax                              ;  压入转换后的数
00442ED5  |.  8D55 FC       lea     edx, dword ptr [ebp-4]
00442ED8  |.  8B83 DC020000 mov     eax, dword ptr [ebx+2DC]
00442EDE  |.  E8 DD03FEFF   call    004232C0                         ;  获取name
00442EE3  |.  8B45 FC       mov     eax, dword ptr [ebp-4]
00442EE6  |.  5A            pop     edx                              ;  弹出转换后的数
00442EE7  |.  E8 08FCFFFF   call    00442AF4                         ;  关键判断函数
00442EEC  |.  84C0          test    al, al                           ;  关键跳
00442EEE  |.  74 1C         je      short 00442F0C
00442EF0  |.  33D2          xor     edx, edx
00442EF2  |.  8B83 D0020000 mov     eax, dword ptr [ebx+2D0]
00442EF8  |.  E8 B302FEFF   call    004231B0
00442EFD  |.  B2 01         mov     dl, 1
00442EFF  |.  8B83 CC020000 mov     eax, dword ptr [ebx+2CC]
00442F05  |.  8B08          mov     ecx, dword ptr [eax]
00442F07  |.  FF51 60       call    dword ptr [ecx+60]
00442F0A  |.  EB 10         jmp     short 00442F1C
00442F1C  |>  33C0          xor     eax, eax
00442F1E  |.  5A            pop     edx
00442F1F  |.  59            pop     ecx
00442F20  |.  59            pop     ecx
00442F21  |.  64:8910       mov     dword ptr fs:[eax], edx
00442F24  |.  68 392F4400   push    00442F39
00442F29  |>  8D45 FC       lea     eax, dword ptr [ebp-4]
00442F2C  |.  E8 8708FCFF   call    004037B8
00442F31  \.  C3            retn 
```

此处为第一个验证函数。

### 字符串转十进制数

```
00407670  /$  55            push    ebp
00407671  |.  8BEC          mov     ebp, esp
00407673  |.  83C4 F0       add     esp, -10
00407692  |.  E8 C1B2FFFF   call    00402958                         ;  字符串转十进制数
00407697  |.  8BF0          mov     esi, eax
00407699  |.  837D FC 00    cmp     dword ptr [ebp-4], 0
0040769D  |.  74 23         je      short 004076C2                   ;  必跳转
004076C2  |>  33C0          xor     eax, eax
004076C4  |.  5A            pop     edx
004076C5  |.  59            pop     ecx
004076C6  |.  59            pop     ecx
004076C7  |.  64:8910       mov     dword ptr fs:[eax], edx
004076CA  |.  68 DF764000   push    004076DF
004076CF  |>  8D45 F8       lea     eax, dword ptr [ebp-8]
004076D2  |.  E8 E1C0FFFF   call    004037B8                         ;  不做任何事
004076D7  \.  C3            retn 
```

进入字符串转十进制数的函数**0x00407670**。

### 字符串转十进制数的算法

```
00402958  /$  53            push    ebx
00402959  |.  56            push    esi
0040295A  |.  57            push    edi
0040295B  |.  89C6          mov     esi, eax
0040295D  |.  50            push    eax
0040295E  |.  85C0          test    eax, eax
00402960  |.  74 73         je      short 004029D5
00402962  |.  31C0          xor     eax, eax
00402964  |.  31DB          xor     ebx, ebx
00402966  |.  BF CCCCCC0C   mov     edi, 0CCCCCCC                    ;  edi=0x0ccccccc
0040296B  |>  8A1E          /mov     bl, byte ptr [esi]
0040296D  |.  46            |inc     esi                             ;  下一个字符
0040296E  |.  80FB 20       |cmp     bl, 20
00402971  |.^ 74 F8         \je      short 0040296B                  ;  如果是空格则跳过
00402973  |.  B5 00         mov     ch, 0
00402975  |.  80FB 2D       cmp     bl, 2D                           ;  -; Switch (cases 24..78)
00402978  |.  74 69         je      short 004029E3
0040297A  |.  80FB 2B       cmp     bl, 2B                           ;  +
0040297D  |.  74 66         je      short 004029E5
0040297F  |.  80FB 24       cmp     bl, 24                           ;  $
00402982  |.  74 66         je      short 004029EA
00402984  |.  80FB 78       cmp     bl, 78                           ;  x
00402987  |.  74 61         je      short 004029EA
00402989  |.  80FB 58       cmp     bl, 58                           ;  X
0040298C  |.  74 5C         je      short 004029EA
0040298E  |.  80FB 30       cmp     bl, 30                           ;  0
00402991  |.  75 13         jnz     short 004029A6
00402993  |.  8A1E          mov     bl, byte ptr [esi]               ;  Case 30 ('0') of switch 00402975
00402995  |.  46            inc     esi                              ;  下一个字符
00402996  |.  80FB 78       cmp     bl, 78
00402999  |.  74 4F         je      short 004029EA
0040299B  |.  80FB 58       cmp     bl, 58
0040299E  |.  74 4A         je      short 004029EA
004029A0  |.  84DB          test    bl, bl
004029A2  |.  74 20         je      short 004029C4
004029A4  |.  EB 04         jmp     short 004029AA
004029A6  |>  84DB          test    bl, bl                           ;  Default case of switch 00402975
004029A8  |.  74 34         je      short 004029DE                   ;  下面这个循环的功能是把字符串转成十进制数
004029AA  |>  80EB 30       /sub     bl, 30                          ;  从ACSII码转回数字
004029AD  |.  80FB 09       |cmp     bl, 9                           ;  和数字9作比较
004029B0  |.  77 2C         |ja      short 004029DE
004029B2  |.  39F8          |cmp     eax, edi
004029B4  |.  77 28         |ja      short 004029DE
004029B6  |.  8D0480        |lea     eax, dword ptr [eax+eax*4]      ;  eax *= 5
004029B9  |.  01C0          |add     eax, eax                        ;  eax *= 2
004029BB  |.  01D8          |add     eax, ebx
004029BD  |.  8A1E          |mov     bl, byte ptr [esi]
004029BF  |.  46            |inc     esi                             ;  下一个字符
004029C0  |.  84DB          |test    bl, bl                          ;  字符串是否到达结尾
004029C2  |.^ 75 E6         \jnz     short 004029AA
004029C4  |>  FECD          dec     ch
004029C6  |.  74 10         je      short 004029D8                   ;  为负数时跳转
004029C8  |.  85C0          test    eax, eax
004029CA  |.  7C 12         jl      short 004029DE
004029CC  |>  59            pop     ecx
004029CD  |.  31F6          xor     esi, esi
004029CF  |>  8932          mov     dword ptr [edx], esi
004029D1  |.  5F            pop     edi
004029D2  |.  5E            pop     esi
004029D3  |.  5B            pop     ebx
004029D4  |.  C3            retn
004029D5  |>  46            inc     esi
004029D6  |.  EB 06         jmp     short 004029DE
004029D8  |>  F7D8          neg     eax                              ;  负数取补码
004029DA  |.^ 7E F0         jle     short 004029CC
004029DC  |.^ 78 EE         js      short 004029CC
004029DE  |>  5B            pop     ebx                              ;  Default case of switch 004029FE
004029DF  |.  29DE          sub     esi, ebx
004029E1  |.^ EB EC         jmp     short 004029CF
004029E3  |>  FEC5          inc     ch                               ;  Case 2D ('-') of switch 00402975
004029E5  |>  8A1E          mov     bl, byte ptr [esi]               ;  Case 2B ('+') of switch 00402975
004029E7  |.  46            inc     esi
004029E8  |.^ EB BC         jmp     short 004029A6
004029EA  |>  BF FFFFFF0F   mov     edi, 0FFFFFFF                    ;  Cases 24 ('$'),58 ('X'),78 ('x') of switch 00402975
004029EF  |.  8A1E          mov     bl, byte ptr [esi]
004029F1  |.  46            inc     esi
004029F2  |.  84DB          test    bl, bl
004029F4  |.^ 74 DF         je      short 004029D5
004029F6  |>  80FB 61       /cmp     bl, 61                          ;  'a'
004029F9  |.  72 03         |jb      short 004029FE
004029FB  |.  80EB 20       |sub     bl, 20
004029FE  |>  80EB 30       |sub     bl, 30                          ;  Switch (cases 30..46)
00402A01  |.  80FB 09       |cmp     bl, 9
00402A04  |.  76 0B         |jbe     short 00402A11
00402A06  |.  80EB 11       |sub     bl, 11
00402A09  |.  80FB 05       |cmp     bl, 5
00402A0C  |.^ 77 D0         |ja      short 004029DE
00402A0E  |.  80C3 0A       |add     bl, 0A                          ;  Cases 41 ('A'),42 ('B'),43 ('C'),44 ('D'),45 ('E'),46 ('F') of switch 004029FE
00402A11  |>  39F8          |cmp     eax, edi                        ;  Cases 30 ('0'),31 ('1'),32 ('2'),33 ('3'),34 ('4'),35 ('5'),36 ('6'),37 ('7'),38 ('8'),39 ('9') of switch 004029FE
00402A13  |.^ 77 C9         |ja      short 004029DE
00402A15  |.  C1E0 04       |shl     eax, 4
00402A18  |.  01D8          |add     eax, ebx
00402A1A  |.  8A1E          |mov     bl, byte ptr [esi]
00402A1C  |.  46            |inc     esi
00402A1D  |.  84DB          |test    bl, bl
00402A1F  |.^ 75 D5         \jnz     short 004029F6
00402A21  \.^ EB A9         jmp     short 004029CC
00402A23   .  C3            retn 
```

继续跟进。

这个跳转有点多，其实就是把字符串中 “+” “x” “X” “¥” 这些特殊字符过滤掉，有可能为负数。

剩下来的数字组合成十进制数。

### 第一个验证函数算法

```
00442AF4  /$  55            push    ebp
00442AF5  |.  8BEC          mov     ebp, esp
00442AF7  |.  83C4 F8       add     esp, -8
00442AFA  |.  53            push    ebx
00442AFB  |.  56            push    esi
00442AFC  |.  8955 F8       mov     dword ptr [ebp-8], edx           ;  codice转回后的十进制数
00442AFF  |.  8945 FC       mov     dword ptr [ebp-4], eax           ;  存放name字符串地址
00442B02  |.  8B45 FC       mov     eax, dword ptr [ebp-4]           ;  无用的指令
00442B05  |.  E8 DE10FCFF   call    00403BE8                         ;  edx=2
00442B0A  |.  33C0          xor     eax, eax
00442B0C  |.  55            push    ebp
00442B0D  |.  68 902B4400   push    00442B90
00442B12  |.  64:FF30       push    dword ptr fs:[eax]
00442B15  |.  64:8920       mov     dword ptr fs:[eax], esp
00442B18  |.  8B45 FC       mov     eax, dword ptr [ebp-4]           ;  存放name字符串地址
00442B1B  |.  E8 140FFCFF   call    00403A34                         ;  获取name长度并返回
00442B20  |.  83F8 05       cmp     eax, 5
00442B23  |.  7E 53         jle     short 00442B78                   ;  小于等于5则跳转
00442B25  |.  8B45 FC       mov     eax, dword ptr [ebp-4]           ;  同上
00442B28  |.  0FB640 04     movzx   eax, byte ptr [eax+4]            ;  eax = name[4]
00442B2C  |.  B9 07000000   mov     ecx, 7                           ;  ecx = 7
00442B31  |.  33D2          xor     edx, edx                         ;  edx 清零
00442B33  |.  F7F1          div     ecx                              ;  eax = name[4]/ecx , edx 存放余数
00442B35  |.  8BC2          mov     eax, edx                         ;  eax = name[4]%7
00442B37  |.  83C0 02       add     eax, 2                           ;  eax += 2
00442B3A  |.  E8 E1FEFFFF   call    00442A20                         ;  1到eax循环相乘
00442B3F  |.  8BF0          mov     esi, eax                         ;  esi=1*2*3...*eax
00442B41  |.  33DB          xor     ebx, ebx
00442B43  |.  8B45 FC       mov     eax, dword ptr [ebp-4]           ;  同上
00442B46  |.  E8 E90EFCFF   call    00403A34                         ;  获取name长度
00442B4B  |.  85C0          test    eax, eax
00442B4D  |.  7E 16         jle     short 00442B65
00442B4F  |.  BA 01000000   mov     edx, 1                           ;  edx=1
00442B54  |>  8B4D FC       /mov     ecx, dword ptr [ebp-4]          ;  ecx存放name字符串地址
00442B57  |.  0FB64C11 FF   |movzx   ecx, byte ptr [ecx+edx-1]       ;  轮询取字符
00442B5C  |.  0FAFCE        |imul    ecx, esi                        ;  所有字符依次乘以计算得出的esi
00442B5F  |.  03D9          |add     ebx, ecx                        ;  求和保存在ebx中
00442B61  |.  42            |inc     edx                             ;  指向下一个字符
00442B62  |.  48            |dec     eax                             ;  计数器减一
00442B63  |.^ 75 EF         \jnz     short 00442B54
00442B65  |>  2B5D F8       sub     ebx, dword ptr [ebp-8]           ;  ebx减去转换后codice的十进制数
00442B68  |.  81FB 697A0000 cmp     ebx, 7A69
00442B6E  |.  75 04         jnz     short 00442B74                   ;  关键跳
00442B70  |.  B3 01         mov     bl, 1
00442B72  |.  EB 06         jmp     short 00442B7A
00442B74  |>  33DB          xor     ebx, ebx
00442B76  |.  EB 02         jmp     short 00442B7A
00442B78  |>  33DB          xor     ebx, ebx
00442B7A  |>  33C0          xor     eax, eax
00442B7C  |.  5A            pop     edx
00442B7D  |.  59            pop     ecx
00442B7E  |.  59            pop     ecx
00442B7F  |.  64:8910       mov     dword ptr fs:[eax], edx
00442B82  |.  68 972B4400   push    00442B97
00442B87  |>  8D45 FC       lea     eax, dword ptr [ebp-4]
00442B8A  |.  E8 290CFCFF   call    004037B8                         ;  eax=ebx
00442B8F  \.  C3            retn 
```

这个函数里面是关键计算过程。

name长度得大于5位，应该是为了保证第一个数大于**0x7A69**(31337), 这样减出来就不会是负数了。

### 循环相乘函数

```
00442A20  /$  53            push    ebx
00442A21  |.  8BD8          mov     ebx, eax                         ;  ebx = eax
00442A23  |.  85DB          test    ebx, ebx
00442A25  |.  75 07         jnz     short 00442A2E                   ;  eax 为0时退出循环
00442A27  |.  B8 01000000   mov     eax, 1
00442A2C  |.  5B            pop     ebx
00442A2D  |.  C3            retn
00442A2E  |>  8BC3          mov     eax, ebx                         ;  无意义指令
00442A30  |.  48            dec     eax                              ;  eax -= 1
00442A31  |.  E8 EAFFFFFF   call    00442A20
00442A36  |.  F7EB          imul    ebx
00442A38  |.  5B            pop     ebx
00442A39  \.  C3            retn 
```

这里用了递归来计算循环相乘。

### 第二个验证函数

```
00442D64  /.  55            push    ebp                              ;  第二个验证函数
00442D65  |.  8BEC          mov     ebp, esp
00442DA1  |.  E8 1A05FEFF   call    004232C0                         ;  获取codice
00442DA6  |.  8B45 FC       mov     eax, dword ptr [ebp-4]
00442DA9  |.  E8 C248FCFF   call    00407670                         ;  字符串转十进制数
00442DAE  |.  50            push    eax
00442DAF  |.  8D55 FC       lea     edx, dword ptr [ebp-4]
00442DB2  |.  8B83 DC020000 mov     eax, dword ptr [ebx+2DC]
00442DB8  |.  E8 0305FEFF   call    004232C0                         ;  获取name
00442DBD  |.  8B45 FC       mov     eax, dword ptr [ebp-4]
00442DC0  |.  5A            pop     edx
00442DC1  |.  E8 DAFDFFFF   call    00442BA0                         ;  判断函数
00442DC6  |.  84C0          test    al, al
00442DC8  |.  74 0D         je      short 00442DD7                   ;  关键跳
00442DD7  |>  33C0          xor     eax, eax
00442DD9  |.  5A            pop     edx
00442DDA  |.  59            pop     ecx
00442DDB  |.  59            pop     ecx
00442DDC  |.  64:8910       mov     dword ptr fs:[eax], edx
00442DDF  |.  68 F42D4400   push    00442DF4
00442DE4  |>  8D45 FC       lea     eax, dword ptr [ebp-4]
00442DE7  |.  E8 CC09FCFF   call    004037B8
00442DEC  \.  C3            retn 
```

继续分析第二个验证函数。

### 第二个验证函数算法

```
00442BA0  /$  55            push    ebp
00442BA1  |.  8BEC          mov     ebp, esp
00442BA3  |.  6A 00         push    0
00442BA5  |.  6A 00         push    0
00442BA7  |.  6A 00         push    0
00442BA9  |.  53            push    ebx
00442BAA  |.  56            push    esi
00442BAB  |.  8BF2          mov     esi, edx                         ;  十进制codice
00442BAD  |.  8945 FC       mov     dword ptr [ebp-4], eax           ;  name
00442BB0  |.  8B45 FC       mov     eax, dword ptr [ebp-4]           ;  无意义指令
00442BB3  |.  E8 3010FCFF   call    00403BE8                         ;  edx=2
00442BB8  |.  33C0          xor     eax, eax
00442BBA  |.  55            push    ebp
00442BBB  |.  68 672C4400   push    00442C67
00442BC0  |.  64:FF30       push    dword ptr fs:[eax]
00442BC3  |.  64:8920       mov     dword ptr fs:[eax], esp
00442BC6  |.  33DB          xor     ebx, ebx
00442BC8  |.  8D55 F8       lea     edx, dword ptr [ebp-8]
00442BCB  |.  8BC6          mov     eax, esi
00442BCD  |.  E8 6E4AFCFF   call    00407640                         ;  scanf ? printf?
00442BD2  |.  8D45 F4       lea     eax, dword ptr [ebp-C]
00442BD5  |.  8B55 F8       mov     edx, dword ptr [ebp-8]
00442BD8  |.  E8 730CFCFF   call    00403850                         ;  复制codice字符串
00442BDD  |.  8B45 F8       mov     eax, dword ptr [ebp-8]
00442BE0  |.  E8 4F0EFCFF   call    00403A34                         ;  返回name长度
00442BE5  |.  83F8 05       cmp     eax, 5
00442BE8  |.  7E 60         jle     short 00442C4A                   ;  codice长度必须大于5
00442BEA  |.  8B45 F8       mov     eax, dword ptr [ebp-8]           ;  codice
00442BED  |.  E8 420EFCFF   call    00403A34                         ;  返回codice长度
00442BF2  |.  8BF0          mov     esi, eax                         ;  esi 保存codice字符串长度
00442BF4  |.  83FE 01       cmp     esi, 1
00442BF7  |.  7C 2F         jl      short 00442C28
00442BF9  |>  8D45 F4       /lea     eax, dword ptr [ebp-C]          ;  获取新字符串的地址
00442BFC  |.  E8 0310FCFF   |call    00403C04                        ;  返回新的字符串地址
00442C01  |.  8D4430 FF     |lea     eax, dword ptr [eax+esi-1]      ;  从codice最后一个字符地址依次向前取
00442C05  |.  50            |push    eax
00442C06  |.  8B45 F8       |mov     eax, dword ptr [ebp-8]          ;  取codice原字符串地址
00442C09  |.  0FB64430 FF   |movzx   eax, byte ptr [eax+esi-1]       ;  从codice最后一个字符依次向前取
00442C0E  |.  F7E8          |imul    eax                             ;  字符的平方
00442C10  |.  0FBFC0        |movsx   eax, ax
00442C13  |.  F7EE          |imul    esi                             ;  eax = eax * esi
00442C15  |.  B9 19000000   |mov     ecx, 19                         ;  ecx = 0x19
00442C1A  |.  99            |cdq
00442C1B  |.  F7F9          |idiv    ecx                             ;  eax = eax / 0x19
00442C1D  |.  83C2 41       |add     edx, 41                         ;  edx = edx + 0x41
00442C20  |.  58            |pop     eax
00442C21  |.  8810          |mov     byte ptr [eax], dl              ;  计算出的新字符覆盖原来的字符
00442C23  |.  4E            |dec     esi                             ;  esi = esi - 1
00442C24  |.  85F6          |test    esi, esi
00442C26  |.^ 75 D1         \jnz     short 00442BF9
00442C28  |>  8B45 F4       mov     eax, dword ptr [ebp-C]           ;  codice字符串运算后的新字符串
00442C2B  |.  8B55 FC       mov     edx, dword ptr [ebp-4]           ;  name字符串
00442C2E  |.  E8 110FFCFF   call    00403B44                         ;  字符串比较
00442C33  |.  75 17         jnz     short 00442C4C                   ;  关键跳
00442C35  |.  8B45 FC       mov     eax, dword ptr [ebp-4]
00442C38  |.  8B55 F4       mov     edx, dword ptr [ebp-C]
00442C3B  |.  E8 040FFCFF   call    00403B44                         ;  字符串又比较了一次
00442C40  |.  75 04         jnz     short 00442C46
00442C42  |.  B3 01         mov     bl, 1
00442C44  |.  EB 06         jmp     short 00442C4C
00442C46  |>  33DB          xor     ebx, ebx
00442C48  |.  EB 02         jmp     short 00442C4C
00442C4A  |>  33DB          xor     ebx, ebx
00442C4C  |>  33C0          xor     eax, eax
00442C4E  |.  5A            pop     edx
00442C4F  |.  59            pop     ecx
00442C50  |.  59            pop     ecx
00442C51  |.  64:8910       mov     dword ptr fs:[eax], edx
00442C54  |.  68 6E2C4400   push    00442C6E
00442C59  |>  8D45 F4       lea     eax, dword ptr [ebp-C]
00442C5C  |.  BA 03000000   mov     edx, 3
00442C61  |.  E8 760BFCFF   call    004037DC
00442C66  \.  C3            retn 
```

这里是第二个验证函数关键的计算部分。

字符串比较函数比较两次是为了保证长度一致，这里就不详细展开了。

## 注册机代码

### 按钮1

```
#include<stdio.h>
int main()
{
  char* name;
  int esi=1,ebx=0;
  unsigned int len_string=0;
  printf("name:");
  scanf("%[^\n]",name);
  len_string=strlen(name);
  if(len_string<=5)
    return 0;
  int len=name[4]%7+2;
  for(int i=1 ; i<=len ;i++)
    {
      esi*=i;
    }
  for(int i=0;i< len_string;i++)
    {
      ebx+=name[i]*esi;
    }
  printf("esi:0x%x\nebx:0x%x\n",esi,ebx);
  printf("codice:%d\n",ebx-0x7A69);
  return 0;
} 
```

### 按钮2

```
#include<stdio.h>
int main()
{
  char* codice;
  printf("codice:");
  scanf("%[^\n]",codice);
  int len=strlen(codice);
  if(len<=5)
    return 0;
  for(int i=len-1;i>=0;i--)
    {
      codice[i]=codice[i]*codice[i]*(i+1)%0x19+0x41;
    }
  printf("name:%s",codice);
  return 0;
} 
```