#NJU PA Manual 
nops1ed
yingzesun@mail.ustc.edu.cn
 *GB 2312*
***
***
##NJU PA0 

###PRE. 生存指南
- 无

该阶段将完成`ics2023`项目的环境配置工作,你需要做的仅是跟随yzh的脚步完成pa0中的内容

###你也许会遇到： 命令 ```make menuconfig``` 报错 
`menuconfig`将会是我们的有力工具，记得熟悉这位朋友！
如果你从未使用过`menuconfig`,在运行时可能会遇到一些错误，使用`apt`(or some package management)来安装需要的软件，注意查看报错信息

**PA0 Manual 到此结束**

***
***
##NJU PA1

###Note: 谨慎选择库函数
尽量不要使用不安全的库函数:`strcpy`,`scanf` ...等,它们可能具有**潜在的安全性问题**,例如[缓冲区溢出(buffer overflow)](https://en.wikipedia.org/wiki/Buffer_overflow)等,你需要足够重视这个问题

###PRE: 在着手PA1之前,一些必要的生存技能:
 - RTFM
 - RTFSC
 - STFW
 - 对指针,函数指针,[jump table](https://en.wikipedia.org/wiki/Branch_table)等拥有基本了解
 - 正则表达式
 - gdb
 - 了解Makefile的一些基本概念,了解make命令的含义
   - 例如该make命令:`$make -j12 ARCH=native run mainargs=mario`
 - Bash(或其他shell)的一些基本内建命令
 - C99标准
 - Little endian
 - BNF -- 巴克斯-诺尔范式

###I. 为了正确运行**fceux**，你的运行环境可能需要配置**SDL2/SDL.h**

如果你的环境通过apt来管理包,使用 ```sudo apt-get install libsdl2-dev``` 来配置SDL_LIB

*Now Try your Game !*

###II. 使用**ccache**来启用compile cache进而加速make
多核/线程make诚然可节省一些时间,但对于`nemu`来说这仍然不够,我们需要`ccache`
你可以通过`man ccache`来查看ccache对应的manual
配置ccache过程如下：
1. 通过编辑器修改文件`bashrc`,其位于`~/.bashrc`下
2. 追加`export PATH="/usr/lib/ccache:$PATH"`至文件尾
3. 刷新`bashrc`,而不是注销用户并重新登陆,使用`source`指令来实现它:`source ~/.bashrc`
4. 通过`which gcc`来检查你的结果是否正确

###III. Which ISA to choose ？

- 可以选择`x86`,CISC指令种类多且复杂
- 一周目推荐`RISC-V`,RISC指令格式简单且固定,实现难度低
  - 后记：工程实践要求`RISC-V`64位版本
- 谨慎选择`MIPS`
  - 二周目推荐

###IV. (二周目)Reg对计算机的影响
- 是否可以没有寄存器？
    - 是的
- 对现代编程模型什么影响？
    - 参考jyy的os课程

###V. Nemu的规模略大，RTFSC应该从哪里开始呢？
- Nemu的初始化从哪里开始执行(考虑xv6 Kernel曾经的初始化工作)?
    - 路径`./nemu/src/nemu-main.c`

###VI. 测试宏的基本工作方式
1. 首先基本的问题是：宏(Macro)是什么？
- 从高级语言的源代码至可执行文件(例:ELF)经历了如下的流程
    - 由`cpp`预处理源文件形成`*.i`的预处理文件
    - 由`cc`(或`cc1`)完成对预处理文件的编译,并形成`*.s`的汇编文件
    - 由`as`完成对汇编文件`*.s`的处理,产生二进制的可重定向文件`*.o`
    - 由`ld`完成对文件`*.o`的链接,形成可执行文件,在Unix/Unix-like下,其格式为`ELF`,你可以通过`file $FILENAME`来查看文件的`head`内容，其中包含了文件格式
    - 最终由`loader`,将可执行文件加载至内存,改变pc指针,修改上下文,发生控制流转移等等一系列流程,进而执行该程序

- 定义宏将在第一阶段由预处理器负责将宏展开

2. PA中的宏
- 涉及到C99标准中的可选参数宏及粘贴运算符
    - 可参考[@HNU_ZHAO的博客](https://blog.csdn.net/qq_42474104/article/details/129105242)

###VII. 为什么`init_monitor`中都是函数?
- 便于调试，也便于阅读
- 实现接口与实现分离,抽象不同ISA差异
  - 拿`ISA`来举例,`init_monitor`中有函数`init_isa`,对于不同的ISA来说这个函数的实现方式应该是不同的,还记得yzh写的南大pa manual中曾说过："改变你选择的ISA不会对主体框架造成任何影响,你不必修改任何代码吗"?这种设计思维在这里体现了出来

###VIII. 参数从何而来？
于`abstract-machine`中脚本对应的mk文件

###IX. 对于Makefile中错误信息的解决
```c
[src/monitor/monitor.c:20 welcome] Exercise: Please remove me in the source code and compile NEMU again.
riscv32-nemu-interpreter: src/monitor/monitor.c:21: welcome: Assertion `0' failed.
```
可以看到make贴心地为我们报告了错误的信息
定位到该文件的第`36`行,去除yzh为我们设定的断言错误保存后即可返回

###X. 为什么要在cpu_exec()中传入-1?
- 由于补码的规则相当于传入`uint64_max`,使循环次数最大化
    - 要知道`uint64_max`是相当巨大的,我们假定在该循环体内部仅有些许简单语句,即便如此,哪怕是对于今天的个人计算机,执行完这些循环也需要数十年之久,我们并不认为一个合理的算法应该执行如此长的时间,要知道C语言诞生至今也才50年之久
- 是否UB?
    - STFW:查阅C99文档
```
6.3.1.3 Signed and unsigned integers
  Otherwise, if the new type is unsigned, the value is converted by repeatedly adding or subtracting one more than the maximum value that can be
represented in the new type until the value is in the range of the new type. 
```

###XI. 优雅的退出
当在sdb中输入q时nemu退出但会显示以下信息:
```c
make: *** [/home/epic/ics2023/nemu/scripts/native.mk:38: run] Error 1
```
不难理解的是，程序返回结果出现了错误:
  - 在C和C++编程语言中，main函数的返回值（通常称为“退出状态”或“返回状态”）表示程序的执行结果。在大多数系统中，返回值为0表示程序成功执行，而非零值表示出现了某种错误。返回值1通常表示程序遇到了一些问题，但具体的问题可能因程序和上下文而异。
  - 后记：大多数教材对C语言返回值的论述到此戛然而止,但实际上以上论述并不完善，C的返回值是一种具有`8 bytes`结构体存储空间的“状态”，事实上，你总是可以通过很多方法来捕获他,若想获得更多关于`SEGXXX`的信息，STFW
  - 思考一下:为什么main的返回值是`int`类型?真的是`int`类型吗?其他类型不可以吗?返回值返回到了哪里?尝试验证你的想法

追溯到源码：键入`q`后发生了什么？
```c
// ./src/monitor/sdb/sdb.c

    //some code else

static int cmd_q(char *args) {
  return -1;
}

    //some code else

void sdb_mainloop() {
    //some code else

    for (i = 0; i < NR_CMD; i ++) {
      if (strcmp(cmd, cmd_table[i].name) == 0) {
        if (cmd_table[i].handler(args) < 0) { return; }
        break;
      }
    }

    //some code else
}
```
显然的是`cmd_q`——即键入`q`命令的功能是什么都不做返回-1；然而在`sdb_mainloop`中，如果某个命令的`handler`返回值为负数便退出`sdb_mainloop`

让我们在回到主程序中，以上代码均由函数`engine_start()`来完成,当`sdb_mainloop`结束后,`engine_start()`也将随之退出，此时pc将指向最后一条语句，即返回`is_exit_status_bad()`的值
显然的是，此函数的返回值出现了问题
```c
// ./src/nemu-main.c

int main(int argc, char *argv[]) {
  /* Initialize the monitor. */
#ifdef CONFIG_TARGET_AM
  am_init_monitor();
#else
  init_monitor(argc, argv);
#endif

  /* Start engine. */
  engine_start();

  return is_exit_status_bad();
}
```
`is_exit_status_bad()`函数定位于:
```c
// ./src/utils/state.c
NEMUState nemu_state = { .state = NEMU_STOP };

int is_exit_status_bad() {
  int good = (nemu_state.state == NEMU_END && nemu_state.halt_ret == 0) ||
    (nemu_state.state == NEMU_QUIT);
  return !good;
}
```
返回值由good决定，而good的值出现了问题——它由一个"或"表达式来决定自己的值
我们希望`q`代表了退出而不是中断，即置`nemu_state.state`为`NEMU_QUIT`：向`cmd_q`中添加该代码即可完成任务

###XII. SDB的构建
关于命令`si` ,`info` , `x`见如下代码:
```c

// ./src/monitor/sdb/sdb.c

static int cmd_si(char *args)
{	
  //parse args
  char *arg = strtok(NULL," ");
  int i = 0;
  if (!arg)	i = 1;
  else sscanf(arg , "%d" , &i);
  cpu_exec(i);	  
  return 0;
}

static int cmd_info(char *args) {
  //parse args
  char *arg = strtok(NULL , " ");
  if (!strcmp(arg , "r"))
		isa_reg_display();
//  else if 
  else 
    printf("info: Invaild Args\n");
  return 0;
}

static int cmd_x(char *args) {
  char *arg = strtok(NULL , " ");
  int i = 0;
  if (!arg) 
  {
    printf("x: Too few Arguments\n");
    return 0;
  } 
  sscanf(arg , "%d" , &i);
  arg = strtok(NULL , " ");
  if (!arg)
  {
    printf("x: Too few Arguments\n");
    return 0;
  }
  vaddr_t addr = 0;
  sscanf(arg, "%x" , &addr);
  for (int j = 0 ; j < i ; j++)
	{
		printf("0x%x: " , addr + 4 * j);
		for (int k = 3 ; k >= 0 ; k++)
			//Little endian
			printf("%02x " , vaddr_read(addr + 4 * j + k, 1));		
		printf("\n");
	}
  return 0;
}

static struct {
  const char *name;
  const char *description;
  int (*handler) (char *);
} cmd_table [] = {
  { "help", "Display information about all supported commands", cmd_help },
  { "c", "Continue the execution of the program", cmd_c },
  { "q", "Exit NEMU", cmd_q },
  { "si" , "Execute by single step" , cmd_si },
  { "info" , "Show info" , cmd_info },
  { "x" , "Scan Memory" , cmd_x },
  /* TODO: Add more commands */

};
```

###XIII. Sanitizer 检测 use after free
sanitizer会标记malloc内存块，同样的，free内存块也会受到相应的标记，每次访问内存，sanitizer都会检测内存访问合法性，当检测到用户企图访问use after free的内存块后会终止程序并提出警告

###Fin. 代码部分见文件，后续将不会在manual中粘贴代码

**PA1 Manual到此结束**