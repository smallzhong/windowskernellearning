# kernellearning
内核学习笔记

- 001.装环境
    - bcdedit设置双机调试环境
    - 真机驱动开发环境
    - win7x86虚拟机内的开发环境(vs2010)
- 002.保护模式科普
    - 科普了实模式、保护模式
    - 保护模式高2G、低2G
    - 段的科普
- 003.段探测
    - limit, base, address, type, p, g, s
    - 拆GDT表段描述符
- 004.D/B位TYPE位
    - D/B位区分16/32位
    - S=1时的TYPE位表示数据段和代码段的类型
- 005.数据段代码段权限规则
    - CPL, DPL, RPL
    - 跨段跳转不提权(jmp far, call far)
    - 远返回retf
    - 上述指令对栈的影响
    - 数据段，代码段，堆栈段权限总结
- 006.调用门
    - 关闭熔断补丁
    - 调用门描述符
    - 调用门提权，带参数和不带参数
    - call far提权后对栈的影响
    - JMP无法同时修改CS和SS，故无法用于提权
    - RETF无法提权
- 007.中断门
    - sgdt,sidt,lgdt,lidt指令
    - 中断和异常的区别（硬件和软件引起）
    - IDT表
    - 中断描述符
    - 构造中断门提权
    - 中断提权对栈的影响
    - 中断提权retf返回（retf会平衡R3栈，返回前需sti开中断）
- 008.中断门作业讲解，HOOK INT3
    - 陷阱门和中断门的区别（中断门清除IF位，中断悬挂）
        - 进入中断门会清除 efi VM NT IF TF位
        - 进入陷阱门会清除 efi VM NT TF位
        - 由于中断门会清除IF位，导致一些中断触发会悬挂,陷阱门则不会
    - HOOK INT3
        - 指令缓存
        - sti，cli开关中断
- 009.hookint3讲解
    - 发现cli，sti导致的奇怪蓝屏
- 010.TSS
    - 任务和线程的区别
        - 任务是CPU提供的并发机制，线程是操作系统实现的
        - 本质上都是切换执行环境（批量替换寄存器）
    - TR寄存器（task register）
        - selector, base address, limit
        - ltr, str 指令
    - TSS（任务状态段）
    - TSS描述符
        - TSS描述符只存储在GDT表
        - busy位用来防止递归切换任务
        - 任务切换条件：CPL <= TSS.DPL
    - Win7 x86 任务切换后会把 previous task link 的 CR3 清零
    - 使用 iretd 进行任务切换需要把NT位置1
    - JMP/CALL/IRET 对 busy, NT, previous task link 等属性的影响
    - call far 任务段，iretd 返回
    - jmp far 任务段
        - jmp 返回
        - iretd 返回
    - 任务段进R1
        - 在TSS中提供SS1,CS1,FS1的值
        - 提权后不要下断
- 011.任务门_101012环境配置
    - 为什么要学TSS和任务门？8号错误（双重错误）
    - 任务门进R0
        - 在IDT表构造任务门
        - 在GDT表构造TSS描述符
        - 中断调用任务门
    - 101012分页环境配置
- 012.101012拆线性地址
    - 为什么一个页是4KB
    - 拆线性地址找物理地址
    - 页目录项PDE，页表项PTE，页内偏移
    - 科普MMU,TLB,页表缓存，L1一级缓存，二级缓存，三级缓存
