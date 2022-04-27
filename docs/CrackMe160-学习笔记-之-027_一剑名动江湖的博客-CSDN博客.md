<!--yml
category: crackme160
date: 2022-04-27 18:16:42
-->

# CrackMe160 学习笔记 之 027_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/104268510](https://blog.csdn.net/guaigle001/article/details/104268510)

## 前言

这个直接爆破了，没什么好讲的。

水博客真开心。
![在这里插入图片描述](img/bce984963df8ed6a629291f28d21fc3d.png)

## 思路

看指令是作者遍历了各个目录，从D盘开始。

CeateFileA的返回值和-1比较，不想等则成功。

把这行语句改成jmp就行了。

```
0040138C   .  0F84 F3000000 je      00401485                         ;  关键跳 
```

因为我这是虚拟机，所以直接爆破了。

## 分析

```
00401207   .  B8 171B4000   mov     eax, 00401B17
0040120C   .  E8 DF040000   call    <jmp.&MSVCRT._EH_prolog>
00401211   .  83EC 50       sub     esp, 50
00401214   .  53            push    ebx
00401215   .  56            push    esi
00401216   .  894D E4       mov     dword ptr [ebp-1C], ecx
00401219   .  57            push    edi
0040121A   .  68 9C304000   push    0040309C                         ;  ASCII "C:\"
0040121F   .  8D4D A4       lea     ecx, dword ptr [ebp-5C]
00401222   .  E8 79040000   call    <jmp.&MFC42.#537_CString::CStrin>
00401227   .  33DB          xor     ebx, ebx
00401229   .  68 98304000   push    00403098                         ;  ASCII "D:\"
0040122E   .  8D4D A8       lea     ecx, dword ptr [ebp-58]
00401231   .  895D FC       mov     dword ptr [ebp-4], ebx
00401234   .  E8 67040000   call    <jmp.&MFC42.#537_CString::CStrin>
00401239   .  68 94304000   push    00403094                         ;  ASCII "E:\"
0040123E   .  8D4D AC       lea     ecx, dword ptr [ebp-54]
00401241   .  C645 FC 01    mov     byte ptr [ebp-4], 1
00401245   .  E8 56040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040124A   .  68 90304000   push    00403090                         ;  ASCII "F:\"
0040124F   .  8D4D B0       lea     ecx, dword ptr [ebp-50]
00401252   .  C645 FC 02    mov     byte ptr [ebp-4], 2
00401256   .  E8 45040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040125B   .  68 8C304000   push    0040308C                         ;  ASCII "G:\"
00401260   .  8D4D B4       lea     ecx, dword ptr [ebp-4C]
00401263   .  C645 FC 03    mov     byte ptr [ebp-4], 3
00401267   .  E8 34040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040126C   .  68 88304000   push    00403088                         ;  ASCII "H:\"
00401271   .  8D4D B8       lea     ecx, dword ptr [ebp-48]
00401274   .  C645 FC 04    mov     byte ptr [ebp-4], 4
00401278   .  E8 23040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040127D   .  68 84304000   push    00403084                         ;  ASCII "I:\"
00401282   .  8D4D BC       lea     ecx, dword ptr [ebp-44]
00401285   .  C645 FC 05    mov     byte ptr [ebp-4], 5
00401289   .  E8 12040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040128E   .  68 80304000   push    00403080                         ;  ASCII "J:\"
00401293   .  8D4D C0       lea     ecx, dword ptr [ebp-40]
00401296   .  C645 FC 06    mov     byte ptr [ebp-4], 6
0040129A   .  E8 01040000   call    <jmp.&MFC42.#537_CString::CStrin>
0040129F   .  68 7C304000   push    0040307C                         ;  ASCII "K:\"
004012A4   .  8D4D C4       lea     ecx, dword ptr [ebp-3C]
004012A7   .  C645 FC 07    mov     byte ptr [ebp-4], 7
004012AB   .  E8 F0030000   call    <jmp.&MFC42.#537_CString::CStrin>
004012B0   .  68 78304000   push    00403078                         ;  ASCII "L:\"
004012B5   .  8D4D C8       lea     ecx, dword ptr [ebp-38]
004012B8   .  C645 FC 08    mov     byte ptr [ebp-4], 8
004012BC   .  E8 DF030000   call    <jmp.&MFC42.#537_CString::CStrin>
004012C1   .  68 74304000   push    00403074                         ;  ASCII "M:\"
004012C6   .  8D4D CC       lea     ecx, dword ptr [ebp-34]
004012C9   .  C645 FC 09    mov     byte ptr [ebp-4], 9
004012CD   .  E8 CE030000   call    <jmp.&MFC42.#537_CString::CStrin>
004012D2   .  68 70304000   push    00403070                         ;  ASCII "N:\"
004012D7   .  8D4D D0       lea     ecx, dword ptr [ebp-30]
004012DA   .  C645 FC 0A    mov     byte ptr [ebp-4], 0A
004012DE   .  E8 BD030000   call    <jmp.&MFC42.#537_CString::CStrin>
004012E3   .  68 6C304000   push    0040306C                         ;  ASCII "O:\"
004012E8   .  8D4D D4       lea     ecx, dword ptr [ebp-2C]
004012EB   .  C645 FC 0B    mov     byte ptr [ebp-4], 0B
004012EF   .  E8 AC030000   call    <jmp.&MFC42.#537_CString::CStrin>
004012F4   .  68 68304000   push    00403068                         ;  ASCII "P:\"
004012F9   .  8D4D D8       lea     ecx, dword ptr [ebp-28]
004012FC   .  C645 FC 0C    mov     byte ptr [ebp-4], 0C
00401300   .  E8 9B030000   call    <jmp.&MFC42.#537_CString::CStrin>
00401305   .  BE 9A164000   mov     esi, <jmp.&MFC42.#800_CString::~>;  入口地址
0040130A   .  33C0          xor     eax, eax
0040130C   .  8D7D DC       lea     edi, dword ptr [ebp-24]
0040130F   .  56            push    esi
00401310   .  C645 FC 0D    mov     byte ptr [ebp-4], 0D
00401314   .  68 94164000   push    <jmp.&MFC42.#540_CString::CStrin>;  入口地址
00401319   .  AB            stos    dword ptr es:[edi]
0040131A   .  6A 01         push    1
0040131C   .  8D45 DC       lea     eax, dword ptr [ebp-24]
0040131F   .  6A 04         push    4
00401321   .  50            push    eax
00401322   .  E8 C3040000   call    004017EA
00401327   .  8D4D E8       lea     ecx, dword ptr [ebp-18]
0040132A   .  C645 FC 0E    mov     byte ptr [ebp-4], 0E
0040132E   .  E8 61030000   call    <jmp.&MFC42.#540_CString::CStrin>
00401333   .  C645 FC 0F    mov     byte ptr [ebp-4], 0F
00401337   .  895D EC       mov     dword ptr [ebp-14], ebx
0040133A   .  8D7D A4       lea     edi, dword ptr [ebp-5C]
0040133D   >  57            push    edi
0040133E   .  8D4D E8       lea     ecx, dword ptr [ebp-18]
00401341   .  E8 48030000   call    <jmp.&MFC42.#858_CString::operat>
00401346   .  FF75 E8       push    dword ptr [ebp-18]               ; /RootPathName
00401349   .  FF15 04204000 call    dword ptr [<&KERNEL32.GetDriveTy>; \GetDriveTypeA
0040134F   .  83F8 03       cmp     eax, 3
00401352   .  74 3E         je      short 00401392
00401354   .  8D45 E8       lea     eax, dword ptr [ebp-18]
00401357   .  68 58304000   push    00403058                         ;  ASCII "CD_CHECK.DAT"
0040135C   .  50            push    eax
0040135D   .  8D45 E0       lea     eax, dword ptr [ebp-20]
00401360   .  50            push    eax
00401361   .  E8 22030000   call    <jmp.&MFC42.#924_operator+>
00401366   .  8B00          mov     eax, dword ptr [eax]
00401368   .  53            push    ebx                              ; /hTemplateFile
00401369   .  53            push    ebx                              ; |Attributes
0040136A   .  53            push    ebx                              ; |Mode
0040136B   .  53            push    ebx                              ; |pSecurity
0040136C   .  6A 01         push    1                                ; |ShareMode = FILE_SHARE_READ
0040136E   .  68 00000080   push    80000000                         ; |Access = GENERIC_READ
00401373   .  50            push    eax                              ; |FileName
00401374   .  FF15 00204000 call    dword ptr [<&KERNEL32.CreateFile>; \CreateFileA
0040137A   .  83F8 FF       cmp     eax, -1                          ;  返回值和-1比较
0040137D   .  8D4D E0       lea     ecx, dword ptr [ebp-20]
00401380   .  0F9445 F3     sete    byte ptr [ebp-D]                 ;  相等时设为1.失败
00401384   .  E8 11030000   call    <jmp.&MFC42.#800_CString::~CStri>
00401389   .  385D F3       cmp     byte ptr [ebp-D], bl
0040138C   .  0F84 F3000000 je      00401485                         ;  关键跳 
```