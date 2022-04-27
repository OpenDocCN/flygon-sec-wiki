<!--yml
category: crackme160
date: 2022-04-27 18:15:29
-->

# CrackMe160 学习笔记 之 064_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/106027384](https://blog.csdn.net/guaigle001/article/details/106027384)

## 前言

这是个VB程序。界面还做的挺好看的。

## 思路

对输入字符串转大写计算生成新的字符串并比较验证。

## 分析

### 字符串转大写

```
0040D96F   .  FF15 3CF14000 call    dword ptr [<&MSVBVM50.#528>]     ;  MSVBVM50.rtcUpperCaseVar 
```

### 遍历并计算字符串

```
0040D9BC   > /66:3BB5 48FFF>cmp     si, word ptr [ebp-B8]
0040D9C3   . |0F8F 3A010000 jg      0040DB03                         ;  循环出口

0040D9D5   .  52            push    edx                              ; /Length8
0040D9D6   .  8D8D 7CFFFFFF lea     ecx, dword ptr [ebp-84]          ; |
0040D9DC   .  50            push    eax                              ; |Start
0040D9DD   .  8D55 AC       lea     edx, dword ptr [ebp-54]          ; |
0040D9E0   .  51            push    ecx                              ; |dString8
0040D9E1   .  52            push    edx                              ; |RetBUFFER
0040D9E2   .  C745 C4 01000>mov     dword ptr [ebp-3C], 1            ; |
0040D9E9   .  C745 BC 02000>mov     dword ptr [ebp-44], 2            ; |
0040D9F0   .  C785 7CFFFFFF>mov     dword ptr [ebp-84], 4008         ; |
0040D9FA   .  FF15 30F14000 call    dword ptr [<&MSVBVM50.#632>]     ; \rtcMidCharVar
0040DA00   .  8D45 AC       lea     eax, dword ptr [ebp-54]          ;  取当前字符

0040DA28   .  FF15 08F14000 call    dword ptr [<&MSVBVM50.#516>]     ; \rtcAnsiValueBstr
0040DA2E   .  66:2D 4000    sub     ax, 40
0040DA32   .  0F80 A1020000 jo      0040DCD9
0040DA38   .  66:69C0 8200  imul    ax, ax, 82
0040DA3D   .  0F80 96020000 jo      0040DCD9
0040DA43   .  66:03C7       add     ax, di
0040DA46   .  0F80 8D020000 jo      0040DCD9
0040DA4C   .  66:05 5000    add     ax, 50
0040DA50   .  0F80 83020000 jo      0040DCD9
0040DA56   .  66:05 5000    add     ax, 50
0040DA5A   .  0F80 79020000 jo      0040DCD9
0040DA60   .  66:05 5000    add     ax, 50
0040DA64   .  0F80 6F020000 jo      0040DCD9
0040DA6A   .  66:05 5000    add     ax, 50
0040DA6E   .  0F80 65020000 jo      0040DCD9
0040DA74   .  66:05 5000    add     ax, 50
0040DA78   .  0F80 5B020000 jo      0040DCD9
0040DA7E   .  66:05 5000    add     ax, 50
0040DA82   .  0F80 51020000 jo      0040DCD9
0040DA88   .  66:05 5000    add     ax, 50
0040DA8C   .  0F80 47020000 jo      0040DCD9
0040DA92   .  66:05 5000    add     ax, 50
0040DA96   .  0F80 3D020000 jo      0040DCD9
0040DA9C   .  66:05 5000    add     ax, 50
0040DAA0   .  0F80 33020000 jo      0040DCD9
0040DAA6   .  66:05 5000    add     ax, 50
0040DAAA   .  0F80 29020000 jo      0040DCD9
0040DAB0   .  66:05 5000    add     ax, 50
0040DAB4   .  0F80 1F020000 jo      0040DCD9
0040DABA   .  66:05 5000    add     ax, 50
0040DABE   .  0F80 15020000 jo      0040DCD9
0040DAC4   .  66:05 5000    add     ax, 50
0040DAC8   .  0F80 0B020000 jo      0040DCD9
0040DACE   .  66:05 5000    add     ax, 50
0040DAD2   .  0F80 01020000 jo      0040DCD9
0040DAD8   .  66:05 5000    add     ax, 50
0040DADC   .  0F80 F7010000 jo      0040DCD9
0040DAE2   .  66:05 5000    add     ax, 50
0040DAE6   .  0F80 ED010000 jo      0040DCD9
0040DAEC   .  8BF8          mov     edi, eax                         ;  edi 保存计算结果
0040DAEE   .  B8 01000000   mov     eax, 1
0040DAF3   .  66:03C6       add     ax, si
0040DAF6   .  0F80 DD010000 jo      0040DCD9
0040DAFC   .  8BF0          mov     esi, eax                         ;  esi 保存循环次数
0040DAFE   .^ E9 B9FEFFFF   jmp     0040D9BC 
```

### 验证

```
0040DB44   .  FF15 E8F04000 call    dword ptr [<&MSVBVM50.__vbaStrI2>;  MSVBVM50.__vbaStrI2
0040DB4A   .  8BD0          mov     edx, eax
0040DB4C   .  8D4D D0       lea     ecx, dword ptr [ebp-30]
0040DB4F   .  FFD3          call    ebx
0040DB51   .  50            push    eax
0040DB52   .  FF15 40F14000 call    dword ptr [<&MSVBVM50.__vbaStrCm>;  MSVBVM50.__vbaStrCmp
0040DB58   .  8BF0          mov     esi, eax
0040DB5A   .  8D45 D0       lea     eax, dword ptr [ebp-30]
0040DB5D   .  F7DE          neg     esi
0040DB5F   .  1BF6          sbb     esi, esi
0040DB9C   . /74 5E         je      short 0040DBFC                   ;  关键跳 
```

## 注册机代码

```
#include<stdio.h>
int main(int argc,char** argv)
{
  if(argc!=2) return 0;
  int len=strlen(argv[1]);
  short sum=0;
  for(int i=0;i<len;i++)
      sum+=(argv[1][i]&0xDF-0x40)*0x82+0x500;
  printf("key:%d",sum);
  return 0;
} 
```