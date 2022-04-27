<!--yml
category: crackme160
date: 2022-04-27 18:15:51
-->

# CrackMe160 学习笔记 之 059_一剑名动江湖的博客-CSDN博客

> 来源：[https://blog.csdn.net/guaigle001/article/details/105936046](https://blog.csdn.net/guaigle001/article/details/105936046)

## 前言

这也是个**VB**程序。和上个程序是一个系列的。

注册码是"**This is the correcj code**",极有可能是作者拼错了。

## 思路

把注册码分成五部分，转成ASCII码字符串依次和输入相比较。

讲道理，没什么难度，但VB的反汇编看起来要花时间。

## 分析

### 关键逻辑

```
00402FE7   .  50            push    eax                              ; /String8
00402FE8   .  8D85 1CFFFFFF lea     eax, dword ptr [ebp-E4]          ; |
00402FEE   .  50            push    eax                              ; |ARG2
00402FEF   .  E8 2AE2FFFF   call    <jmp.&MSVBVM60.__vbaStrVarVal>   ; \__vbaStrVarVal
00402FF4   .  50            push    eax
00402FF5   .  E8 06E2FFFF   call    <jmp.&MSVBVM60.#581>
00402FFA   .  DD9D A0FEFFFF fstp    qword ptr [ebp-160]              ;  固定字符串转成浮点数
00403000   .  8D45 84       lea     eax, dword ptr [ebp-7C]
00403003   .  50            push    eax                              ; /String8
00403004   .  8D85 20FFFFFF lea     eax, dword ptr [ebp-E0]          ; |
0040300A   .  50            push    eax                              ; |ARG2
0040300B   .  E8 0EE2FFFF   call    <jmp.&MSVBVM60.__vbaStrVarVal>   ; \__vbaStrVarVal
00403010   .  50            push    eax                              ;  输入字符串转成浮点数
00403011   .  E8 EAE1FFFF   call    <jmp.&MSVBVM60.#581>
00403016   .  DCA5 A0FEFFFF fsub    qword ptr [ebp-160]              ;  字符串相减
0040301C   .  DD9D D0FEFFFF fstp    qword ptr [ebp-130]              ;  结果弹出到 ebp-130 
```

```
0040305F   .  83A5 D0FEFFFF>and     dword ptr [ebp-130], 0           ;  清空结果的低四个字节
00403066   .  C785 C8FEFFFF>mov     dword ptr [ebp-138], 8002
00403070   .  8D85 24FFFFFF lea     eax, dword ptr [ebp-DC]
00403076   .  50            push    eax                              ; /var18
00403077   .  8D85 C8FEFFFF lea     eax, dword ptr [ebp-138]         ; |
0040307D   .  50            push    eax                              ; |var28
0040307E   .  E8 71E1FFFF   call    <jmp.&MSVBVM60.__vbaVarTstEq>    ; \__vbaVarTstEq 
```

可以看到，**最后将清空低四个字节的浮点数结果和浮点数结果比较，变相就是结果为0时才成立进行下一步跳转。**

VB的指令虽然有点奇葩，但是还是能分析出来的。