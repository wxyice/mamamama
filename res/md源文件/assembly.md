# 汇编语言
[TOC]

- [指令](#指令)
  - [数据传输指令](#数据传输指令)
    - [通用数据传输指令](#通用数据传输指令)
    - [端口输出指令](#端口输入输出指令)
    - []()
  - []()
    - 
## 指令
### 数据传输指令
#### 通用数据传输指令

1. mov
2. push
3. pop
4. xchg

#### 端口输入输出指令
1. in
2. out


#### 地址传输指令

1. lea
2. lds
3. les

#### 标志寄存器传输指令

1. lahf
2. sahf
3. pushf
4. popf


### 扩充指令
#### 符号扩充指令

1. cbw
2. cwd
3. cdq
4. movsx

#### 零扩充指令

1. movzx

#### 换码指令
1. xlat

### 算术运算指令

#### 加法运算

1. add
2. adc
3. inc

#### 减法运算

1. sub
2. sbb
3. dec
4. neg
5. (*)cmp

#### 乘除法运算

1. mul
2. imul

1. div
2. idiv

#### 小数运算

1. fadd
2. fsub
3. fdiv
4. fmul

### 跳转指令

#### 无条件跳转
1. jmp

#### 有条件跳转

1. jz jnz
2. jc jnc
3. js jns
4. jo jno
5. jp jnp

#### 有条件非符号数短跳

1. ja
2. jb
3. jae
4. jbe

#### 有条件符号数短跳

1. jg
2. jl
3. jge
4. jle

#### cx跳转
1. jcxz



### 逻辑运算指令

1. and
2. or
3. xor
4. not
5. test

### 移位指令
1. shr
2. shl
3. sar
4. sal
5. ror
6. rol
7. rcr
8. rcl

### 字符串操作指令
#### 重复指令
1. rep
2. repe
3. repne
4. repz
5. repnz

#### 字符串相关指令

1. movs
2. cmps
3. scas
4. stos
5. lods

### 循环指令

1. loop

### 子程序调用指令

1. call ret
2. call retf

### 中断和中断返回
1. int iret



