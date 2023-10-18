#NJU PA Manual 
nops1ed
yingzesun@mail.ustc.edu.cn
 *GB 2312*
***
***
##NJU PA0 

###PRE. ����ָ��
- ��

�ý׶ν����`ics2023`��Ŀ�Ļ������ù���,����Ҫ���Ľ��Ǹ���yzh�ĽŲ����pa0�е�����

###��Ҳ��������� ���� ```make menuconfig``` ���� 
`menuconfig`���������ǵ��������ߣ��ǵ���Ϥ��λ���ѣ�
������δʹ�ù�`menuconfig`,������ʱ���ܻ�����һЩ����ʹ��`apt`(or some package management)����װ��Ҫ�������ע��鿴������Ϣ

**PA0 Manual ���˽���**

***
***
##NJU PA1

###Note: ����ѡ��⺯��
������Ҫʹ�ò���ȫ�Ŀ⺯��:`strcpy`,`scanf` ...��,���ǿ��ܾ���**Ǳ�ڵİ�ȫ������**,����[���������(buffer overflow)](https://en.wikipedia.org/wiki/Buffer_overflow)��,����Ҫ�㹻�����������

###PRE: ������PA1֮ǰ,һЩ��Ҫ�����漼��:
 - RTFM
 - RTFSC
 - STFW
 - ��ָ��,����ָ��,[jump table](https://en.wikipedia.org/wiki/Branch_table)��ӵ�л����˽�
 - ������ʽ
 - gdb
 - �˽�Makefile��һЩ��������,�˽�make����ĺ���
   - �����make����:`$make -j12 ARCH=native run mainargs=mario`
 - Bash(������shell)��һЩ�����ڽ�����
 - C99��׼
 - Little endian
 - BNF -- �Ϳ�˹-ŵ����ʽ

###I. Ϊ����ȷ����**fceux**��������л���������Ҫ����**SDL2/SDL.h**

�����Ļ���ͨ��apt�������,ʹ�� ```sudo apt-get install libsdl2-dev``` ������SDL_LIB

*Now Try your Game !*

###II. ʹ��**ccache**������compile cache��������make
���/�߳�make��Ȼ�ɽ�ʡһЩʱ��,������`nemu`��˵����Ȼ����,������Ҫ`ccache`
�����ͨ��`man ccache`���鿴ccache��Ӧ��manual
����ccache�������£�
1. ͨ���༭���޸��ļ�`bashrc`,��λ��`~/.bashrc`��
2. ׷��`export PATH="/usr/lib/ccache:$PATH"`���ļ�β
3. ˢ��`bashrc`,������ע���û������µ�½,ʹ��`source`ָ����ʵ����:`source ~/.bashrc`
4. ͨ��`which gcc`�������Ľ���Ƿ���ȷ

###III. Which ISA to choose ��

- ����ѡ��`x86`,CISCָ��������Ҹ���
- һ��Ŀ�Ƽ�`RISC-V`,RISCָ���ʽ���ҹ̶�,ʵ���Ѷȵ�
  - ��ǣ�����ʵ��Ҫ��`RISC-V`64λ�汾
- ����ѡ��`MIPS`
  - ����Ŀ�Ƽ�

###IV. (����Ŀ)Reg�Լ������Ӱ��
- �Ƿ����û�мĴ�����
    - �ǵ�
- ���ִ����ģ��ʲôӰ�죿
    - �ο�jyy��os�γ�

###V. Nemu�Ĺ�ģ�Դ�RTFSCӦ�ô����￪ʼ�أ�
- Nemu�ĳ�ʼ�������￪ʼִ��(����xv6 Kernel�����ĳ�ʼ������)?
    - ·��`./nemu/src/nemu-main.c`

###VI. ���Ժ�Ļ���������ʽ
1. ���Ȼ����������ǣ���(Macro)��ʲô��
- �Ӹ߼����Ե�Դ��������ִ���ļ�(��:ELF)���������µ�����
    - ��`cpp`Ԥ����Դ�ļ��γ�`*.i`��Ԥ�����ļ�
    - ��`cc`(��`cc1`)��ɶ�Ԥ�����ļ��ı���,���γ�`*.s`�Ļ���ļ�
    - ��`as`��ɶԻ���ļ�`*.s`�Ĵ���,���������ƵĿ��ض����ļ�`*.o`
    - ��`ld`��ɶ��ļ�`*.o`������,�γɿ�ִ���ļ�,��Unix/Unix-like��,���ʽΪ`ELF`,�����ͨ��`file $FILENAME`���鿴�ļ���`head`���ݣ����а������ļ���ʽ
    - ������`loader`,����ִ���ļ��������ڴ�,�ı�pcָ��,�޸�������,����������ת�Ƶȵ�һϵ������,����ִ�иó���

- ����꽫�ڵ�һ�׶���Ԥ���������𽫺�չ��

2. PA�еĺ�
- �漰��C99��׼�еĿ�ѡ�����꼰ճ�������
    - �ɲο�[@HNU_ZHAO�Ĳ���](https://blog.csdn.net/qq_42474104/article/details/129105242)

###VII. Ϊʲô`init_monitor`�ж��Ǻ���?
- ���ڵ��ԣ�Ҳ�����Ķ�
- ʵ�ֽӿ���ʵ�ַ���,����ͬISA����
  - ��`ISA`������,`init_monitor`���к���`init_isa`,���ڲ�ͬ��ISA��˵���������ʵ�ַ�ʽӦ���ǲ�ͬ��,���ǵ�yzhд���ϴ�pa manual����˵����"�ı���ѡ���ISA���������������κ�Ӱ��,�㲻���޸��κδ�����"?�������˼ά�����������˳���

###VIII. �����Ӻζ�����
��`abstract-machine`�нű���Ӧ��mk�ļ�

###IX. ����Makefile�д�����Ϣ�Ľ��
```c
[src/monitor/monitor.c:20 welcome] Exercise: Please remove me in the source code and compile NEMU again.
riscv32-nemu-interpreter: src/monitor/monitor.c:21: welcome: Assertion `0' failed.
```
���Կ���make���ĵ�Ϊ���Ǳ����˴������Ϣ
��λ�����ļ��ĵ�`36`��,ȥ��yzhΪ�����趨�Ķ��Դ��󱣴�󼴿ɷ���

###X. ΪʲôҪ��cpu_exec()�д���-1?
- ���ڲ���Ĺ����൱�ڴ���`uint64_max`,ʹѭ���������
    - Ҫ֪��`uint64_max`���൱�޴��,���Ǽٶ��ڸ�ѭ�����ڲ�����Щ������,�������,�����Ƕ��ڽ���ĸ��˼����,ִ������Щѭ��Ҳ��Ҫ��ʮ��֮��,���ǲ�����Ϊһ��������㷨Ӧ��ִ����˳���ʱ��,Ҫ֪��C���Ե�������Ҳ��50��֮��
- �Ƿ�UB?
    - STFW:����C99�ĵ�
```
6.3.1.3 Signed and unsigned integers
  Otherwise, if the new type is unsigned, the value is converted by repeatedly adding or subtracting one more than the maximum value that can be
represented in the new type until the value is in the range of the new type. 
```

###XI. ���ŵ��˳�
����sdb������qʱnemu�˳�������ʾ������Ϣ:
```c
make: *** [/home/epic/ics2023/nemu/scripts/native.mk:38: run] Error 1
```
���������ǣ����򷵻ؽ�������˴���:
  - ��C��C++��������У�main�����ķ���ֵ��ͨ����Ϊ���˳�״̬���򡰷���״̬������ʾ�����ִ�н�����ڴ����ϵͳ�У�����ֵΪ0��ʾ����ɹ�ִ�У�������ֵ��ʾ������ĳ�ִ��󡣷���ֵ1ͨ����ʾ����������һЩ���⣬�������������������������Ķ��졣
  - ��ǣ�������̲Ķ�C���Է���ֵ�����������Ȼ��ֹ,��ʵ�������������������ƣ�C�ķ���ֵ��һ�־���`8 bytes`�ṹ��洢�ռ�ġ�״̬������ʵ�ϣ������ǿ���ͨ���ܶ෽����������,�����ø������`SEGXXX`����Ϣ��STFW
  - ˼��һ��:Ϊʲômain�ķ���ֵ��`int`����?�����`int`������?�������Ͳ�������?����ֵ���ص�������?������֤����뷨

׷�ݵ�Դ�룺����`q`������ʲô��
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
��Ȼ����`cmd_q`����������`q`����Ĺ�����ʲô����������-1��Ȼ����`sdb_mainloop`�У����ĳ�������`handler`����ֵΪ�������˳�`sdb_mainloop`

�������ڻص��������У����ϴ�����ɺ���`engine_start()`�����,��`sdb_mainloop`������,`engine_start()`Ҳ����֮�˳�����ʱpc��ָ�����һ����䣬������`is_exit_status_bad()`��ֵ
��Ȼ���ǣ��˺����ķ���ֵ����������
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
`is_exit_status_bad()`������λ��:
```c
// ./src/utils/state.c
NEMUState nemu_state = { .state = NEMU_STOP };

int is_exit_status_bad() {
  int good = (nemu_state.state == NEMU_END && nemu_state.halt_ret == 0) ||
    (nemu_state.state == NEMU_QUIT);
  return !good;
}
```
����ֵ��good��������good��ֵ���������⡪������һ��"��"���ʽ�������Լ���ֵ
����ϣ��`q`�������˳��������жϣ�����`nemu_state.state`Ϊ`NEMU_QUIT`����`cmd_q`����Ӹô��뼴���������

###XII. SDB�Ĺ���
��������`si` ,`info` , `x`�����´���:
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

###XIII. Sanitizer ��� use after free
sanitizer����malloc�ڴ�飬ͬ���ģ�free�ڴ��Ҳ���ܵ���Ӧ�ı�ǣ�ÿ�η����ڴ棬sanitizer�������ڴ���ʺϷ��ԣ�����⵽�û���ͼ����use after free���ڴ������ֹ�����������

###Fin. ���벿�ּ��ļ���������������manual��ճ������

**PA1 Manual���˽���**