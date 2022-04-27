<!--yml
category: crackme160
date: 2022-04-27 18:15:47
-->

# CrackMe160 学习笔记 之 062_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/106005881](https://blog.csdn.net/guaigle001/article/details/106005881)

## 前言

这是个VB程序。通过一个双重循环对用户名运算生成新的字符串来比较。

## 思路

偶数字符不变，单数转成ASCII码值再拆出来算。

## 分析

### 验证输入

```
00403CDA    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]                       ; 输入用户名地址
00403CE0    50              push    eax 
00403CE1    8D85 DCFEFFFF   lea     eax, dword ptr [ebp-124]
00403CE7    50              push    eax
00403CE8    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
00403CEE    50              push    eax
00403CEF    E8 42D6FFFF     call    <jmp.&msvbvm60.__vbaVarCmpEq>
00403CF4    50              push    eax
00403CF5    8D85 0CFFFFFF   lea     eax, dword ptr [ebp-F4]                       ; 输入密码地址
00403CFB    50              push    eax
00403CFC    8D85 CCFEFFFF   lea     eax, dword ptr [ebp-134]
00403D02    50              push    eax
00403D03    8D85 FCFEFFFF   lea     eax, dword ptr [ebp-104]
00403D09    50              push    eax
00403D0A    E8 27D6FFFF     call    <jmp.&msvbvm60.__vbaVarCmpEq>
00403D0F    50              push    eax
00403D10    8D85 ECFEFFFF   lea     eax, dword ptr [ebp-114]
00403D16    50              push    eax
00403D17    E8 20D6FFFF     call    <jmp.&msvbvm60.__vbaVarOr> 
```

对输入和0比较再通过异或保存结果来验证输入是否为空。

### 双重循环计算

```
00403EE0    E8 21D4FFFF     call    <jmp.&msvbvm60.__vbaLenVar>                   ; 计算用户名长度
00403EE5    8BD0            mov     edx, eax
00403EE7    8D8D 54FFFFFF   lea     ecx, dword ptr [ebp-AC]
00403EED    E8 1AD4FFFF     call    <jmp.&msvbvm60.__vbaVarMove>
00403EF2    C785 E4FEFFFF 0>mov     dword ptr [ebp-11C], 2
00403EFC    C785 DCFEFFFF 0>mov     dword ptr [ebp-124], 2
00403F06    C785 D4FEFFFF 0>mov     dword ptr [ebp-12C], 1
00403F10    C785 CCFEFFFF 0>mov     dword ptr [ebp-134], 2
00403F1A    8D85 DCFEFFFF   lea     eax, dword ptr [ebp-124]                      ; step:2
00403F20    50              push    eax
00403F21    8D85 54FFFFFF   lea     eax, dword ptr [ebp-AC]                       ; end:strlen(name)
00403F27    50              push    eax
00403F28    8D85 CCFEFFFF   lea     eax, dword ptr [ebp-134]                      ; start:1
00403F2E    50              push    eax
00403F2F    8D85 74FEFFFF   lea     eax, dword ptr [ebp-18C]
00403F35    50              push    eax
00403F36    8D85 84FEFFFF   lea     eax, dword ptr [ebp-17C]
00403F3C    50              push    eax
00403F3D    8D45 84         lea     eax, dword ptr [ebp-7C]
00403F40    50              push    eax
00403F41    E8 BAD3FFFF     call    <jmp.&msvbvm60.__vbaVarForInit>
00403F46    8985 3CFEFFFF   mov     dword ptr [ebp-1C4], eax
00403F4C    E9 D4020000     jmp     00404225
00403F51    C785 34FFFFFF 0>mov     dword ptr [ebp-CC], 1
00403F5B    C785 2CFFFFFF 0>mov     dword ptr [ebp-D4], 2
00403F65    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]
00403F6B    50              push    eax
00403F6C    8D45 84         lea     eax, dword ptr [ebp-7C]
00403F6F    50              push    eax
00403F70    E8 7FD3FFFF     call    <jmp.&msvbvm60.__vbaI4Var>
00403F75    50              push    eax
00403F76    8D45 98         lea     eax, dword ptr [ebp-68]                       ; 输入的用户名
00403F79    50              push    eax
00403F7A    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
00403F80    50              push    eax
00403F81    E8 74D3FFFF     call    <jmp.&msvbvm60.rtcMidCharVar>
00403F86    8D95 1CFFFFFF   lea     edx, dword ptr [ebp-E4]
00403F8C    8D8D 64FFFFFF   lea     ecx, dword ptr [ebp-9C]
00403F92    E8 75D3FFFF     call    <jmp.&msvbvm60.__vbaVarMove>
00403F97    8D8D 2CFFFFFF   lea     ecx, dword ptr [ebp-D4]
00403F9D    E8 4CD3FFFF     call    <jmp.&msvbvm60.__vbaFreeVar>
00403FA2    8D85 64FFFFFF   lea     eax, dword ptr [ebp-9C]
00403FA8    50              push    eax
00403FA9    8D85 4CFFFFFF   lea     eax, dword ptr [ebp-B4]
00403FAF    50              push    eax
00403FB0    E8 21D3FFFF     call    <jmp.&msvbvm60.__vbaStrVarVal>
00403FB5    50              push    eax
00403FB6    E8 21D3FFFF     call    <jmp.&msvbvm60.rtcAnsiValueBstr>
00403FBB    50              push    eax
00403FBC    E8 21D3FFFF     call    <jmp.&msvbvm60.__vbaStrI2>
00403FC1    8BD0            mov     edx, eax
00403FC3    8D4D BC         lea     ecx, dword ptr [ebp-44]
00403FC6    E8 1DD3FFFF     call    <jmp.&msvbvm60.__vbaStrMove>
00403FCB    8D8D 4CFFFFFF   lea     ecx, dword ptr [ebp-B4]
00403FD1    E8 FAD2FFFF     call    <jmp.&msvbvm60.__vbaFreeStr>
00403FD6    FF75 BC         push    dword ptr [ebp-44]
00403FD9    E8 ECD2FFFF     call    <jmp.&msvbvm60.__vbaLenBstr>
00403FDE    8985 E4FEFFFF   mov     dword ptr [ebp-11C], eax                     
00403FE4    C785 DCFEFFFF 0>mov     dword ptr [ebp-124], 3
00403FEE    8D95 DCFEFFFF   lea     edx, dword ptr [ebp-124]
00403FF4    8D4D A8         lea     ecx, dword ptr [ebp-58]
00403FF7    E8 10D3FFFF     call    <jmp.&msvbvm60.__vbaVarMove>
00403FFC    C785 E4FEFFFF 0>mov     dword ptr [ebp-11C], 1
00404006    C785 DCFEFFFF 0>mov     dword ptr [ebp-124], 2
00404010    C785 D4FEFFFF 0>mov     dword ptr [ebp-12C], 1
0040401A    C785 CCFEFFFF 0>mov     dword ptr [ebp-134], 2
00404024    8D85 DCFEFFFF   lea     eax, dword ptr [ebp-124]                      ; step:1
0040402A    50              push    eax
0040402B    8D45 A8         lea     eax, dword ptr [ebp-58]                       ; end:strlen(name[i])
0040402E    50              push    eax
0040402F >  8D85 CCFEFFFF   lea     eax, dword ptr [ebp-134]                      ; start:1
00404035    50              push    eax
00404036    8D85 54FEFFFF   lea     eax, dword ptr [ebp-1AC]
0040403C    50              push    eax
0040403D    8D85 64FEFFFF   lea     eax, dword ptr [ebp-19C]
00404043    50              push    eax
00404044    8D85 74FFFFFF   lea     eax, dword ptr [ebp-8C]
0040404A    50              push    eax
0040404B    E8 B0D2FFFF     call    <jmp.&msvbvm60.__vbaVarForInit>
00404050    8985 38FEFFFF   mov     dword ptr [ebp-1C8], eax
00404056    E9 A8000000     jmp     00404103
0040405B    C785 34FFFFFF 0>mov     dword ptr [ebp-CC], 1
00404065    C785 2CFFFFFF 0>mov     dword ptr [ebp-D4], 2
0040406F    8D45 BC         lea     eax, dword ptr [ebp-44]
00404072    8985 E4FEFFFF   mov     dword ptr [ebp-11C], eax
00404078    C785 DCFEFFFF 0>mov     dword ptr [ebp-124], 4008
00404082    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]
00404088    50              push    eax
00404089    8D85 74FFFFFF   lea     eax, dword ptr [ebp-8C]
0040408F    50              push    eax
00404090    E8 5FD2FFFF     call    <jmp.&msvbvm60.__vbaI4Var>
00404095    50              push    eax
00404096    8D85 DCFEFFFF   lea     eax, dword ptr [ebp-124]
0040409C    50              push    eax
0040409D    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
004040A3    50              push    eax
004040A4    E8 51D2FFFF     call    <jmp.&msvbvm60.rtcMidCharVar>
004040A9    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
004040AF    50              push    eax
004040B0    E8 0FD2FFFF     call    <jmp.&msvbvm60.__vbaI2Var>
004040B5    66:8945 94      mov     word ptr [ebp-6C], ax
004040B9    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
004040BF    50              push    eax
004040C0    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]
004040C6    50              push    eax
004040C7    6A 02           push    2
004040C9    E8 56D2FFFF     call    <jmp.&msvbvm60.__vbaFreeVarList>
004040CE    83C4 0C         add     esp, 0C
004040D1    66:8B45 D8      mov     ax, word ptr [ebp-28]
004040D5    66:0345 94      add     ax, word ptr [ebp-6C]
004040D9    0F80 3D060000   jo      0040471C
004040DF    66:8945 D8      mov     word ptr [ebp-28], ax
004040E3    8D85 54FEFFFF   lea     eax, dword ptr [ebp-1AC]
004040E9    50              push    eax
004040EA    8D85 64FEFFFF   lea     eax, dword ptr [ebp-19C]
004040F0    50              push    eax
004040F1    8D85 74FFFFFF   lea     eax, dword ptr [ebp-8C]
004040F7    50              push    eax
004040F8    E8 C1D1FFFF     call    <jmp.&msvbvm60.__vbaVarForNext>
004040FD    8985 38FEFFFF   mov     dword ptr [ebp-1C8], eax
00404103    83BD 38FEFFFF 0>cmp     dword ptr [ebp-1C8], 0
0040410A  ^ 0F85 4BFFFFFF   jnz     0040405B                                      ; 内层循环结束
00404110    FFB5 50FFFFFF   push    dword ptr [ebp-B0]
00404116    FF75 D8         push    dword ptr [ebp-28]
00404119    E8 C4D1FFFF     call    <jmp.&msvbvm60.__vbaStrI2>
0040411E    8BD0            mov     edx, eax
00404120    8D8D 4CFFFFFF   lea     ecx, dword ptr [ebp-B4]
00404126    E8 BDD1FFFF     call    <jmp.&msvbvm60.__vbaStrMove>
0040412B    50              push    eax
0040412C    E8 7BD1FFFF     call    <jmp.&msvbvm60.__vbaStrCat>
00404131    8985 04FFFFFF   mov     dword ptr [ebp-FC], eax
00404137    C785 FCFEFFFF 0>mov     dword ptr [ebp-104], 8
00404141    C785 24FFFFFF 0>mov     dword ptr [ebp-DC], 1
0040414B    C785 1CFFFFFF 0>mov     dword ptr [ebp-E4], 2
00404155    C785 E4FEFFFF 0>mov     dword ptr [ebp-11C], 1
0040415F    C785 DCFEFFFF 0>mov     dword ptr [ebp-124], 2
00404169    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
0040416F    50              push    eax
00404170    8D45 84         lea     eax, dword ptr [ebp-7C]
00404173    50              push    eax
00404174    8D85 DCFEFFFF   lea     eax, dword ptr [ebp-124]
0040417A    50              push    eax
0040417B    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]
00404181    50              push    eax
00404182    E8 1FD1FFFF     call    <jmp.&msvbvm60.__vbaVarAdd>                   
00404187    50              push    eax
00404188    E8 67D1FFFF     call    <jmp.&msvbvm60.__vbaI4Var>
0040418D    50              push    eax
0040418E    8D45 98         lea     eax, dword ptr [ebp-68]
00404191    50              push    eax
00404192    8D85 0CFFFFFF   lea     eax, dword ptr [ebp-F4]
00404198    50              push    eax
00404199    E8 5CD1FFFF     call    <jmp.&msvbvm60.rtcMidCharVar>
0040419E    8D85 FCFEFFFF   lea     eax, dword ptr [ebp-104]
004041A4    50              push    eax
004041A5    8D85 0CFFFFFF   lea     eax, dword ptr [ebp-F4]
004041AB    50              push    eax
004041AC    8D85 ECFEFFFF   lea     eax, dword ptr [ebp-114]
004041B2    50              push    eax
004041B3    E8 FAD0FFFF     call    <jmp.&msvbvm60.__vbaVarCat>
004041B8    50              push    eax
004041B9    E8 FAD0FFFF     call    <jmp.&msvbvm60.__vbaStrVarMove>
004041BE    8BD0            mov     edx, eax
004041C0    8D8D 50FFFFFF   lea     ecx, dword ptr [ebp-B0]
004041C6    E8 1DD1FFFF     call    <jmp.&msvbvm60.__vbaStrMove>
004041CB    8D8D 4CFFFFFF   lea     ecx, dword ptr [ebp-B4]
004041D1    E8 FAD0FFFF     call    <jmp.&msvbvm60.__vbaFreeStr>
004041D6    8D85 ECFEFFFF   lea     eax, dword ptr [ebp-114]
004041DC    50              push    eax
004041DD    8D85 0CFFFFFF   lea     eax, dword ptr [ebp-F4]
004041E3    50              push    eax
004041E4    8D85 FCFEFFFF   lea     eax, dword ptr [ebp-104]
004041EA    50              push    eax
004041EB    8D85 1CFFFFFF   lea     eax, dword ptr [ebp-E4]
004041F1    50              push    eax
004041F2    8D85 2CFFFFFF   lea     eax, dword ptr [ebp-D4]
004041F8    50              push    eax
004041F9    6A 05           push    5
004041FB    E8 24D1FFFF     call    <jmp.&msvbvm60.__vbaFreeVarList>
00404200    83C4 18         add     esp, 18
00404203    66:8365 D8 00   and     word ptr [ebp-28], 0
00404208    8D85 74FEFFFF   lea     eax, dword ptr [ebp-18C]
0040420E    50              push    eax
0040420F    8D85 84FEFFFF   lea     eax, dword ptr [ebp-17C]
00404215    50              push    eax
00404216    8D45 84         lea     eax, dword ptr [ebp-7C]
00404219    50              push    eax
0040421A    E8 9FD0FFFF     call    <jmp.&msvbvm60.__vbaVarForNext>
0040421F    8985 3CFEFFFF   mov     dword ptr [ebp-1C4], eax
00404225    83BD 3CFEFFFF 0>cmp     dword ptr [ebp-1C4], 0
0040422C  ^ 0F85 1FFDFFFF   jnz     00403F51                                      ; 外层循环结束 
```

### 验证

```
00404303    E8 98CFFFFF     call    <jmp.&msvbvm60.__vbaStrToAnsi>                ; 输入的字符串
00404308    50              push    eax
00404309    FFB5 50FFFFFF   push    dword ptr [ebp-B0]
0040430F    8D85 4CFFFFFF   lea     eax, dword ptr [ebp-B4]
00404315    50              push    eax
00404316    E8 85CFFFFF     call    <jmp.&msvbvm60.__vbaStrToAnsi>                ; 计算出的字符串
0040431B    50              push    eax
0040431C    E8 07E7FFFF     call    00402A28                                      ; 通过DLLFunction调用字符串比较函数
00404321    8985 A8FEFFFF   mov     dword ptr [ebp-158], eax                      ; 比较结果保存到ebp-158中 
```

## 注册机代码

```
#include<stdio.h>
#include<stdlib.h>
int main(int argc,char ** argv)
{
  if(argc!=2) return 0;
  int len,tmp;
  len=strlen(argv[1]);
  printf("key:");
  for(int i=0;i<len;i+=2)
    {

      tmp=(int)argv[1][i];
      printf("%d",tmp/100+tmp%100/10+tmp%10);
      if(i>=len-1 && len%2!=0) goto end;
      printf("%c",argv[1][i+1]);
    }
 end: 
  return 0;
} 
```