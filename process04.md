# PROCESS 进程

### 进程基础

* 进程是各种多任务操作(Linux, Windows, MacOS)系统中，执行任务的单元称为进程，进程是系统默认的调度单位，可以访问硬件资源(CPU,内存，硬盘等等)完成特定任务，不同的进程因为任务的差异访问不同的硬件，进程是系统中最小的分配资源单位，会为每个独立的进程分配内存空间(有些执行单元是不分配内存的)
* 程序与进程的关系(exe，elf)
  * 程序安装后，指定位置，它是一种静态资源数据，只占用磁盘空间(用户启动程序，系统创建进程)，程序是进程的静态表现
  * 进程被创建后根据需求和需要执行的任务，访问各种硬件设备，进程是程序的动态表现(进程执行程序的任务加载程序的配置文件)
    * 进程启动后，如果对程序文件没有后续的依赖和访问，程序文件可以删除，但是无法再次启动
* 进程的构成，以及进程在系统中的生存环境()
  * 进程是逻辑单元，没有实体，它不属于硬件，但是可以从内存中观察出进程组成结构
  * 下图详细展示

![https://i.imgs.ovh/2026/09/20/3ca785e2fba5be3f365f8dcf495325d5.png](https://i.imgs.ovh/2026/09/20/3ca785e2fba5be3f365f8dcf495325d5.png)

* 内存的基本单位Page页 (小页4096bytes) (大页16kb)

  * PROT_READ 只读权限
  * PROT_WRITE 只写
  * PROT_EXEC  执行，经典的library
  * PROT_NONE 无

* 不同系统位宽，分配的虚拟内存大小不同，32位系统(0-4G)，64位系统(0-16T)

  * 内存分配默认分为1-3级间接寻页，例如一级间接寻页，将4k/4=1024 newptr 指向1024新
    页面，三级间接寻页(1024三次方) = 最大寻址范围是GB单位
  * 64位操作系统最大支持4级寻页(1024四次方)虚拟地址可访问TB范围

* 系统开销

  * ```c
    //评价两个人的代码，申请8k内存
    A:ma11oc(8192)   //直接向用户分配两页内存使用
    
    B:mal1oc(5000); mal1oc(3192);  //系统开销，系统分配两页内存(8192)，访问限制，进行分配内存的检查， 发现进行了访问限制， 解除限制用户可以使用8192
    
    内存页4k   以太网帧大小1500   内核缓冲区4096
    ```

* 关于分时复用机制

  * cpu称为核心处理器，进程任务的执行需要请求cpu，进程占用cpu后执行执行，机器码，即使是多核cpu但是请求cpu的单位太多，cpu资源有限，所以不允许进程持续占用cpu，按cpu的使用时长创建时间片(10ms)一个事件片分配给进程，多个进程快速轮转交替使用cpu(称为并发执行)
    * 串行，顺序执行，例如代码段从上到下
    * 并发，多进程共用一个cpu 根据时间片使用和切换，称为并发执行，多进程并发程序可以提高得到cpu的概率，以及某个进程因阻塞放弃资源，相邻进程可以继续使用
    * 并行，必须硬件支持，多核处理器，可能多个进程占用不同的cpu同时执行，称为并行执行，并行的执行效率更高，因为可以占用大量cpu处理

* 进程的状态（五种常态）

  * 就绪态，进程等待资源(时间片)，这状态为就绪态(转为运行)   (转为终止)
  * 运行态，获取到时间片，切换到运行态使用cpu完成任务 (转为阻塞、挂起、终止、就绪)
  * 阻塞态/睡眠态，执行过程中产生阻塞，放弃cpu，等待唤醒，阻塞态可能被强制中断，无法
    完成 (转为终止、就绪) -> 外部控制切换到挂起
  * 挂起态，执行过程中被挂起，放弃cpu，等待唤醒，不可中断的，只能由唤醒继续操作，
    被挂起的进程内存数据会被交换到外存，不交换回内存，无法继续执行 (转为就绪、终止)
    终止，进程退出，释放进程资源
  * 僵尸态(zombie process)
  * 孤态(Orphan process)
  * 新生态，进程创建后没有初始化完毕，进程不可调度，称为新生态

* cpu的权限问题

  * cpu中具有权限设置，不同的级别cpu权限不同
  * level0，cpu执行在0级下，系统所有的软件硬件资源数据，cpu都可以直接访问
  * level3，cpu的最低级别访问权限，只能访问系统允许的部分数据，大多数访问受限
  * 用户层: cpu 低权限执行
  * 内核层: cpu 高权限执行

> 进程执行时，例如执行代码，系统函数都是用户层执行， 需要进行上下文切换，提高cpu权限，完成后续任务，执行完毕降低权限恢复起始权限，所以上下文切换是cpu的权限转换，主程序代码执行时总因权限不足进行切换，而后再继续执行，绝大多数的系统函数，都会触发系统调用，系统调用事件进行上下文切换，只要发起切换必须两次。（切换都是成对出现的）



### 进程原语

### 进程原语函数：

> 操作系统为了让开发者进行进程的创建与使用，提供的一些系统库函数，fork，excel ，wait， waitpid，所以掌握Linux或者unix进程的编程，上述函数一定要掌握学习。

* 在Linux或者Unix操作系统中，进程间是存在亲缘关系的，亲缘进程概念，父进程是比较有代表性的

  fork单词英文原意(叉子) 系统中的进程具备亲缘关系，每个进程必然有父进程，系统中所有的进程都是init (pid=1，pid=0)，的子级，这个核心进程没有父进程

#### fork

> fork函数用于进程创建，父进程代码中调用fork()，会创建一个子进程
> #include <unistd.h>

```c
pid_t pid; //进程id类型，pid为进程id，系统唯一的进程编号和标识
pid_t pid = getpid(); //返回当前调用进程的pid
pid = fork(void); //调用后，当前进程会创建一个子进程
```

fork函数的返回值(Return Value):

\> 0     在父进程中返回子进程pid (>0)
   0     在子进程中返回0
  -1     创建进程失败，返回-1，errno被设置，后续用户进行错误处理

* 关于继承
  * 拷贝继承方式：子进程会继承父进程所有执行数据 (早期版本fork使用)

​	![https://i.imgs.ovh/2026/09/21/27ef0caefc1abe325917c26bdea91319.png](	https://i.imgs.ovh/2026/09/21/27ef0caefc1abe325917c26bdea91319.png)



##### 早期拷贝继承的问题

```text
1.早期的拷贝继承， 会将父进程内存页表（假设100mb） 复制给子进程， 拷贝开销大， 速度慢 -> 多进程模型内存压力暴增， 因为每个子进程都要默认拷贝
2.linux系统允许进程通过execl自行重载用户空间， 无需父进程拷贝继承， 但是拷贝继承这件事是强制的， 导致子进程创建过程中产生大量无意义的拷贝开销
```



##### VFORK

```text
使用vork函数创建的子进程， 用户层是空的， 使用vfork创建的子进程无法直接被系统执行， 因为没有初始化完毕， vork+execl , 两个方法必须结合使用, 这个版本函数的诞生可以让用户选择fork创建的方式
```



##### new fork 最新版

```text
使用COW(copy on wreite)机制, 读时共享， 写时复制， 这个版本可以让父进程检测出子进程是否需要拷贝， 如果是， 则拷贝部分数据， 如果子进程不进行写操作， 可以直接共享访问父进程内存，拷贝开销为0 ， 即是写了， 父进程也只会拷贝部分数据(按page页拷贝)
新版的fork 不会产生任何多余的拷贝开销， 而且多进程内存压力几乎没有， 当前的ubuntu支持的fork
```



![](https://i.imgs.ovh/2026/09/22/660a97e336ef05759746e538b278e319.png)

* 可以使用`ps aux`命令查看进程详细信息, ubuntu 系统下父子进程的pid是连续的

  ```bash
  ps aux
  ```

  * 子进程虽然与进程共享内存， 但是子进程无法执行父进程fork， 因为无法进程调用入口， 子
    进程只能得到系统重置后的fork返回值， 为0 ， 所以子进程无法继续创建， 只能获取0后结
    束， 这就是为什么 else if(pid == 0)

  ![](https://i.imgs.ovh/2026/09/22/f89971695de5aa14a00cde1cf9e04726.png)

* 关于父子代码段区分

  >默认情况下子进程被创建后， 执行与父进程相同的代码任务， 但是多进程模型中， 大多数
  >时间和场景 ，子进程执行的任务与父进程不同， 如何区分父子进程的代码段 , 因为在不同进
  >程中(父子) pid的值不同，只需要判断pid 值 即可区分父子进程的代码段.

```c
int main(void)
{ //Linux或unix系统使用fork , 下面的代码是固定的模板， 用于区分父子进程任务
    pid_t pid;
    pid = fork();
    if(pid > 0){
        printf("parent pid %d Runing..\n",getpid());
        while(1);
    }else if(pid == 0){
        printf("child pid %d runing..\n",getpid());
        exit(0);//退出子进程 或者持续while执行
    }else{
   		perror("fork call failed");//fork函数的错误处理
    }
    printf("哈哈哈哈\n");
}
```
   *  子进程的注意事项：
子进程在自己的代码片段中执行 else if (pid == 0), 执行完毕后立即退出， 不允许踏出此段代码， 执行父进程的代码， 父进程的代码是从起始到末尾除了 else if

*  关于循环创建子进程

> 实现for循环创建多进程模型， 两种多进程：二级多进程（一父多子） ， 多级多进程（多层父子进程）

```c
int main(void)
{
    int i;
    //创建二级多进程, 每个子进程通过break退出循环， 不继续创建
    pid_t pid;
    for(i=0;i<3;i++){
        pid = fork();
        if(pid == 0)
        	break;
    }
    if(pid > 0){
    }else if(pid == 0){
    }else{}
}
```



```c
for(int i=0;i<3;i++) //循环创建进程，公式2的n次方-1，n为循环次数
	pid = fork();
```

```c
无需拷贝返回值为-1的情况， 请问下面的代码一共创建了多少个子进程
fork();
fork()&&fork()||fork();
fork();
创建四个子进程 1=3 2=2 3=2 4=0
```





#### excel

> 进程任务替换， 在进程信息保持不变的情况下， 替换更改进程任务，进程的任务数据存储在(0GB-3GB)用户层， execl可以为进程建立新的用户层， 包含新的任务代码数据， 让进程焕然一新， 拥有新的任务
>
> 1.重载：可以方便让子进程进行功能拓展， 例如进程可以随意重载系统命令和用户自定义程序，
> 某些时候多进程模型中， 子进程完成指定的任务甚至不需要编写任务代码， 使用execl就可以完成
>
> 2.例如，可以让主进程创建子进程， 子进程重载任务模块， 使用execl 实现动态更新



```bash
execl("目标程序的绝对路径","参数1 argv0","argv1",...,NULL); #重载函数，指定程序位置和参数列表， 最后一个参数必须传null, 否则调用失败
```



* 重载的过程：

![](https://i.imgs.ovh/2026/09/23/bf72bfba15cf74514abde886638e2435.png)



> 如果让子进程执行自定义代码任务，必须在fork之后、execl之前完成。



#### 僵尸进程(Zombie process)

> 进程退出之后， 进程的状态发生转变， 进程会转变为僵尸进程，进程退出转变为僵尸进程这个过
> 程必然发生， 无法避免， 进程会变为僵尸态 ，状态标识变更为（z）

* 僵尸进程的危害：
  * 僵尸进程产生内存泄漏，进程退出后，释放进程全部用户层数据，但是PCB保留未处理，产生
    内存泄漏，这个现象为僵尸
  * 进程僵尸进程会占用PCB，影响新进程的创建



![](https://i.imgs.ovh/2026/09/23/668f8e6bf280884674be2511c2ae06b1.png)



#### wait()


当产生僵尸进程后， 父进程需要调用wait函数， 对僵尸进程进行处理

* 释放PCB, 处理内存泄漏(必选)
* 是否对子进程的退出原因进行校验？ （可选）

#include <sys/wait.h>

```c
int status
pid_t zpid = wait(&status or NULL); //阻塞回收函数， 当子进程无需回收， wait函数会陷入阻塞等待，子进程退出， wait立即进行回收， 每调用一次只回收一个僵尸进程， 多进程模式需要循环
回收成功返回僵尸进程的pid (zpid), 失败返回-1 （没有子进程调用wait导致失败）
	int * status, 使用&status , 传出子进程退出信息， 便于父进程进行校验， 如果不校验， 传NULL即可
```

> wait函数只支持阻塞回收。



#### waitpid()

> 相比于wait(), waitpid 回收的方式更灵活， 而且支持通过WNOHANG关键字实现非阻塞回收，阻塞模式回收导致， 如果子进程未退出， 父进程持续因wait函数陷入阻塞， 无法执行自己的代码和任务， 非阻塞模式也叫轮询回收， 父进程询问子进程是否需要回收， 需要则立即回收， 不需要，waitpid函数会立即返回， 可以执行其他任务稍后继续询问， 父进程使用非阻塞回收模式可以在回收与自定义任务之间交替执行

```c
#include <sys/wait.h>
pid_t zpid waitpid(pid_t pid , int * status , WNOHANG);
pid : 指定回收方式， （>0 传入子进程pid 指定回收一个子进程）（-1 回收任意子进程）(0 父进程回收同组的所有子进程) （-pgid 跨组回收, 回收在pgid组中的子进程）
int * status : 在回收PCB从pcb中获取子进程退出信息传出到status变量， 后续父进程可以对子进程进行退出校验， 如果不校验， 此参数传null即可
opt : WNOHANG, 非阻塞关键字, 让waitpid变为非阻塞
Return Value : 回收u成功返回zpid(僵尸pid) , 回收失败返回-1 , 如果是非阻塞返回（0）
```

* 演示使用`waitpid()` 实现非阻塞循环回收， 回收过程中父进程的任务交替执行

```c
#include <unistd.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
void busines(void){
	printf("parent pid %d , exec busines..\n",getpid());
}
int main(void)
{
    pid_t pid;
    int i;
    for(i=0;i<3;i++)
    {
        pid = fork();
        if(pid == 0)
        	break;
    }
    if(pid > 0){
        //非阻塞循环回收， 并且执行自定义任务
        pid_t zpid;
        while((zpid = waitpid(-1,NULL,WNOHANG))!=-1){
            if(zpid > 0)
                printf("parent %d wait zombie sucess, zpid %d\n",getpid(),zpid);
            else
                busines();
            sleep(1);//间隔s数
    	}
    }else if(pid == 0){
        sleep(i+5);
        printf("child process pid %d , sleep %d exit\n",getpid(),i+5);
        exit(0);
    }else{
        perror("fork call failed");
        exit(0);
    }
}
编译指令: gcc waitpid.c -o app
```

### 使用status参数，对子进程的退出信息进行传出和验证

>我们在调用wait或waitpid时， 需要指定status参数， 而后通过系统提供的函数， 进行判断是否为正常退出，正常退出返回退出码， 如果是异常退出，例如被某个信号杀死，可以获取信号编号

* 关于验证的几个函数

  * ```c
    waitpid(-1,&status,WNOHANG);  //将子进程的退出信息传出到status中
    ```

  * WIFEXITED(status);  //判断子进程是否正常退出， 如果是返回1

  * WIFSIGNALED(status);  //判断子进程是否被信号杀死， 如果是返回1 ， （异常退出）

  * reval or exitcode = WEXITSTATUS(status);  //如果是正常退出， 返回退出码或返回值

  * signo = WTERMSIG     //如果是异常退出， 返回信号编号

### 多进程并发程序开发

> 结合几次课学习的进程相关内容，实现一个多进程Copyfile ， 一个实例， 完成一个并发任务， 使用多进程提高拷贝速度， 多进程拷贝相比单进程可以得到更多cpu资源， 但是更大的调度开销和内存开销， 要在特定的任务和场景加入并发程序设置， 而不是盲目的使用，否则会发现增大了系统开销和内存开销任务执行的速度依然没有变快

* 并发程序两个优势
  * 多个执行相同任务的进程申请cpu资源，可以得到更多时间片和cpu使用权
  * 多进程可以提高cpu的利用率， 例如某个拷贝进程阻塞， 方式cpu ,可能交给相邻的拷贝进
    程，而不是拷贝无关的进程
  * 多核处理器可能触发并行执行， 多个拷贝进程挂载到不同cpu同时执行， 效率更高
* 多进程拷贝的实例
  * 程序名：Process_copy，编译后的程序名(ELF可执行文件)

```c
./Process_copy 1.png 2.png pronum
#1.png 源文件 2.png 目标文件 pronum 进程数量， 进程数量可以缺省，如果用户不传入进程数 pronum 缺省值= 3
```
  * * 多进程拷贝流程简图

    * 模块

      ```c
      int main(int argc , char ** argv); //主接口 _START
      ```

      ```c
      int check_pram(int argc , const char * srcfile , int pronum); //参数验证
      ```

      ```c
      int block_cur(const char * srcfile, int pronum); //切片
      ```

      ```c
      int process_create(const char * srcfile, const char * destfile, int pronum , int blocksize);
      ```

      ```c
      void process_wait(void ); //父进程循环回收
      ```

      

    * 基础实现，细节部分需要学员自行完成

* 将程序的主体结构， 课上写完， 同学回去实现各个模块后， 测试多进程拷贝图片或文件, 写完后上传github提交作业

![](https://i.imgs.ovh/2026/09/25/a0c20ae467c34498d694129191bf3b12.png)



MOD/Copy.c

![](https://i.imgs.ovh/2026/09/25/db7e6a65f3468138ae53c17699b80ca0.png)

include/process_copy.h

![](https://i.imgs.ovh/2026/09/25/752b67b4fd2eaf33991def8ea212c622.png)



source/process_wait.c

![](https://i.imgs.ovh/2026/09/25/3fa4699374235d18651e6bafcded0316.png)

source/process_create.c

![](https://i.imgs.ovh/2026/09/25/dfda7766e9b33d3ae22fbee82d285bb3.png)

source/block_cur.c

![](https://i.imgs.ovh/2026/09/25/df77fec14a57839ebaccd17b4aabead8.png)

source/check_pram.c

![](https://i.imgs.ovh/2026/09/25/af322f4dc1c3649830d2b6964da2b374.png)



运行结果：

![](https://i.imgs.ovh/2026/09/25/c780993089c3ec941c9de7dfe5fb3603.png)





![https://i.imgs.ovh/2026/09/25/7bf1c85f05faf485b9855d8beb1cb70d.png](https://i.imgs.ovh/2026/09/25/7bf1c85f05faf485b9855d8beb1cb70d.png)
=======
