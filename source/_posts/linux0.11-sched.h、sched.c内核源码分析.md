---
title: linux0.11-sched.h、sched.c内核源码分析
date: 2019-12-12 18:05:00
tags:
    - UNIX
    - linux
    - 源码
    - 进程调度
categories:
    - 操作系统
mathjax: true
---

这篇文章 只是学习笔记，如有错误或疑问，欢迎指出。
```c
#ifndef _SCHED_H
#define _SCHED_H

#define NR_TASKS 64  //系统最多的进程数
#define HZ 100       // 系统时钟频率 100HZ

#define FIRST_TASK task[0]      //任务0是  比较特殊 init()
#define LAST_TASK task[NR_TASKS-1]  //任务数组里面最后一个


#include <linux/head.h>
#include <linux/fs.h>
#include <linux/mm.h>
#include <signal.h>

#if (NR_OPEN > 32)
#error "Currently the close-on-exec-flags are in one word, max 32 files/proc"
#endif


//前面说过的 几种状态  当前版本只有这么几个
#define TASK_RUNNING		0
#define TASK_INTERRUPTIBLE	1
#define TASK_UNINTERRUPTIBLE	2
#define TASK_ZOMBIE		3
#define TASK_STOPPED		4

#ifndef NULL
#define NULL ((void *) 0)
#endif
// 几个具体的函数  后面会在sched.c 里面会讲实现

// 复制 进程 目录页表
extern int copy_page_tables(unsigned long from, unsigned long to, long size);
//释放页表所指定的 内存 和页表本身
extern int free_page_tables(unsigned long from, unsigned long size);

//初始化调度
extern void sched_init(void);
//调度程序
extern void schedule(void);

//异常中断处理 ，设置中断调用门，并开启中断
extern void trap_init(void);

//显示出错信息，然后死循环
extern void panic(const char * str);

//在 tty 上显示制定长度信息
extern int tty_write(unsigned minor,char * buf,int count);

// 定义一个函数指针类型
typedef int (*fn_ptr)();

//这个暂时不用管  是数学处理器使用的结构，我也不知道具体是干啥的
struct i387_struct {
	long	cwd;
	long	swd;
	long	twd;
	long	fip;
	long	fcs;
	long	foo;
	long	fos;
	long	st_space[20];	/* 8*10 bytes for each FP-reg = 80 bytes */
};
// 任务状态段数据结构 也不知道干啥的
struct tss_struct {
	long	back_link;	/* 16 high bits zero */
	long	esp0;
	long	ss0;		/* 16 high bits zero */
	long	esp1;
	long	ss1;		/* 16 high bits zero */
	long	esp2;
	long	ss2;		/* 16 high bits zero */
	long	cr3;
	long	eip;
	long	eflags;
	long	eax,ecx,edx,ebx;
	long	esp;
	long	ebp;
	long	esi;
	long	edi;
	long	es;		/* 16 high bits zero */
	long	cs;		/* 16 high bits zero */
	long	ss;		/* 16 high bits zero */
	long	ds;		/* 16 high bits zero */
	long	fs;		/* 16 high bits zero */
	long	gs;		/* 16 high bits zero */
	long	ldt;		/* 16 high bits zero */
	long	trace_bitmap;	/* bits: trace 0, bitmap 16-31 */
	struct i387_struct i387;
};


//1ong 1eader会话首领。
//long start_time进程开始运行时刻。
//unsigned short used_math 标志：是否使用了协处理器。
//int tty进程使用tty的子设备号。-1表示没有使用。//unsigned short umask文件创建属性屏蔽位。
//struct minode*pwd 当前工作目录i节点结构。//struct m inode*root 根目录i节点结构。
//struct m inode*executable执行文件i节点结构。
//unsigned 1ong close_on_exec执行时关闭文件句柄位图标志。（参见include/fcntl.h）
//struct file*filp[NR_OPEN]进程使用的文件表结构。
//struct desc_struct 1dt[3]本任务的局部表描述符。0-空，1-代码段cs，2-数据和堆栈段ds&ss。

// 重头戏，也就是我们说的 PCB
struct task_struct {
/* these are hardcoded - don't touch */
	long state;	/* -1 unrunnable, 0 runnable, >0 stopped */
	long counter;
	long priority;
	long signal;

	//信号执行属性结构，对应信号将要执行的操作
	struct sigaction sigaction[32];
	// 进程信号屏蔽码，计算机组成原理上面 有详细解释
	long blocked;	/* bitmap of masked signals */
/* various fields */
    // 任务执行退出码，给父进程用的
	int exit_code;
	//代码段信息 开始 结束  数据段信息 总长度 堆栈地址
	unsigned long start_code,end_code,end_data,brk,start_stack;
	// 自己 父亲（新版本中 懂事直接用 父亲指针 指向他的PCB） 组号，会话号，会话首领
	long pid,father,pgrp,session,leader;
    //unsigned short uid 用户标识号（用户id）。
//euid 有效用户id。suid 保存的用户id。
// gid组标识号（组id）egid有效组id。sgid保存的组id。
    unsigned short uid,euid,suid;
	unsigned short gid,egid,sgid;
//1ong alarm报警定时值（滴答数）。
	long alarm;
//1ong utime用户态运行时间（滴答数）。
//long stime系统态运行时间（滴答数）。
//1ong cutime子进程用户态运行时间。
// 1ong cstime子进程系统态运行时间。
// start_time 开始运行时间
	long utime,stime,cutime,cstime,start_time;
	// 是否使用了协处理器
	unsigned short used_math;
/* file system info */
// tty 设备 -1 表示没有用
	int tty;		/* -1 if no tty, so it must be signed */
	// 文件穿件属性屏蔽位
	unsigned short umask;
	// 当前工作目录i 节点结构
	struct m_inode * pwd;
	// 更目录 i 节点结构
	struct m_inode * root;
	// 执行文件 i 节点结构
	struct m_inode * executable;
	// 关闭文件句柄位 图标志
	unsigned long close_on_exec;
	// 进程使用的文件表 结构
	struct file * filp[NR_OPEN];
/* ldt for this task 0 - zero 1 - cs 2 - ds&ss */
// 任务局部表描述符，  0 空  1代码段 2 数据和堆栈段 ds &ss
struct desc_struct ldt[3];
/* tss for this task */
    //  本进程的任务状态信息结构
	struct tss_struct tss;
};

/*
 *  INIT_TASK is used to set up the first task table, touch at
 * your own risk!. Base=0, limit=0x9ffff (=640kB)
 */

// 这个 用于 设置第一个任务表   别动就行
#define INIT_TASK \
/* state etc */	{ 0,15,15, \
/* signals */	0,{{},},0, \
/* ec,brk... */	0,0,0,0,0,0, \
/* pid etc.. */	0,-1,0,0,0, \
/* uid etc */	0,0,0,0,0,0, \
/* alarm */	0,0,0,0,0,0, \
/* math */	0, \
/* fs info */	-1,0022,NULL,NULL,NULL,0, \
/* filp */	{NULL,}, \
	{ \
		{0,0}, \
/* ldt */	{0x9f,0xc0fa00}, \
		{0x9f,0xc0f200}, \
	}, \
/*tss*/	{0,PAGE_SIZE+(long)&init_task,0x10,0,0,0,0,(long)&pg_dir,\
	 0,0,0,0,0,0,0,0, \
	 0,0,0x17,0x17,0x17,0x17,0x17,0x17, \
	 _LDT(0),0x80000000, \
		{} \
	}, \
}

//任务指针数组
extern struct task_struct *task[NR_TASKS];
    //上一个使用过 协处理器的进程
extern struct task_struct *last_task_used_math;
//当前进程
extern struct task_struct *current;
//开机时开始技术滴答数  10ms/滴答
extern long volatile jiffies;
//开机时间。从 1970:0:0:0 开始计时  秒为单位
extern long startup_time;

//当前时间
#define CURRENT_TIME (startup_time+jiffies/HZ)

// 添加定时器 到时间执行函数
extern void add_timer(long jiffies, void (*fn)(void));

// 不可中断的等待睡眠
extern void sleep_on(struct task_struct ** p);
// 可中断的
extern void interruptible_sleep_on(struct task_struct ** p);
//唤醒程序
extern void wake_up(struct task_struct ** p);

/*
 * Entry into gdt where to find first TSS. 0-nul, 1-cs, 2-ds, 3-syscall
 * 4-TSS0, 5-LDT0, 6-TSS1 etc ...
 */
//
#define FIRST_TSS_ENTRY 4
#define FIRST_LDT_ENTRY (FIRST_TSS_ENTRY+1)
#define _TSS(n) ((((unsigned long) n)<<4)+(FIRST_TSS_ENTRY<<3))
#define _LDT(n) ((((unsigned long) n)<<4)+(FIRST_LDT_ENTRY<<3))
#define ltr(n) __asm__("ltr %%ax"::"a" (_TSS(n)))
#define lldt(n) __asm__("lldt %%ax"::"a" (_LDT(n)))
#define str(n) \
__asm__("str %%ax\n\t" \
	"subl %2,%%eax\n\t" \
	"shrl $4,%%eax" \
	:"=a" (n) \
	:"a" (0),"i" (FIRST_TSS_ENTRY<<3))
/*
 *	switch_to(n) should switch tasks to task nr n, first
 * checking that n isn't the current task, in which case it does nothing.
 * This also clears the TS-flag if the task we switched to has used
 * tha math co-processor latest.
 */
//这是一个很重要的跳转 ，用于上下文切换
/*switch_to（n）将切换当前任务到任务nr，即n。首先检测任务n不是当前任务，
*如果是则什么也不做退出。如果我们切换到的任务最近（上次运行）使用过数学
        *协处理器的话，则还需复位控制寄存器cr0中的TS标志。
*/

//跳转到一个任务的TSS段选择符组成的地址处会造成CPU进行任务切换操作。//输入：%0-指向tmp；%1-指向tmp.b处，用于存放新TSS的选择符；
//dx-新任务n的TSS段选择符；ecx-新任务n的任务结构指针task[n]。
//其中临时数据结构tmp用 跳转（farjump）指令的操作数。该操作数由4字节偏移
//地址和2字节的段选择符组成。因此tmp中a的值是32位偏移值，而b的低2字节是新TSS段的
//选择符（高2字节不用）。跳转到TSS段选择符会造成任务切换到该TSS对应的进程。对于造成任务//切换的长跳转，a值无用。177行上的内存间接跳转指令使用6字节操作数作为跳转目的地的长指针，
//其格式为：jmp16位段选择符：32位偏移值。但在内存中操作数的表示顺序与这里正好相反。//任务切换回来之后，在判断原任务上次执行是否使用过协处理器时，是通过将原任务指针与保存在
//last_task used math变量中的上次使用过协处理器任务指针进行比较而作出的，参见文件
//kernel/sched.c中有关math state restore）函数的说明。
#define switch_to(n) {\
struct {long a,b;} __tmp; \
/*
 * __asm__("cmpl %%ecx,_current\n\t" \          比较是不是当前运行的进程
	"je 1f\n\t" \                           是的话啥都不做
	"movw %%dx,%1\n\t" \                将新任务TSS 的16位 选择符存入 __tmp.b中
	"xchgl %%ecx,_current\n\t" \            //current=task[n] ecx=被换出的任务
	"ljmp %0\n\t" \                     跳到 *&__tmp,任务切换   这个时候已经不在这里了

必须要等到那个任务  执行完后 重新回到这里

	"cmpl %%ecx,_last_task_used_math\n\t" \   //原任务使用过协处理器吗
	"jne 1f\n\t" \                              //没有则跳转 退出
	"clts\n" \                                  //使用过，清楚cr0任务切换
	"1:" \                                      //标志 TS
	::"m" (*&__tmp.a),"m" (*&__tmp.b), \
	"d" (_TSS(n)),"c" ((long) task[n])); \
}

 * */
__asm__("cmpl %%ecx,_current\n\t" \
	"je 1f\n\t" \
	"movw %%dx,%1\n\t" \
	"xchgl %%ecx,_current\n\t" \
	"ljmp %0\n\t" \
	"cmpl %%ecx,_last_task_used_math\n\t" \
	"jne 1f\n\t" \
	"clts\n" \
	"1:" \
	::"m" (*&__tmp.a),"m" (*&__tmp.b), \
	"d" (_TSS(n)),"c" ((long) task[n])); \
}

#define PAGE_ALIGN(n) (((n)+0xfff)&0xfffff000)

#define _set_base(addr,base) \
__asm__("movw %%dx,%0\n\t" \
	"rorl $16,%%edx\n\t" \
	"movb %%dl,%1\n\t" \
	"movb %%dh,%2" \
	::"m" (*((addr)+2)), \
	  "m" (*((addr)+4)), \
	  "m" (*((addr)+7)), \
	  "d" (base) \
	:"dx")

#define _set_limit(addr,limit) \
__asm__("movw %%dx,%0\n\t" \
	"rorl $16,%%edx\n\t" \
	"movb %1,%%dh\n\t" \
	"andb $0xf0,%%dh\n\t" \
	"orb %%dh,%%dl\n\t" \
	"movb %%dl,%1" \
	::"m" (*(addr)), \
	  "m" (*((addr)+6)), \
	  "d" (limit) \
	:"dx")

#define set_base(ldt,base) _set_base( ((char *)&(ldt)) , base )
#define set_limit(ldt,limit) _set_limit( ((char *)&(ldt)) , (limit-1)>>12 )

#define _get_base(addr) ({\
unsigned long __base; \
__asm__("movb %3,%%dh\n\t" \
	"movb %2,%%dl\n\t" \
	"shll $16,%%edx\n\t" \
	"movw %1,%%dx" \
	:"=d" (__base) \
	:"m" (*((addr)+2)), \
	 "m" (*((addr)+4)), \
	 "m" (*((addr)+7))); \
__base;})

#define get_base(ldt) _get_base( ((char *)&(ldt)) )

#define get_limit(segment) ({ \
unsigned long __limit; \
__asm__("lsll %1,%0\n\tincl %0":"=r" (__limit):"r" (segment)); \
__limit;})

#endif

```



![在这里插入图片描述](https://img-blog.csdnimg.cn/2019121215481494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20191212154926724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

关于 sleep_on    wait。  
这是两个 帮助 理解 的图片 
重点 是 sleep  可wake
注意 进程 本身代码段  和PCB  不是 同一个东西。

sleep_on 中 调用 了 schedule();
等到 他返回的时候 ，你换这个进程已经唤醒了，也就是 说 ，当前进程唤醒之后，实际上还在sleep on() 里面.

wake() 只是 把 PCB 调入了唤醒状态 ，实际上 ，代码段 并没有执行，他还需要等待调度，wake() 只是修改了状态，并不是立马调度。

```c
/*
 *  linux/kernel/sched.c
 *
 *  (C) 1991  Linus Torvalds
 */

/*
 * 'sched.c' is the main kernel file. It contains scheduling primitives
 * (sleep_on, wakeup, schedule etc) as well as a number of simple system
 * call functions (type getpid(), which just extracts a field from
 * current-task
 */
#include <linux/sched.h>
#include <linux/kernel.h>
#include <linux/sys.h>
#include <linux/fdreg.h>
#include <asm/system.h>
#include <asm/io.h>
#include <asm/segment.h>

#include <signal.h>
// 读取 nr 在信号位图中  对应的二进制数值
#define _S(nr) (1<<((nr)-1))
// 除了 下面两个 信号 其他都是可阻塞的信号
#define _BLOCKABLE (~(_S(SIGKILL) | _S(SIGSTOP)))

//内核调试函数 不用详细解释了把
void show_task(int nr,struct task_struct * p)
{
	int i,j = 4096-sizeof(struct task_struct);
// 进程Nr 的 进程号  状态 以及 堆栈空闲字节数
	printk("%d: pid=%d, state=%d, ",nr,p->pid,p->state);
	i=0;
	while (i<j && !((char *)(p+1))[i])//检查指定任务结构以后等于 0的字节数
		i++;
	printk("%d (of %d) chars free in kernel stack\n\r",i,j);
}

//调用前面那个函数 ，非空进程就打印
void show_stat(void)
{
	int i;

	for (i=0;i<NR_TASKS;i++)
		if (task[i])
			show_task(i,task[i]);
}

//频率 / HZ
#define LATCH (1193180/HZ)

extern void mem_use(void);

//时钟中断处理 具体见 system_call.s
extern int timer_interrupt(void);
//系统 中断处理 具体见 system_call.s
extern int system_call(void);

// 任务堆栈段  ，任务内核态堆栈结构
union task_union {//任务联合，任务结构体 和stack字符数组成员

	struct task_struct task;  //任务数据结构和内核态堆栈 在同意内存页
	char stack[PAGE_SIZE];   //所以 ss 可以或得其数据段选择符号
};

//初始化任务结构  就是第一个进程任务
static union task_union init_task = {INIT_TASK,};
//从开机开始算起的滴答数时间值全局变量（10ms/滴答）。系统时钟中断每发生一次即一个滴答。//前面的限定符volatile，英文解释是易改变的、不稳定的意思。这个限定词的含义是向编译器
//指明变量的内容可能会由于被其他程序修改而变化。通常在程序中中明一个变量时，编译器会
//尽量把它存放在通用寄存器中，例如ebx，以提高访问效率。当CPU把其值放到ebx中后一般
//就不会再关心该变量对应内存位置中的内容。若此时其他程序（例如内核程序或一个中断过程）
//修改了内存中该变量的值，ebx中的值并不会随之更新。为了解决这种情况就创建了volatile
//限定符，让代码在引用该变量时一定要从指定内存位置中取得其值。这里即是要求gcc不要对
//jiffies 进行优化处理，也不要挪动位置，并且需要从内存中取其值。因为时钟中断处理过程
//等程序会修改它的值。

long volatile jiffies=0;

//讲过了  开机时间
long startup_time=0;
// 当前任务   一开始就是第一个任务  现在是init()程序
struct task_struct *current = &(init_task.task);

//使用过协处理器任务指针
struct task_struct *last_task_used_math = NULL;

//任务列表，一开始 就是 init（）
struct task_struct * task[NR_TASKS] = {&(init_task.task), };

// 用户堆栈
//定义用户堆栈，共1K项，容量4K字节。在内核初始化操作过程中被用作内核栈，初始化完成
//以后将被用作任务0的用户态堆栈。在运行任务0之前它是内核栈，以后用作任务0和1的用
//户态栈。下面结构用于设置堆栈ss:esp（数据段选择符，指针），见head.s，第23行。
//ss被设置为内核数据段选择符（0x10），指针esp指在user_stack数组最后一项后面。这是
//因为IntelCPU执行堆栈操作时是先递减堆栈指针sp值，然后在sp指针处保存入栈内容。
long user_stack [ PAGE_SIZE>>2 ] ;

struct {
	long * a;
	short b;
	} stack_start = { & user_stack [PAGE_SIZE>>2] , 0x10 };
/*
 *  'math_state_restore()' saves the current math information in the
 * old math state array, and gets the new ones from the current task
 */
/*
 * 将党建内容 保存 到 老处理器状态数组中
 * 并回将当前任务的协处理器内容加载到协处理器中
 * */



//此函数用于上下文切换  ，  当任务 被调度交换以后，该函数用以保存协处理器状态（上下文）
// 并恢复新的调度进来的当前任务的协处理器转台
void math_state_restore()
{
    // 任务没有改变 就直接返回
	if (last_task_used_math == current)
		return;
	//在发送协处理器命令之前要发 wait 指令，如果上一个任务使用了协处理器 就要保存
	__asm__("fwait");
	if (last_task_used_math) {
		__asm__("fnsave %0"::"m" (last_task_used_math->tss.i387));
	}
	//协处理器指向自己
	last_task_used_math=current;

	//如果第一用 就初始化 协处理器 否则 直接回复状态
	if (current->used_math) {
		__asm__("frstor %0"::"m" (current->tss.i387));
	} else {
		__asm__("fninit"::);//像协处理器发送初始化命令
		current->used_math=1;//表示自己使用协处理器
	}
}

/*
 *  'schedule()' is the scheduler function. This is GOOD CODE! There
 * probably won't be any reason to change this, as it should work well
 * in all circumstances (ie gives IO-bound processes good response etc).
 * The one thing you might take a look at is the signal-handler code here.
 *
 *   NOTE!!  Task 0 is the 'idle' task, which gets called when no other
 * tasks can run. It can not be killed, and it cannot sleep. The 'state'
 * information in task[0] is never used.
 */
/*
 * 上面告诉 你 schedule 是个很好的东西 没有要改的必要，任何环境下都能工作。
 * 你只需要注意信号处理代码即可
 *
 * 注意!! 任务0 比是一个闲置任务，只有没有其他任务运行的时候你才能调用 ，
 * 他不能杀死 也不能睡眠，他的state 从来不用。
 * */

void schedule(void)
{
	int i,next,c;
	struct task_struct ** p; //指向任务结构指针的指针

/* check alarm, wake up any interruptible tasks that have got a signal */
/*  检测到alarm（进程的报警定时器），唤醒任何已经得到信号的可中断任务*/
/

//从后往前遍历 任务，跳过 空指针
	for(p = &LAST_TASK ; p > &FIRST_TASK ; --p)
		if (*p) {
		    // 如果设置了警报，且经过 过期，就设置signal然后清除alarm
			if ((*p)->alarm && (*p)->alarm < jiffies) {
					(*p)->signal |= (1<<(SIGALRM-1));
					(*p)->alarm = 0;
				}
			//如果信号位图中被阻塞的信号外还有其他信号，并且任务处于可中断转台，酒吧任务变为就绪
			// ~(_BLOCKABLE & (*p)->blocked))用于忽略阻塞信号
			if (((*p)->signal & ~(_BLOCKABLE & (*p)->blocked)) &&
			(*p)->state==TASK_INTERRUPTIBLE)
				(*p)->state=TASK_RUNNING;
		}

/* this is the scheduler proper: */
    //调度的核心

	while (1) {
		c = -1;
		next = 0;
		i = NR_TASKS;
		p = &task[NR_TASKS];
		while (--i) {
		    //如果是空指针就跳过
			if (!*--p)
				continue;
			//比较所有 就绪状态 counter 的大小，找到最大的
			if ((*p)->state == TASK_RUNNING && (*p)->counter > c)
				c = (*p)->counter, next = i;
		}
		//如果找到了就跳出去
		if (c) break;
        // 如果最大的counter = 0  就更新 counter ，就是下面这个算法  ，就是当前值 /2  +priority ......好敷衍的感觉
		for(p = &LAST_TASK ; p > &FIRST_TASK ; --p)
			if (*p)
				(*p)->counter = ((*p)->counter >> 1) +
						(*p)->priority;
	}
	//下面这个就是跳转执行  如果next=0  实际上又回到了这个函数
	switch_to(next);
}

//暂停当前进程，调度下一个
//调用当前函数 会使当前 进程进入睡眠 直到 收到一个信号
int sys_pause(void)
{
	current->state = TASK_INTERRUPTIBLE;
	schedule();
	return 0;
}

/*
 * 函数 将当前 进程 睡眠 等待wakeup
 *
 * */

// **p 是 等待队列头指针的指针
void sleep_on(struct task_struct **p)
{

	struct task_struct *tmp;
	//如果等待队列是空指针 则 无效
	if (!p)
		return;
    // 如果 当前是任务 0  就死机了
	if (current == &(init_task.task))
		panic("task[0] trying to sleep");

	tmp = *p;
	*p = current;
	//把当前状态 阻塞，直到调用wakeup
	current->state = TASK_UNINTERRUPTIBLE;
	//调度
	schedule();

	//当调度 完成之后 重新唤醒这个进程 代码在这继续执行

	if (tmp)        //如果后面还有进程就把 下一个也唤醒
		tmp->state=0;
}

/*
 * 举个例子 ，如果有误欢迎指出
 *
 * A sleep  调用 B  然后再  sleep 调用 C 然后C调用wake 唤醒了B ，如果A  B 在同一个队列 ，B 也会唤醒A
 *
 * */

void interruptible_sleep_on(struct task_struct **p)
{
	struct task_struct *tmp;

	if (!p)
		return;
	if (current == &(init_task.task))
		panic("task[0] trying to sleep");
	tmp=*p;
	*p=current;
	//前面都是一样的..
	//设置当前中断等待状态
repeat:	current->state = TASK_INTERRUPTIBLE;
	schedule();

	//如果当前执行的 进程 并不是 当前等待进程队列的第一个 那么 就把他重新丢回去，再调度一次
	if (*p && *p != current) {
		(**p).state=0;
		goto repeat;
	}
	*p=NULL;
	if (tmp)
		tmp->state=0;
}

// 直接唤醒

void wake_up(struct task_struct **p)
{
	if (p && *p) {
		(**p).state=0;
		*p=NULL;
	}
}


/*
 * 后面的代码  有兴趣的可以自己搜索 理解一下...
 *
 * */

/*
 * OK, here are some floppy things that shouldn't be in the kernel
 * proper. They are here because the floppy needs a timer, and this
 * was the easiest way of doing it.
 */
static struct task_struct * wait_motor[4] = {NULL,NULL,NULL,NULL};
static int  mon_timer[4]={0,0,0,0};
static int moff_timer[4]={0,0,0,0};
unsigned char current_DOR = 0x0C;

int ticks_to_floppy_on(unsigned int nr)
{
	extern unsigned char selected;
	unsigned char mask = 0x10 << nr;

	if (nr>3)
		panic("floppy_on: nr>3");
	moff_timer[nr]=10000;		/* 100 s = very big :-) */
	cli();				/* use floppy_off to turn it off */
	mask |= current_DOR;
	if (!selected) {
		mask &= 0xFC;
		mask |= nr;
	}
	if (mask != current_DOR) {
		outb(mask,FD_DOR);
		if ((mask ^ current_DOR) & 0xf0)
			mon_timer[nr] = HZ/2;
		else if (mon_timer[nr] < 2)
			mon_timer[nr] = 2;
		current_DOR = mask;
	}
	sti();
	return mon_timer[nr];
}

//等待制定软驱马达启动一段时间然后返回。
void floppy_on(unsigned int nr)
{
	cli();  //这个是禁止中断
	while (ticks_to_floppy_on(nr))
		sleep_on(nr+wait_motor);
	sti();
}
//置关闭相应 的软驱马达停转  3s  不设置 是 100s
void floppy_off(unsigned int nr)
{
	moff_timer[nr]=3*HZ;
}
//软盘定时处理子程序
void do_floppy_timer(void)
{
	int i;
	unsigned char mask = 0x10;

	for (i=0 ; i<4 ; i++,mask <<= 1) {
		if (!(mask & current_DOR))
			continue;
		if (mon_timer[i]) {
			if (!--mon_timer[i])
				wake_up(i+wait_motor);
		} else if (!moff_timer[i]) {
			current_DOR &= ~mask;
			outb(current_DOR,FD_DOR);
		} else
			moff_timer[i]--;
	}
}

#define TIME_REQUESTS 64
// 定时器链表结构和定时器数组
static struct timer_list {
	long jiffies;
	void (*fn)();
	struct timer_list * next;
} timer_list[TIME_REQUESTS], * next_timer = NULL;
//  添加定时器输入时间 和相应的处理函数指针
void add_timer(long jiffies, void (*fn)(void))
{
	struct timer_list * p;

	if (!fn)
		return;
	cli();
	if (jiffies <= 0)
		(fn)();
	else {
		for (p = timer_list ; p < timer_list + TIME_REQUESTS ; p++)
			if (!p->fn)
				break;
		if (p >= timer_list + TIME_REQUESTS)
			panic("No more time requests free");
		p->fn = fn;
		p->jiffies = jiffies;
		p->next = next_timer;
		next_timer = p;
		while (p->next && p->next->jiffies < p->jiffies) {
			p->jiffies -= p->next->jiffies;
			fn = p->fn;
			p->fn = p->next->fn;
			p->next->fn = fn;
			jiffies = p->jiffies;
			p->jiffies = p->next->jiffies;
			p->next->jiffies = jiffies;
			p = p->next;
		}
	}
	sti();
}
//时间中断处理函数
void do_timer(long cpl)
{
	extern int beepcount;
	extern void sysbeepstop(void);

	if (beepcount)
		if (!--beepcount)
			sysbeepstop();

	if (cpl)
		current->utime++;
	else
		current->stime++;

	if (next_timer) {
		next_timer->jiffies--;
		while (next_timer && next_timer->jiffies <= 0) {
			void (*fn)(void);
			
			fn = next_timer->fn;
			next_timer->fn = NULL;
			next_timer = next_timer->next;
			(fn)();
		}
	}
	if (current_DOR & 0xf0)
		do_floppy_timer();
	if ((--current->counter)>0) return;
	current->counter=0;
	if (!cpl) return;
	schedule();
}


// 这个是设置报警时间（秒）
//seconds 大于0  设置新时间 并返回 老时间还差多少秒。否则返回 0
int sys_alarm(long seconds)
{
	int old = current->alarm;

	if (old)
		old = (old - jiffies) / HZ;
	current->alarm = (seconds>0)?(jiffies+HZ*seconds):0;
	return (old);
}

//后面这是一大堆获取当前的一些值的函数
int sys_getpid(void)
{
	return current->pid;
}

int sys_getppid(void)
{
	return current->father;
}

int sys_getuid(void)
{
	return current->uid;
}

int sys_geteuid(void)
{
	return current->euid;
}

int sys_getgid(void)
{
	return current->gid;
}
int sys_getegid(void)
{
	return current->egid;
}
//降低优先权
int sys_nice(long increment)
{
	if (current->priority-increment>0)
		current->priority -= increment;
	return 0;
}
// 这是一个内核初始化程序
void sched_init(void)
{
	int i;
	struct desc_struct * p;

	if (sizeof(struct sigaction) != 16)
		panic("Struct sigaction MUST be 16 bytes");
	set_tss_desc(gdt+FIRST_TSS_ENTRY,&(init_task.task.tss));
	set_ldt_desc(gdt+FIRST_LDT_ENTRY,&(init_task.task.ldt));
	p = gdt+2+FIRST_TSS_ENTRY;
	for(i=1;i<NR_TASKS;i++) {
		task[i] = NULL;
		p->a=p->b=0;
		p++;
		p->a=p->b=0;
		p++;
	}
/* Clear NT, so that we won't have troubles with that later on */
	__asm__("pushfl ; andl $0xffffbfff,(%esp) ; popfl");
	ltr(0);
	lldt(0);
	outb_p(0x36,0x43);		/* binary, mode 3, LSB/MSB, ch 0 */
	outb_p(LATCH & 0xff , 0x40);	/* LSB */
	outb(LATCH >> 8 , 0x40);	/* MSB */
	set_intr_gate(0x20,&timer_interrupt);
	outb(inb_p(0x21)&~0x01,0x21);
	set_system_gate(0x80,&system_call);
}

```