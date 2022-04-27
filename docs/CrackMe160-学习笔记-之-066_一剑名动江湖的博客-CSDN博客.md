<!--yml
category: crackme160
date: 2022-04-27 18:15:45
-->

# CrackMe160 学习笔记 之 066_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/106032791](https://blog.csdn.net/guaigle001/article/details/106032791)

## 分析

```
004013B1  |.  6A 14         push    14                               ; /Count = 14 (20.)
004013B3  |.  8D45 EB       lea     eax, dword ptr [ebp-15]          ; |
004013B6  |.  50            push    eax                              ; |Buffer
004013B7  |.  6A 65         push    65                               ; |ControlID = 65 (101.)
004013B9  |.  53            push    ebx                              ; |hWnd
004013BA  |.  E8 1D020000   call    <jmp.&USER32.GetDlgItemTextA>    ; \GetDlgItemTextA
004013BF  |.  8945 D0       mov     dword ptr [ebp-30], eax          ;  获取输入
004013C2  |.  89C6          mov     esi, eax
004013C4  |.  EB 06         jmp     short 004013CC
004013C6  |>  C64435 EB 20  /mov     byte ptr [ebp+esi-15], 20
004013CB  |.  46            |inc     esi
004013CC  |>  83FE 14        cmp     esi, 14
004013CF  |.^ 7C F5         \jl      short 004013C6                  ;  填充输入字符串
004013D1  |.  31F6          xor     esi, esi
004013D3  |>  0FB67C35 EB   /movzx   edi, byte ptr [ebp+esi-15]
004013D8  |.  0FB65435 F5   |movzx   edx, byte ptr [ebp+esi-B]
004013DD  |.  89F8          |mov     eax, edi
004013DF  |.  31D0          |xor     eax, edx
004013E1  |.  B9 0A000000   |mov     ecx, 0A
004013E6  |.  99            |cdq
004013E7  |.  F7F9          |idiv    ecx
004013E9  |.  83C2 30       |add     edx, 30
004013EC  |.  885435 D6     |mov     byte ptr [ebp+esi-2A], dl
004013F0  |.  46            |inc     esi
004013F1  |.  39CE          |cmp     esi, ecx
004013F3  |.^ 7C DE         \jl      short 004013D3                  ;  计算生成新的字符串 
```

## 注册机代码

```
#include<stdio.h>
#include<string.h>
int main(int argc,char** argv)
{

  char s[0x14];
  if(argc!=2) return 0;
  int len=strlen(argv[1]);
  sprintf(s,"%s",argv[1]);
  printf("key:");
  for(int i=len;i<0x14;i++)
     s[i]=0x20;
  for(int i=0;i<0xA;i++)
    printf("%d",(s[i]^s[i+0xA])%10);
  return 0;
} 
```