#NJU PA Manual 
nops1ed
yingzesun@mail.ustc.edu.cn

***
***
##NJU PA0 

###PRE. ����ָ��
�ý׶ν����`ics2023`��Ŀ�Ļ������ù���,����Ҫ���Ľ����Ǹ���yzh�ĽŲ����pa0�е�����

###I. ���� ```make menuconfig``` ���� 
ʹ��`apt`(or some package management)����װ��Ҫ�������ע��鿴������Ϣ

**PA0 Manual ���˽���**

***
***
##NJU PA1
###PRE: ������PA1֮ǰ,һЩ��Ҫ�����漼��:
 - ��ָ��,����ָ��,[jump table](https://en.wikipedia.org/wiki/Branch_table)��ӵ�л����˽�
 - ������ʽ
 - `man` 
 - gdb
 - �˽�Makefile��һЩ��������,�˽�make����ĺ���
   - �����make����:`$make -j12 ARCH=native run mainargs=mario`
 - Bash(������shell)��һЩ�����ڽ�����
 - C99��׼
 - Little endian
###I. Ϊ����ȷ����**fceux**��������л���������Ҫ����**SDL2/SDL.h**

�����Ļ���ͨ��apt�������,ʹ�� ```sudo apt-get install libsdl2-dev``` ������SDL_LIB

*Now Try your Game !*

###II. ʹ��**ccache**������compile cache��������make
���/�߳�make��Ȼ�ɽ�ʡһЩʱ��,������`nemu`��˵����Ȼ����,������Ҫ`ccache`
�����ͨ��`man ccache`���鿴ccache��Ӧ��manual
����ccache�������£�
1. ͨ���༭���޸��ļ�`bashrc`,��λ��`~/.bashrc`��
2. ׷��`export PATH="$PATH:/usr/lib/ccache"`���ļ�β
3. ˢ��`bashrc`,������ע���û������µ�½,ʹ��`source`ָ����ʵ����:`source ./~bashrc`
4. ʹ��`which gcc`�������Ľ���Ƿ���ȷ

###III. Which ISA to choose ��

- ����ѡ��`x86`,CISCָ��������Ҹ���
- һ��Ŀ�Ƽ�`RISC-V`,RISCָ���ʽ���ҹ̶�,ʵ���Ѷȵ�
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
    - ��`ld`��ɶ��ļ�`*.o`������,�γɿ�ִ���ļ�,��Unix/Unix-like��,���ʽΪ`ELF`,�����ͨ��`file FILENAME`���鿴�ļ���`head`���ݣ����а������ļ���ʽ
    - ������`loader`,����ִ���ļ��������ڴ�,�ı�pcָ��,�޸�������,����������ת�Ƶȵ�һϵ������,����ִ�иó���

- ����꽫�ڵ�һ�׶���Ԥ���������𽫺�չ��

2. PA�еĺ�
- �漰��C99��׼�еĿ�ѡ�����꼰ճ�������
    - �ɲο�[@HNU_ZHAO�Ĳ���](https://blog.csdn.net/qq_42474104/article/details/129105242)

###VII. Ϊʲô`init_monitor`�ж��Ǻ���?
- ���ڵ��ԣ�Ҳ�����Ķ�
- ʵ�ֽӿ���ʵ�ַ���
  - ��`ISA`������,`init_monitor`���к���`init_isa`,���ڲ�ͬ��ISA��˵���������ʵ�ַ�ʽӦ���ǲ�ͬ��,���ǵ�yzhд���ϴ�pa manual����˵����"�ı���ѡ���ISA���������������κ�Ӱ��,�㲻���޸��κδ�����"?�������˼ά�����������˳���

###VIII. �����Ӻζ�����
��Shell�л�ȡ

###IX. ����Makefile�д�����Ϣ�Ľ��
```
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
```
make: *** [/home/epic/ics2023/nemu/scripts/native.mk:38: run] Error 1
```
���������ǣ����򷵻ؽ�������˴���:
  - ��C��C++��������У�main�����ķ���ֵ��ͨ����Ϊ���˳�״̬���򡰷���״̬������ʾ�����ִ�н�����ڴ����ϵͳ�У�����ֵΪ0��ʾ����ɹ�ִ�У�������ֵ��ʾ������ĳ�ִ��󡣷���ֵ1ͨ����ʾ����������һЩ���⣬�������������������������Ķ��졣

׷�ݵ�Դ�룺����`q`������ʲô��
```
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
```
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
```
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
���������