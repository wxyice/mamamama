
# 考试要求

考试题型
1. 填空 涉及计算  20个
2. 简答         3-4个
3. 程序分析      3-4个
4. 编程题  4个


# 重点

1. 单片机系统的基本知识
    1. 基本概念
        记忆
        1. 单片机负责完成从输入设备信号到输出设备动作驱动信号的转换和控制。
        2. 存在单片机的地方
        3. AT89C51
        4. （单片）微型计算机（MP）（MC）：微处理器（CPU）（运算器+控制器）+程序存储器（ROM）+数据存储器（RAM）+I/O输出输出电路+时钟+计算机外围电路
        5. 微处理器（CPU）（MPU）
        6. 单片机（MCU）：Single Chip Microcomputer 将微型计算机的各功能部件微型化并集成到一块集成电路芯片上，也称单片微型计算机、微控制器、嵌入式微控制器。
        7. 1976-1990
        8. 嵌入式系统：以应用技术产品为核心
                     以计算机技术为基础
                     以通信技术为载体
                     以消费类产品为对象
                     引入各种传感器
                     进入Internet网络技术的连接
                     适应应用环境的产品
    2. 特点：
        1. 体积小 使用灵活 成本低 易于产业化
        2. 可靠性好 适应温度范围广
        3. 易扩展
        4. 系统内无监控或系统管理程序
        
    3. 发展趋势
        1. CMOS
        2. 低功耗
        3. 低噪声 高可靠性
        4. 大容量
        5. 高性能
        6. 片内功能丰富
        7. 引脚少

    4. 应用：
        1. 在智能仪表中的应用
        2. 机电一体化
        3. 实时控制
        4. 智能家电
        5. 物联网

    5. 数制
        1. 常用数制（D，B，H）
            基：数制中使用数码的个数
            权：每一位所代表的值
        2. 数制转换
            B<->H
                4位换1位
            B->D/H->D 
                按权转
            D->B/H
                连续除基
                记下余数
                逆序排列
            
    6. 码制
        1. 有符号数
            最高位为“0”表示正数，最高位为“1”表示负数
            有符号数的范围
            -128～+127
            1111 1111 ～ 0111 1111
        2. 机器数
            把数码化的有符号数称为“机器数”，原来的数称为“真值”
            原码
            反码
            补码

            对于正数
                原码=反码=补码
            对于负数
                反码等于原码符号位不变 数值按位取反
                补码=反码+1

            补码连符号位一起取反加1=其相反数的补码
            =》负数的补码变为其绝对值正数
            
        3. 有符号数的运算
            计算机中普遍用补码表示有符号数，因无论是加法还是减法，都可采用加法运算

            求负数的补码可以先写正数原码，减一全取反就可以
            
            正负数全都用补码运算 得到的结果 负数做一次补码运算就是负数原码，正数就是这个数
            负数的补码的补码是自己
        
    7. 编码
        BCD码：
            采用4位二进制数来储存一个十进制的数码，使二进制和十进制之间的转换得以快捷的进行
        压缩BCD码：
            一个BCD码占4位，而一个字节有8位。把2个BCD码放在一个字节中，就叫压缩的BCD码，而一个字节只放一个BCD码，高位置0，则叫非压缩的BCD码
        ASCII码：（美国信息交换标准代码）
            是字母、数字、常用的符号（例如*、#、@等）在计算机中存储时对应的二进制数表示，是人为制定的一套编码
            0～9
            前4位为0011 后4位为0000～1001
    
    8. 存储器
        容量
            bit： 比特 0/1
            Byte：8位
            Word：16位
            1KB=1024bytes 
            1MB=1024KB
            每个单元都有自己唯一的地址
        速度 一般为几十~几百ns

        类型
            ROM 只读
                PROM 可编程
                EPROM 可改写
                EEPROM 电可改写
                FLASH闪存
            RAM
                SRAM
                DRAM
        
        地址和数据
            地址可用8位二进制数表示（寻址256个单元，用于片内RAM），也可用16位表示（最大寻址64K个单元，用于片内和片外程序存储器，以及片外数据存储器寻址）
            
2. 单片机的结构和原理
    - 80C51 结构
        1. 80C51CPU             
        2. ROM 4KB/RAM 256B
        3. 定时/计数器 2个16位   (T0,T1)
        4. 可编程I/O口          (P0,P1,P2,P3)
        5. 振荡器和时序
        6. 中断控制器            (INT0,INT1)
        7. 64KB扩展控制器        (ALE/PROG，PSEN)
        8. 可编程全双工串行口     (TXD,RXD)

    - 80C51CPU结构
        1. 运算器
            1. ALU 算数逻辑单元
            2. ACC (Accumulator) 8位
                参与运算
            3. B 寄存器           8位
                参与乘法 ACC，B 中各放一个8位数 
                乘积的高8位B中 低8位ACC中
            4. 暂存器1 暂存器2
            5. 布尔处理器
            6. PSW (Program Status Word) 8位
                上面有7个反应CPU状态的量
                B7  B6  B5  B4  B3  B2  B1  B0 
                CY  AC  F0  RS1 RS0 OV  保留 P

                CY:进位标志 无符号数溢出
                AC:半进位标志
                OV:溢出标志 有符号数溢出
                P: 奇偶标志 ACC中1的个数奇数P=1

                RS1 RS2 :工作组选择

        2. 控制器
            1. 定时控制逻辑
            2. PC   (Program Counter) 16位
                存放下一条指令的地址
                物理上独立 用户无法直接访问 
            3. 指令寄存器
            4. 指令译码器
            5. DPTR (Data Pointer Register)
            6. 信息传送控制部件

    - 存储机制
        1. 内部RAM
            80C51 有256个字节的RAM 前128个用户RAM
            后128 特殊功能寄存器 SFR 用了21个
        2. 内部ROM
            80C31内部无ROM
            80C32内部无ROM

            80C51内部共有4KB的ROM
            87C51内部共有4KB的EPROM
            89C51内部共有4KB的Flash

            80C52内部共有8KB的ROM
            87C52内部共有8KB的EPROM
            89C52内部共有8KB的Flash
    - 并行I/O
        80C51内部有4个8位I/O口P0, P1, P2, P3
    - 串行I/O口
        80C51内部有1个全双工、可编程串行口
        全双工：任意时刻均可同时收、发
        半双工：同一时刻只能收或发
        单工：只能收或只能发

    - 时钟
        80C51内部有时钟电路，但晶振和微调电容需外接
    - 定时/计数
        80C51内部有2个16位定时/计数器
    - 中断
        80C51共有5个中断源：
            2个外部中断源
            2个定时/计数器中断源
            1个串行口中断源
    - 80C51 引脚
        1. 电源VCC 和VSS（GND）
        2. 时钟电路XTAL1 和XTAL2
        3. I/O口引脚P0，P1，P2，P3口，均可作为通用I/O口

        第二功能
        1. 地址总线低8位，数据总线
        2. 地址总线高8位
        3.  RXD:串行接收,
            TXD:串行发送；
            INT0，INT1:外部中断0，1；
            T0，T1:外部计数输入0，1；
            WR:外部RAM写选通；
            RD:外部RAM读选通（这里有问题）
        4. 控制信号
            RST－2个机器周期（24个振荡周期）高电平时复位
            EA(External Address Enable) 
                EA=0，全部使用外部ROM 
                EA=1，低地址使用内部ROM
            ALE(Address Latch Enable)地址锁存允许（接外部锁存器EN端）
                ALE=1, P0口的内容为地址低8位
            PSEN(Program Store Enable)外部ROM读选通
    
    - 计算机结构
        - 哈佛结构
            - 程序和数据存储器有各自的寻址方法 寻址空间 控制系统
            80C51用哈佛结构

        - 冯诺伊曼结构
            - 将指令、数据存储在同一存储器中并统一编址，依靠指令计数器提供的地址来区分指令和数据。数据吞吐率低，但数据线少。
        
    - 程序寄存器
        - 0000H－复位后程序的入口地址
        - 0003H－外部中断0中断服务程序入口地址
        - 000BH－定时/计数器0中断服务程序入口地址
        - 0013H－外部中断1中断服务程序入口地址
        - 001BH－定时/计数器1中断服务程序入口地址
        - 0023H－串行口中断服务程序入口地址
    
        内部4K   
            当EA/VPP=1 用
            当EA/VPP=0 不用内部
        
    - 数据存储器
        - 内部RAM
            前128字节 用户
                工作寄存器 8个字节 00H~17H 
                    R0～R7 共8个 4组
                    0,1,2,3
                位寻址区          20H~2FH
                    SETB CLR 
                    16*8=128
                    每一位用00H-7FH

                开放区            30H~7FH 

                
            后128字节 特殊寄存器SFR PSW ACC
            
        - 外部64K
            I/O 
    
    - 时钟与时序
        - 80C51单片机具有内部时钟电路，只需外界晶振
        - 1状态（时钟）周期＝2振荡周期
        - 1机器周期＝6状态周期=12振荡周期
        - 1指令周期＝1或2或4个机器周期（无3周期）

        晶振    振荡周期   状态周期   机器周期  指令周期
        6MHz   1/6μs     1/3μs     2μs      2~8μs
        12MHz  1/12μs    1/6μs     1μs      1~4μs

        
    
        
3. 80C51的指令系统

4. 汇编语言程序设计

5. 定时器计数器