# 第一讲
汇编特点：控制强，代码短 速度快
    汇编有可能实现高级语言实现不了的功能


1. 16位汇编和32位汇编的区别
    1. 宽度的区别
        16位是dos下的汇编语言
        32位是window或Linux的汇编语言

        16位 寄存器16位 地址16位
        32位 寄存器32位 地址32位
    2. 调用操作系统函数有区别

2. 16位汇编语言编程演示
    1. 编译->.obj
    2. 连接->.exe

3. 调试
    td Turbo Debugger
    单步执行 单步跳转

# 第二讲

1. 变量定义
    db byte 8          ==>unsigned char 1B
    dw word 16         ==>unsigned short int 2B
    dd double 32       ==>unsigned long int/float 32 位小数 4B
    dq quadruple word  ==> double 64位      8B
    dt                 ==> long double 80位小数 10B
    
    汇编语言在定义的时候不区分有符号还是无符号
    但是在引用的时候可以用指令来区分是否是有符号数
    ```
    eg: a db 0FFh ;此时不知道是255还是-1
        b db 0FFh ; 同样也不知道
        
        mov al,a 
        mul b     ; 表示255*255 因为mul是非符号是乘法
        
        mov al,a 
        imul b    ; 表示-1*-1 因为imul是符号数乘法
    ```

2. 小端规则
    先存放低8位，后存放高8位的规则称为
    低位低地址，高位高地址

3. 取值范围
    非符号数

    8位  [00h-0FFh] [0-255]
    16位 [0000h-0FFFFh] [0-65535]
    32位 [00000000h-0FFFFFFFFh] [0,2^32-1]

    符号数
    
    8位  [80h-7Fh] [-128,+127]
    16位 [8000h-7FFFh] [-32768,32767]
    32位 [80000000h,7FFFFFFFh] [-2147483648, +2147483647]

4. 扩充
    当把一个宽度较小的值赋给宽度较大的变量时，会发生扩充。
    1. 零扩充
        针对非符号数
        扩充位全部置0即可
    2. 符号扩充
        针对符号数
        扩充位全部置符号位 负数置1 正数置0

5. 小数表示方法

# 第三讲

1. 算数运算
    
    add
        不能add 变量，变量
    sub
    mov 
    语法逻辑相同 要求前后等宽 
    变量是没有长度的 （ptr修饰）（当常数赋值给变量的时候，一定要用）

    mul
        8位
        al*乘数=ax
        16位
        ax*乘数=dx：ax
        32位
        eax*乘数=edx：eax

    div
        (1) 16位除以8位得8位
        ax /除数 =al。。ah
        ah余数
        al商
        (2) 32位除以16位得16位
        dx：ax/除数 =ax 。。dx
        ax商
        dx余数

        (3) 64位除以32位得32位
        edx：eax/除数 =eax。。edx


2. 逻辑运算
    shl
    shr

    xor

    rol
    ror

# 第四讲

1. 段地址
    编程的时候只能使用逻辑地址，不能使用物理地址。  
    段起始地址的16进制个位必须=0, 否则不能成为段起始地址
    段长度=2233F-12340+1=22340-12340=10000h=64K

    一个物理地址可以表示成多个逻辑地址, 例如:
    12398h=1234:0058=1235:0048=1236:0038=1230:0098 
    段地址的1相当于偏移地址的10h:
    段地址+1就意味着偏移地址-10h;
    段地址-1就意味着偏移地址+10h;

2. 寻址方式
    除了cs不能通过mov修改外，ds、es、ss均可以修改。
    1. 直接寻址
    用常数来表示变量的偏移地址
    mov al, 1000h:[2000h]; 语法错误 段地址用寄存器
    mov al, ds:[2000h]

    2. 间接寻址
    用寄存器、寄存器+常数来表示偏移地址
    mov al, ds:[bx] ; 只能用 bx,bp,si,di 
        - [bx]  [bp]  [si]  [di]就成了最简单的间接寻址方式
        - [bx+si] [bx+di] [bp+si] [bp+di]
        - [bx+2] [bp-2] [si+1] [di-1]
        - [bx+si+2] [bx+di-2] [bp+si+1] [bp+di-1]
    bx或bp通常用来表示数组的首地址, 而si或di则用来表示下标。
    两个寄存器相加再加一个常数的间接寻址通常用来访问结构数组中某个元素中的某个成员

3. 引用数组元素
    若[]中不包含寄存器bp，则该变量默认的段地址一定是ds

# 第五讲
1. ptr
    byte ptr
    word ptr
    dword ptr
    当目标操作数与源操作数的其中之一有明确的宽度时，另外一方不需要加类型修饰
    当源操作数为常数，目标操作数为变量时，则该变量必须加上类型修饰
    byte ptr ; 1字节
    word ptr ; 2字节
    dword ptr; 4字节(32位整数或float类型小数)
    fword ptr; 6字节(4字节偏移地址+2字节段地址)
    qword ptr; 8字节(64位整数或double类型小数)
    tbyte ptr; 10字节(long double类型的80位小数)
    short     用来修饰一个短的标号
    near ptr 用来修饰一个近的标号
    far ptr  用来修饰一个远的标号


2. assume 作用
    帮助编译器建立段寄存器与段的关联
    当源程序中引用了某个 段内的变量时，编译器会在编译出来的机器码中把变量的段地址替换成关联的段寄存器。

3. 图形界面编程
    1. 切换图形模式
        调用int 10h中断
        要切换到分辨率320*200,颜色为256色的图形模式:
        ```
        mov ah, 0  	; 其中AH=0表示int 10h的子功能号为0
        mov al, 13h; 其中AL=13h代表图形模式编号
        ```

        int 10h     ; int 10h是与显示相关的bios中断

    2. 计算偏移地址
        y*320+x

    3. 写入数据
        一个点对应一个字节,该字节的值表示该点的颜色
        0=黑 1=蓝   2=绿  3=青   4=红  5=洋红  6=棕 7=白
        8=灰 9=亮蓝 A=亮绿 B=亮青 C=亮红 D=紫   E=黄 F=亮白

# 第六讲
1. ss:sp 堆栈段
    ss: stack segment 表示堆栈的段地址
    sp: stack pointer 堆栈指针 表示堆栈顶端的偏移地址

    不能用[sp]或[sp+常数或其它寄存器]的形式来引用某个变量
    为了弥补sp不能表示间接地址的缺陷，同时也为了凑齐4个偏移地址寄存器（？？？有毒）
    于是从通用寄存器中找来bx作为sp的替补，于是就有了以下4个偏移地址寄存器:bx, bp, si, di

    1. push 的过程
        @sp=sp-2 
        @把push后的值保存到ss:sp的内存单元中
        高地址先保存 低地址后保存 依旧满足小端规则
    2. pop 过程
        @把当前ss:sp 指向的word(16位)取出来保存到pop后的变量中
        @sp=sp+2

2. es
    es:extra segment 附加段

3. FL标志寄存器
    mov 指令不影响标志位
    一共有16位 只有9位有用 9位包括有6个状态标志和3控制标志
    11 10  9  8  7  6  4  2  0(ODITSZAPC)
    OF DF IF TF SF ZF AF PF CF

    CF carry flag 进位标志 jc jnc
    ZF zero flag 零标志 jz jnz je jne
    SF sign flag 符号标志 首位0or1 js jns 
    OF overflow flag 溢出标志 有符号数溢出标志 jo jno
    PF parity flag 奇偶标志 偶数PF=1 奇数PF=0 jp(jpe) jnp(jpo)
        它只统计低8位
    AF auxiliary flag 辅助进位标志 低4位向高4位产生的辅助进位或借位
        主要和BCD码有关 （binary coded decimal）
        AF 等于1 要加6调整 个位和十位之间的调整
    
    CF ZF SF OF AF PF: 这6个称为状态标志

    DF TF IF: 这3个称为控制标志
    DF direction flag 
        控制字符串操作的方向
        DF=0 正方向（低地址到高地址）
        DF=1 反方向（高地址到低地址）

        cld 令DF=0
        std 令DF=1

        若源数据首地址>目标数据首地址，则复制时要按正方向(从低地址到高地址)；
        若源数据首地址<目标数据首地址，则复制时要按反方向(从高地址到低地址)
        strcpy(target, source); 永远按正方向复制
        memcpy(target, source, n);永远按正方向复制
        memmove(target, source, n); 能正确处理部分重叠
    IF interrupt flag
        IF=1 允许中断
        IF=0 禁止中断
        cli IF=0
        sti IF=1
        此时若用户敲键,则CPU会在此处插入一条int 9h指令并执行它
        若程序已运行了1/18秒,则cpu会在此处插入一条int 8h指令
        用cli和sti把一段代码包围起来可以达到该段代码在执行过程中不会被打断的效果

    TF trap flag
        TF =1 CPU 进入单步模式 
        当TF=1时,CPU在每执行完一条指令后,会自动在该条指令与下条指令之间插入一条int 1h指令并执行它。
        利用单步模式可以实现反调试:

4. 端口
    CPU <-> 端口(port)  <-> I/O设备
    端口编号就是端口地址。端口地址的范围是：[0000h, 0FFFFh]，共65536个端口。
    对端口操作使用指令in与out实现。
    60h号端口，CPU与键盘之间可以建立通讯。
    in al, 60h; 从端口60h读取一个字节并存放到AL中

    70h及71h端口与cmos内部的时钟有关。
    其中cmos中的地址4、2、0中分别保存了当前的时、分、秒，并且格式均为BCD码。

# 第七讲
1. 1. 32位间接寻址方式
(1) 32位比16位多了以下这种寻址方式:
[寄存器+寄存器*n+常数]
n=2,4,8
(2) 32位寻址方式里面，对[]中的两个寄存器几乎不加限制

2. 段跨越
    通过在操作数前添加一个段前缀(segment prefix)如CS:、DS:、ES:、SS:来强制改变操作数的段址，这就是段跨越。

    段地址的隐含
    []无bp寄存器的时候 隐含的端地址是ds
    当[]中包含有寄存器bp时，该变量的段地址一定是ss。

3. 通用数据传送指令：MOV，PUSH，POP，XCHG
    mov byte ptr ds:[bx], byte ptr es:[di]
    错误原因:两个操作数不能同时为内存变量

    push/pop后面也可以跟变量

    8086中, push不能跟常数, 但80386及以后的cpu允许push一个常数。

    xchg 交换两个寄存器的数

4. 地址传送指令
    lea
        lea dest，src
        ```
        lea dx，ds:[1000h]  ;DX=1000h
        mov dx,1000h ; lea 相当于mov

        lea dx，abc== mov dx offset abc
        lea dx, ds:[bx+si+3];dx= bx+si+3
        ```
        用lea做乘法
        lea eax,[eax+4*eax]
        lea eax,[eax+eax*2]

    lds
    les
    假定要把1000:0000中存放的远指针取出来，存放到
    es:bx中，则
    mov ax, 1000h
    mov ds, ax
    les bx, dword ptr ds:[0000h]; es=1234h, bx=5678h


5. 远指针
    16位汇编中，远指针是指16位段地址+16位偏移地址;
    32位汇编中，远指针是指16位段地址+32位偏移地址。

    32位系统下的远指针
    mov eax, ds:[ebx]; 这里的ebx表示32位偏移地址
    在32位系统下，由于偏移地址的范围是[0, 232-1]
    在段地址不变只变化偏移地址的情况下可以访问4G空间
    所以段寄存器cs=ds=es=ss并且是不变的

    32位汇编中，远指针是指16位段地址+32位偏移地址。
    若变量p定义成48位的远指针，则它的类型修饰为:
    fword ptr
    p 	dd 12345678h; p里面存放了一个48位的远指针
  	    dw 18h       ; 18h:12345678h
	begin:
        les ebx, fword ptr cs:[p]; es=18h
                                 ; ebx=12345678h
    
    gdt表
    该数组的首地址存放在gdtr寄存器内
    数组中每个元素的宽度均为8字节
    把段地址18h与t相加得到gdt表内第3个元素的偏移
    再取出t+18h指向的元素的值(共8字节), 其中第2、3、4、7个字节逆向排列得到段首地址=10203040h
    18h:12345678h = 10203040h + 12345678h
        逻辑地址                 物理地址
    8字节中余下的4个xx计32位，其中20位用来表示段长度(长度单位可以是字节也可以是页，其中1页=4K)，剩余的12位中有一部分位用来表示段的ring级别(0、1、2、3共4级)及权限(读Read、写Write、执行eXecute)。
    系统代码与用户代码用ring分级: 其中系统代码是ring0，而用户代码是ring3。
    ring级别保存在cs低2位中。当用户代码要访问某个段如18h时，cpu会检查用户代码的(cs & 0x0003)是否小于等于18h对应段描述中的ring级别，若条件为真则允许访问，否则拒绝访问。
            保护模式(段有权限)<-> 实模式(段无权限)



6. 近指针(near pointer)：
    16位汇编中，近指针是指16位的偏移地址；
    32位汇编中，近指针是指32位的偏移地址；
    远指针(far pointer)包括段地址及偏移地址两个部分；
    近指针(near pointer)只包括偏移地址，不包含段地址。


7. pushf/popf
    把标志寄存器FL压入堆栈/弹出堆栈
    不能写成push FL  ,  pop FL
    寄存器FL及IP在编程时都是不能直接引用的:
        mov ip, ax;\
        mov ax, ip; \ 都是错误的
        mov bx, fl; /
        mov fl, bx;/
    间接改变ip
    改变FL
    stc指令可以使CF=1;
    clc指令可以使CF=0;
    cld指令可以使DF=0;
    std指令可以使DF=1;
    cli指令可以使IF=0;
    sti指令可以使IF=1;

    但并不存在指令clt、stt来改变TF的值。要改变TF的值必须通常pushf和popf实现:

    在常数表达式中除了+ - * /运算符外，还可以使用not, or, and, xor等运算符
    pushf/popf配合起来除了可以刻意改变FL中的某些位外，也可以用来保护/恢复FL的值:

    在32位系统中, EFL是一个32位寄存器, 对应的指令是pushfd和popfd; 若在32位系统中使用pushf/popf则控制的仅是EFL的低16位即FL。

8. 符号扩充指令
    cbw:convert byte to word            al->ax
    cwd:convert word to double word     ax->dx:ax
    cdq:convert double word to quadruple word   eax->edx:eax

    新的符号扩充指令: movsx
    movsx ax, al; sx:sign extension符号扩充


9. 零扩充指令
    movzx
    movzx ax, al;
    movzx eax, al;
    movzx ebx, cx;

# 第八讲
1. 换码指令/查表指令
    xlat
    在xlat执行前必须让ds:bx指向表, al必须赋值为
    数组的下标; 执行xlat后, AL=ds:[bx+AL]
    ```
    mov bx, offset t; BX=表的首地址
    mov al, 10; AL为下标
    xlat; 结果AL='A'
    xlat指令要求DS:BX指向数组，AL=数组下标。
    执行指令后, AL=数组元素
    ```
2. 算数指令
    （1）加法add inc adc
    inc自加 不影响标志位
    add影响CF
    adc 带进位加
    （2）减法 sub sbb dec neg cmp
    dec 自减 不影响CF
    neg 求相反数 会影响 CF，ZF，SF等标志位
        相当于 做零0-ax
    neg ax =（not ax）+1 /-x=~x+1
    sbb 带位减法
    cmp 与sub类似，但是只保留标志位
        cmp ax, bx
        je equal ;zf=1跳
        jz
    （3）比较相关跳转指令
    1. ja,jb,jae,jbe 非符号数
    ja:CF=0且ZF=0 跳
    jb: CF=1 jb=jc
    jae
    jab
    2. jg jl jge,jle 符号数
    jg:SF ==OF 
    jl:SF!=OF

    (4)乘法mul，imul
    mul 非符号数
    imul 符号数

    386以上CPU对imul功能进行了扩充：
    ①imul eax,    ebx,     1234h===eax=eax*1234h
            寄存器  寄存器或  只能是常数
                变量

    （5）除法 div idiv
    （6）小数运算
    fadd
    fsub
    fmul
    fdiv
    小数用 dd dq dt定义
    八个小数寄存器
    st（0）～st（7） 其中st（0）就是st
    每个寄存器达到80位 相当于long double

# 第九讲
1. 除法溢出
    1. 除以零
    2. 商大于可以保存的长度也会发生溢出

    除法溢出会插入int 00h并执行
    dos下 int 00h会显示溢出信息 终止运行

2. 逻辑运算指令
    and
    or
    xor
    not
    test 与and 的关系相当于cmp和sub

    判断寄存器是否为0的方法
    test cl,cl
    or cl,cl
    and cl,cl
    or cl,0
    cmp cl,0
    用jz jnz 判断
3.移位指令
    shl 
        shl dx，cl

    shr
    shl及shr是针对非符号数的移位运算,无论左移还是右移, 空缺的部分永远补0。


    算术左右
    sal

    sar
    sal及sar是针对符号数的移位运算, 对负数右移的时候要在左边补1, 对正数右移的时候左边补0, 无论对正数还是负数左移右边都补0。显然sal≡shl。


    rol
    ror

    rcl 带位循环
    rcr

    shl, shr, rol, ror, rcl, rcr最后移出去的那一位一定在CF中。
4. 字符串操作
    1. 传送
        movsb
        movsw
        movsd

        rep movsb （string byte）
        要准备
        ①ds:si->源字符串(si就是source index)
        ②es:di->目标字符串(di就是destination index)
        ③cx=移动次数
        ④DF=0即方向标志设成正方向(用指令cld)
        每次加1di

        rep movsw  每次加2di
        rep movsd 每次加4di

        在32位系统下, 假定ds:esi->源内存块, es:edi->目标块, DF=0, 则当要复制的字节数ecx不是4的倍数时，可以做如下处理:
        push ecx
        shr ecx, 2
        rep movsd
        pop ecx
        and ecx, 3; 相当于ecx = ecx % 4
        rep movsb

    2. 比较指令
    cmpsb
    cmpsw
    cmpsd

    1. cmpsb
    2. repe cmpsb
    3. repne cmpsb

    3. 扫描

    scasb
    scasw
    scasd

    repne scasb

    4. 赋值
    stosb
    es：[di]=al
    stosw
    stosd

    5. 取值
    lodsb
    al=ds:[si]

5. 控制转移
    1. jmp
        ①jmp short target			; 短跳
        ②jmp near ptr target 	; 近跳
        ③jmp far ptr target  	; 远跳

        写源程序的时候不需要加上short、near ptr、far ptr等类型修饰。即上述三种写法一律可以简化为jmp target。

        短跳指令
        ①短跳指令的格式
        jmp 偏移地址或标号
        以下条件跳转指令也都属于短跳: jc jnc jo jno js jns jz jnz ja jb jae jbe jg jge jl jle jp jnp

        机器码 EB 目标地址-下一条指令的偏移地址


        短跳太远跳不过去的解决办法
        换成jmp

        近跳
        jmp 偏移地址或标号; 如jmp 1000h
        jmp 16位寄存器		; 如jmp bx
        jmp 16位变量  		; 如jmp word ptr [addr]

        ②近跳指令的机器码
        地址        机器码      汇编指令    
        1D3E:0100   E9FD1E      jmp  2000h
        近跳指令的第1个字节=E9
        第2个字节=Δ=目标地址-下条指令的偏移地址
        =2000h-103h=1EFDh

        (4)远跳指令
        ①远跳指令的2种格式
        jmp 段地址:偏移地址
        jmp dword ptr 32位变量
        ②远跳指令的机器码
        jmp 1234h:5678h; 机器码为0EAh,78h,56h,34h,12h
        远跳到某个常数地址时,在源程序中不能直接用jmp指令，而应该改用机器码0EAh定义，如:
        db 0EAh
        dw 5678h
        dw 1234h
        上述3行定义合在一起表示jmp 1234h:5678h
6. 循环指令
    loop
    CX = CX - 1   	; 循环次数减1
    if(CX != 0)   	; 若CX不等于0，则
        goto  dest 	; 跳转至dest
7. call ret
(1) 汇编语言中的三种参数传递方式
① 寄存器传递
f:
   add ax, ax; ax=2*ax
   ret        ; 返回时ax就是函数值
main:
   mov ax, 3; ax就是f()的参数
   call f
next:
   mov ah, 4Ch
   int 21h


② 变量传递
f:
   mov ax, var
   add ax, ax; ax就是函数值
   ret
main:
   mov var, 3; var是一个16位的变量, 用作参数
   call f

在汇编语言中，用db、dw等关键词定义的变量均为全局变量。在堆栈中定义的变量才是局部变量。
③ 堆栈传递
f:
   push bp
   mov bp, sp
   mov ax, [bp+4]; 从堆栈中取得参数
   add ax, ax
   pop bp
   ret
main:
   mov ax, 3
   push ax; 参数压入到堆栈
   call f
   add sp, 2

(2) C语言函数调用y=f(2,3)求两数之和转化成汇编语言
f:
   push bp;(4)
   mov bp, sp
   mov ax, [bp+4]
   add ax, [bp+6]
   pop bp; (5)
   ret; (6)
main:
   mov ax, 3
   push ax; (1)
   mov ax, 2
   push ax; (2)
   call f; (3)
here:
   add sp, 4;(7)
上述程序运行过程中的堆栈布局如下:
ss:1FF8 old bp <- bp (4)
ss:1FFA here <- (3)(5)
ss:1FFC 02   <- (2)(6)
ss:1FFE 03   <- (1)
ss:2000 ??   <- (7)

1. 用堆栈传递参数有3种方式:
(1) __cdecl 
参数从右到左顺序压入堆栈，由调用者清理堆栈;
是C语言参数传递规范。
__cdecl的例子:
f:
push bp; (4)
mov bp, sp
mov ax, [bp+4]; arg0
add ax, [bp+6]; arg1
pop bp; (5)
ret; (6)
main:
mov ax, 20; arg1
push ax; (1)
mov ax, 10; arg0
push ax; (2)
call f; (3)
here:
add sp, 4; (7)
__cdecl堆栈布局:
ss:1FF8 old bp  bp (4)
ss:1FFA here  (3)(5)
ss:1FFC 10  (2)(6)
ss:1FFE 20  (1)
ss:2000      (7)
(2) __pascal
参数从左到右顺序压入堆栈，由被调用者清理堆栈;
是Pascal语言参数传递规范。
__pascal的例子：
f:
push bp; (4)
mov bp, sp
mov ax, [bp+6]; arg0
add ax, [bp+4]; arg1
pop bp; (5)
ret 4; (6)
main:
mov ax, 10
push ax; (1) arg0
mov ax, 20
push ax; (2) arg1
call f; (3)
here: 
__pascal的堆栈布局:
ss:1FF8 old bp<- bp (4)
ss:1FFA here <- (3)(5)
ss:1FFC 20 <- (2)
ss:1FFE 10 <- (1)
ss:2000    <-(6)
(3) __stdcall
参数从右到左顺序压入堆栈，由被调用者清理堆栈;
是Windows API函数的参数传递规范。
__stdcall的例子：
f:
push bp; (4)
mov bp, sp
mov ax, [bp+4]; arg0
add ax, [bp+6]; arg1
pop bp; (5)
ret 4; (6)
main:
mov ax, 20
push ax; (1) arg1
mov ax, 10
push ax; (2) arg0
call f; (3)
here: 
__stdcall的堆栈布局:
ss:1FF8 old bp<- bp (4)
ss:1FFA here <- (3)(5)
ss:1FFC 10 <- (2)
ss:1FFE 20 <- (1)
ss:2000    <-(6)

32位__stdcall的例子:
f:
push ebp;(4)
mov ebp, esp
mov eax,[ebp+8];arg0
add eax,[ebp+0Ch];arg1
pop ebp;(5)
ret 8;(6)
main:
push 20;(1) arg1
push 10;(2) arg0
call f;(3)
here:
32位__stdcall的堆栈布局:
ss:1FF0 old ebp<- ebp (4)
ss:1FF4 here <- (3)(5)
ss:1FF8 10 <- (2)
ss:1FFC 20 <- (1)
ss:2000    <-(6)

2. 动态变量
int f(int a, int b)
{
   int c; /* c是局部动态变量 */
   c = a+b;
   return c;
}
上述C语言函数可翻译成以下汇编代码:
f:
push bp; (4)
mov bp, sp
sub sp, 2; (5) 这里挖的坑就是给变量c的
mov ax, [bp+4]
add ax, [bp+6]
mov [bp-2], ax
mov ax, [bp-2]
mov sp, bp; (6)此时变量c死亡
pop bp; (7)
ret; (8)
main:
mov ax, 20
push ax; (1)
mov ax, 10
push ax; (2)
call f; (3)
here:
add sp, 4;(9)此时参数a,b死亡
执行上述代码时, 堆栈布局如下:
ss:1FF6 [30] (5) 变量c
ss:1FF8 old bp<- bp(4)(6)
ss:1FFA here <- (3)(7)
ss:1FFC 10 <- (2)(8)
ss:1FFE 20 <- (1)
ss:2000    <-(9)


3. C语言函数中需要保护bp, bx, si, di
C语言的函数里面除了不能破坏bp外，还要保护
bx,si,di的值:
f:
push bp
mov bp, sp
sub sp, n; 其中n一个常数,用来为动态变量分配空间
push bx
push si
push di
...
pop di
pop si
pop bx
mov sp, bp
pop bp
ret

4. 递归
int f(int n)
{
   if(n==1)
      return 1;
   return n+f(n-1);
}
上述C语言递归函数可翻译成以下汇编代码:
f:
push bp;(3)(6)(9)
mov bp, sp
mov ax, [bp+4]
cmp ax, 1
je done
dec ax
push ax;(4)(7)
call f;(5)(8)
there:
add sp, 2;(12)(15)
add ax, [bp+4]
done:
pop bp;(10)(13)(16)
ret;(11)(14)(17)
main:
mov ax, 3
push ax;(1)
call f;(2)
here:
add sp, 2;(18)
执行上述代码时的堆栈布局如下:
ss:1FEE oldbp<-bp(9)
ss:1FF0 there<-(8)(10)
ss:1FF2 1<-(7)(11)
ss:1FF4 oldbp<-bp(6)12
ss:1FF6 there<-(5)(13)
ss:1FF8 2<-(4)(14)
ss:1FFA oldbp<-bp(3)(15)
ss:1FFC here <-(2)(16)
ss:1FFE 3 <-(1)(17)
ss:2000   <-(18)

6. int、iret
int 21h对应的函数首地址保存在0:84h处, 该地址是一个远指针。
0:84 78h
0:85 56h
0:86 34h
0:87 12h
dword ptr 0:[84h]称为int 21h的中断向量(其实是它的函数首地址)
int n对应的中断向量的存储位置一定是0:n*4
n的取值范围是[00, FF], 所以256个中断向量会占用
0:0~0:3FF之间共400h个字节的内存，这块区域称为
中断向量表。
显然, int 00h的中断向量保存在dword ptr 0:[0];
int 01h的中断向量保存在dword ptr 0:[4];
int 03h的中断向量保存在dword ptr 0:[0Ch];
int 08h的中断向量保存在dword ptr 0:[20h];
int 0FFh的中断向量保存在dword ptr 0:[3FCh];
BIOS会完成部分中断向量的填写, 如int 10h、int 16h、int 13h这几个BIOS中断的向量在DOS启动前就已经填好了; DOS启动完成后，会填入int 21h的中断向量。

1000:2000 mov ah, 2
1000:2002 mov dl, 'A'
1000:2004 int 21h
1000:2006 mov ah,4Ch
上面这条int 21h指令执行时, cpu做了以下4件事:
pushf
push cs即1000h
push 下条指令的偏移地址即2006h
jmp dword ptr 0:[84h]; jmp 1234h:5678h

上面这条jmp会跳转到int 21h的中断服务函数内部:
1234:5678  ...
             ...
1234:56FF  iret; 中断返回
iret在执行时, cpu做了以下3件事情:
pop ip即ip=2006h
pop cs即cs=1000h
popf

1000:2000 call 1234:5678; 远调用
;此时会push 1000h 再 push 2005h
;最后jmp 1234:5678
1000:2005 mov ah, 4Ch
1000:2007 int 21h
...
1234:5678 ...
            retf; 此时执行pop ip再pop cs

中断程序例子:
http://10.71.45.100/bhh/int80.asm
http://10.71.45.100/bhh/int00.asm
http://10.71.45.100/bhh/int8.asm

7. 缓冲溢出
http://10.71.45.100/bhh/overflow.rar

8. 期末考试重点
http://10.71.45.100/bhh/asm_sum.doc








# 例程

运用rol指令把16位整数转化成16进制格式输出:
http://10.71.45.100/bhh/v2h.asm

运用rol指令把32位整数转化成16进制格式输出:
http://10.71.45.100/bhh/v2h32.asm

引用数组元素的例子: 
http://10.71.45.100/bhh/addr.asm

输入一个字符串再输出的例子: 
http://10.71.45.100/bhh/io.asm

用汇编语言控制文本方式下整个屏幕的输出:
输出2000个'A'的例子: http://10.71.45.100/bhh/2000a.asm
屏幕上移动的'A'的例子: http://10.71.45.100/bhh/mova.asm

http://10.71.45.100/bhh/assume.asm

图形界面编程
http://10.71.45.100/bhh/block.asm
http://10.71.45.100/bhh/cn.asm
http://10.71.45.100/bhh/cnt.asm

堆栈段的定义及使用。例如
http://10.71.45.100/bhh/ss.asm

es表示一个数据段的段址。例如:
http://10.71.45.100/bhh/2seg.asm

例如: http://10.71.45.100/bhh/key.asm

例如: http://10.71.45.100/bhh/readtime.asm
例如: http://10.71.45.100/bhh/music.asm

32位push、pop过程演示:
http://10.71.45.100/bhh/stk1.txt 代码
http://10.71.45.100/bhh/stk2.txt 堆栈布局

代码: http://10.71.45.100/bhh/val2decy.asm

远指针的汇编语言例子:
http://10.71.45.100/bhh/les.asm
近指针的汇编语言例子:
http://10.71.45.100/bhh/nearptr.asm

利用乘法指令把十进制字符串转化成32位整数的例子:
http://10.71.45.100/bhh/dec2v32.asm
利用乘法指令把十进制字符串转化成16位整数的例子:
http://10.71.45.100/bhh/dec2v16.asm


例子: http://10.71.45.100/bhh/xlat.asm
例子: http://10.71.45.100/bhh/xlat_sub.asm

例子: http://10.71.45.100/bhh/float.asm

例:自我移动的代码 http://10.71.45.100/bhh/movcode.asm
例: 修改printf让它做加法运算http://10.71.45.100/bhh/printf.c

例: 演示短跳、近跳、远跳 http://10.71.45.100/bhh/jmp.asm

参数个数可变的函数例子: myprintf.c
http://10.71.45.100/bhh/myprintf.c
5. 混合语言编程
http://10.71.45.100/bhh/asm_c.txt
