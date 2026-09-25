# Linux 高阶

### GitHub

**Github 项目托管网站，大量的企业级工程和开源工程在网站云端托管，聚集和海量项目，软件研发工程师的工具网站**

**仓库**：是github中项目(工程)的存储单位，一个用户可以创建多个仓库，每个仓库存一个项目
**分支**：资源的存储单位，一般一个仓库中有一个默认的主分支(Master)，上传代码数据默认放到主分支中，仓库包含分支，一个仓库可以有多个分支，主分支只有一个

#### **关于查询:**

按用户关键字进行全站的模糊查询， 注意下面的语言标签
标签查询, (样本)xxx sample         (资料) xxxx tutorial

**关于仓库中的项目**
code: 存储工程资源数据，代码、配置文件等等
README.md       LISTENSE
markedown 文本修饰语言: 文件后缀统一为.md，使用修饰符与正文结合变为一个很好的效果
README.md       LISTENSE
关于许可证，GPL3.0   APHACHE 2.0   MIT 给使用者最大使用权力，最小的限制，如果不是知名机构许可证，要详细阅读条款(避免法务问题)

issues: 问答板块，提问和解决问题

#### **git基本配置**

用个人电脑，向云端(github仓库)上传数据，需要进行设备认证，让github账户任务此设备是受信任的，后续完成加密传输
1.设备认证
创建一个本地仓库 

```
git init
```

当前位置有(Master)表示再仓库所在位置，后续的所有操作都是在这个位置

```
git config --list #查看git配置文件信息
```

在配置文件中添加两条配置，关于 email 和 username

```
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

查看是否配置成功

```
ssh -T git@github.com
```

密钥生成，创建本机RSA非对称加密密钥，并且传给账户，后续使用此密钥完成数据加密传输

```
ssh-keygen -t rsa -C"邮箱"
```

*记录一下密钥文件的存储位置，等会去复制密钥串
粘贴位置,  点击头像-> menu-> Settings-> ssh and GPG keys -> new ssh key-> 填写密钥名->粘贴密钥-> add ssh key
2.为仓库地址创建别名

```git
git remote add origin "sshittt"  *为云端仓库ssh地址创建别名，叫origin
git remote remove origin   *删除origin地址别名
```



#### **关于项目管理(内容的上传下载)**

*使用**git   bash**进行本地内容的上传，通过命令方式，数据更新/版本更新

```c++
git add file #资源添加到缓冲区
git rm file#资源从缓冲区删除同时删除文件
git restore file #将资源冲缓冲区还原到磁盘中
科林明伦
git status #查看缓冲区状态
gitcommit-m “提交说明”#将缓冲区数据提交到本地仓库，
可以附加说明信息
git push origin master 将本地仓库主分支数据 推到origin 指向的云端仓库中
```



#### **Markdown 文本修饰语言**

**使用修饰符对正文进行修饰，让正文内容附带各种效果**
1.标题修饰符，多级标题
2.正文及换行符
3.文本修饰符
4.列表 (无序列表，有序列表)
5.引用修饰符
6.超链接，图片 (本地,和网络图片) https://i.imgs.ovh/2026/09/15/f8c5db4147483f4dd9dc6cbd728e0bf9.jpg
7.表格
8.插入代码片段

```c++
#include<iostream>
using namespace std;
int main()
{
	
	return 0;
}
```

```c++
#插入命令
echo 回退
pwd
date
```

![https://i.imgs.ovh/2026/09/15/d83d9fa629254ad775a971c199fd74e3.png](https://i.imgs.ovh/2026/09/15/d83d9fa629254ad775a971c199fd74e3.png)





## 正则表达式

​	正则技术是经典的数据查询技术，通过贪心算法对数据进行快速的过滤匹配(模式匹配)，查询符合数据规则的数据(模糊查询)，例如mySQL，如果要从数据文本中查找某些片段需要使用正则技术，mysgl提供了正则表达式的支持，例如旧的str函数相关，提供一些简单的字符串处理函数 strcmp，这类函数是逐字符偏移比较，效率低下，而且不支持模糊查询，只能查找特定数据，正则技术查询效率高，按数据规则，持续向后匹配。

* 正则的匹配过程
  * 正则语句: 正则语句是由一系列表达式构成，描述数据规则 (字符串)
  * 数据源: data，可以是数据库，文本数据等等
  * 模式空间，从数据源中拷贝部分数据到模式空间，使用正则语句进行匹配
* 正则技术使用贪心算法进行查询
  * 贪心算法的不可回溯性，如果匹配过程中出现异常，无法回溯处理异常，只能持续向后，准确性存疑
  * 得到局部最优解，尝试得到全局最优解，但是实际上是不一定的
* 使用的正则技术的两种方式Linux
  1.命令使用正则  grep
  2.函数使用正则   #include<regex.h>



#### 命令使用正则

```bash
grep '正则语句' Filename  #基本构成，命令 正则语句 数据源
```



#### 正则元符号

​	正则表达式是由一系列表达式+特殊符号构成的长字符串，每个字符都是表达式，表达式默认颗粒度是一个字符为单位，用于按数据规则，匹配数据，(模式匹配)，必须了解每个正则特殊符号的含义，后续使用正则技术查找


​	正则技术非常依赖数据规则，规则越明确清晰，正则表达式的编写越简单，如果数据无序存储，没有任何边界和规则，无法使用正则表达式

```text
张三:长春理工大学:20:北京市海淀区xxxx
<name>张三</name><se>长春理工大学</se><age>20</age><addr>北京市海淀区xxxx</addr>

<name>[^<]+?</name> <name></name> 规则表达式(直接查找)  [^<]+? 关键表达式(模糊查询)
```



`使用grep正则命令后，所有显示的内容都是结果数据，红色标注一下绝对匹配到的`

* 正则元符号
  * \ 转义符号，转换表达式含义
  * \* 以前一个表达式为参照，表示该表达式出现0次或多次 例如: a*
    * a 和 aa\* 两个表达式结果相同，但是有区别吗?     aa*这种方式使用了贪心算法，匹配效率更高。
  
  * \+ 以前一个表达式为参照，表示该表达式出现1次或多次，例如 a\+，`需要转义`
  
  *  . 可以表示任意字符1次，只匹配有效数据，无法匹配空行，缩进等
  
  * [] 集合，表达式集合，集合中由若干表达式构成，每次匹配时从集合中选一个表达式匹配
  
    * [a-z]\[A-Z]\[0-9]\[a-zA-Z0-9]
    * [^abc]取非，匹配所有非集合内容的数据
  
  * ^ 以后一个表达式为参照，该表达式为行首  例如 ^a
  
  * \$ 以前一个表达式为参照，该表达式为行尾   例如n\$，"^$' 用于匹配空行
  
  * {} 以前一个表达式为参照，用于指定表达式连续出现的频率，例如a\{2\}，a{n,}   a{n,m}
  
    `需要转义`连续出现的频率从大到小匹配
  
  * () 用于创建表达式，创建一个新表达式，提升匹配表达式的颗粒度   例如(description)* `需要转义`
  * ?  切换匹配模式使用，可以切换成非贪婪模式匹配  `需要转义`



```text
ho
heo
heeo
heeeo
heeeeo
```

```bash
#匹配所有h开头o结尾的行，表达式怎么写?
grep '^h.*o$' test
```



```bash
#直接使用* + 默认贪婪模式匹配数据
grep  'a*’  file  #贪婪模式  匹配a，0次多次
grep  'a+' fi1e   #贪婪模式 匹配a  1次多次

# 非贪婪模式
grep 'a*? a+?'file  #匹配a一次或多次， 以非贪婪模式匹配
```



```html
<div>容器数据1</div>垃圾数据1<div>容器数据2</div>垃圾数据2<div>容器数据3</div>
```



```bash
grep '<div>.\+</div>'   #匹配结果 = <div>容器数据1</div>垃圾数据1<div>容器数据2</div>垃圾数据2<div>容器数据3</div>
贪婪模式尽可能多的匹配，一行中要匹配多个结果，而后结束，非贪婪模式少匹配， 匹配一个结果立即退出
非贪婪模式每匹配一个结果返回， 再进行下次匹配

grep '<div>.\+\?</div>'
<div>容器数据1</div><div>容器数据2</div><div>容器数据3</div>
```



#### 手机列表(将正确符合规则的手机号匹配出来)

* 手机号码规则
  * 11位数字构成，数字头尾
  * 1开头
  * 号段第二位3-9
  * 后9位，是0-9之间任意数，出现9次

```text
18204508500
185890980917
11238771312
12539078133
13478039213
15189097725
```

正则表达式：

```bash
grep '^1[3-9][0-9]\{9\}$' test  #以1开头，号段第二位3-9，后9位，0-9之间的数出现9次
```

#### 匹配符合规则的邮箱

```text
#编写正则表达式，要求用户名长度 8-15之间，用户名中不允许出现特殊符号， 只匹配.com后缀的邮箱
zhang777@126.com
7813981032810587209182945@qq.com
busy897@7786@gmail.com
986@126.com
878231321@qq.com
mumu78213@163.cn
890138729@qq.net
```

根据题目要求：

- 用户名长度 **8～15**
- 用户名不能出现特殊符号，也就是只允许 **字母和数字**
- 必须有且只有一个 `@`
- 邮箱后缀必须是 `.com`

正则表达式：

```bash
grep '^[a-zA-Z0-9]\{8,15\}@[a-zA-Z0-9]\+\.com$' test
```

另外一种写法 扩展正则 `grep -E`

```	bash
grep -E '^[A-Za-z0-9]{8,15}@[A-Za-z0-9]+\.com$' test.txt
```



#### 代码中使用正则技术，正则表达式函数

```bash
#include <regex.h>  #包含正则表达式头文件
```

* 正则函数列表

##### regcomp 正则转换

​	在代码中，并不是直接使用表达式(字符串)进行匹配的，首先将表达式字符串转为正则类型，后续使用正则类型进行模式匹配，正则类型 regex_t  Reg;

```c
#include <regex.h>
//<a href="https://www.baidu.com">点击访问百度</a> #超链接标签
    regex_treg; //声明正则类型
    const char * regstr = "<a href="\\([^\"]\\+\\?\\">\\([^<]\\+\\?\))
</a>"; //匹配超连接标签的表达式
	regcomp(&reg,regstr,0);
```


* 参数
  * regex_t* reg, 传入正则类型地址
  * char* regstr, 正则字符串的地址
  * int cflag，选项，默认传0即可

将regstr转换成正则类型存储在reg中，便于后续匹配查询。



**下面专门研究一下正则表达式的写法：**

```c
#include <regex.h>
<a href="https://www.baidu.com">点击访问百度</a> #超链接标签
    regex_t reg; //声明正则类型
    const char * regstr = "<a href="\\([^\"]\\+\\?\\">\\([^<]\\+\\?\))</a>"; //匹配超连接标签的表达式
//解析：
<a href="([^"]+?)">[^<]+?</a>
双引号中使用模糊查询，[]创建子表达式, 因为结尾是引号，所以[]中写的是^" 代表所有非引号的任意数据，+?非贪婪模式匹配，一直到"引号结束。
所有非<的 +？非贪婪模式匹配，一直到<
其中，如果标题如果也想要，那就在后面标题的位置加()
结果如下：<a href="([^"]+?)">([^<]+?)</a>
```



##### regfree 释放正则

正则类型reg，使用完毕通过regfree释放，避免内存泄漏

```c
regfree(&reg);  //释放正则类型
```



##### regerror 错误处理

如果因表达式不对，导致异常，可以通过regerror检查错误

```c
regerror(&reg，char *err_buffer);  //对reg中的正则进行语法检查，将结果传出到err_buffer中
```



##### regexec 匹配查询

* 正则数量？
  * 表达式中分为规则表达式(父表达式)，和关键数据表达式(子表达式)，例如匹配a标签的为规则表达式，匹配内容的即是关键数据表达式，可以使用 ()  将关键数据表达式括起来，每加一对括号，表达式数量+1，表达式的总数是n+1,n指括号的数量，使用()标记的数据，是后续我们要匹配提取的数据. 

![https://i.imgs.ovh/2026/09/17/7226c675df1ea50c7d45166b9db86434.png](https://i.imgs.ovh/2026/09/17/7226c675df1ea50c7d45166b9db86434.png)



```c
#include <regex.h>
<name>张三丰</name>
regex_t reg;//包含正则表达式
int reg_num = 2;//正则数量
char * regstr ="<name>\\([^<]\\+\\?\\</name>"  //[^<]代表所有非'<'的任意字符，'+?'非贪婪模式匹配
regmatch_t match[reg_num];//传出位置数组，数组长度取决于正则数量
regexec(&reg，mmap_ptr，intregnum，match，0);//匹配函数,调用一次匹配一条结果，匹配成功返回0， 匹配失败返回REG_NOMAT 要自行while， 循环匹配
```



* 两种方式将数据源加载到进程中
  * open, read 打开文件，读取文件到缓冲区
  * mmap 文件映射，将整个文件数据映射到进程内存中 (先简单使用，后面会讲解)



```c
#include <regex.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
int main(void)
{
    int fd = open("a.txt",0_RDWR);
    int size = 1seek(fd,0,SEEK_END);//返回文件大小
    char * mmap_ptr = NULL;
    //内存映射
    mmap_ptr = mmap(NULL, Size, PROT_READ| PROT_WRITE, MAP_PRIVATE, fd,0) ;
    //将文件数据映射到进程内存，通过mmap_ptr可以访问
    close(fd);
    //关于正则代码...
}
```



#### 正则技术使用说明

* 数据源 (磁盘文件)  (进程内存)   (外部输入/命令行参数) 

* 例如支持正则技术的应用，例如mySQL无需使用系统的正则函数，mysql内部支持，直接使用正则语句即可

* 1.确定数据源后，  2.分析待处理数据规则，编写表达式(可用性强)     3.使用正则函数匹配和提取

* 可用性强的表达式，可以匹配一种数据类型，不同的格式变体

  ```html
  <a[^>]+?href="[^"+?]"[^>]+?>[^<]+?</a>
  ```

  

#### Demo

```text
文件名:url.html
要求:使用regex正则技术
效果:将文件中的超链接地址和连接标题，匹配提取出来，打印输出
```



```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/fcntl.h>
#include <sys/mman.h>
#include <regex.h>

int main(void)
{
    //准备表达式
    char* regstr = "<a[^>]*href=\"\\([^\"]*\\)\"[^>]*>\\([^<]*\\)</a>";
    //准备正则
    regex_t reg;
    regcomp(&reg,regstr,0);
    
    //关于数据源
    int fd;    
    fd = open("url.html",O_RDWR);
    //文件大小
    int size;
    size = 1seek(fd,0,SEEK_END);
    
    //内存映射
  	char * mmap_data = NULL;
    mmap_data = mmap(NULL, size, PROT_READ| PROT_WRITE,MAP_PRIVATE,fd,0);
    close(fd);
    //遍历查找
    int regnum = 3;
    regmatch_t match[regnum];
    char link[1024];
    char title[1024];
    while((regexec(&reg,mmap_data,regnum,match,0)) == 0)
    {
        //提取数据
        bzero(link,sizeof(link));
        bzero(title,sizeof(title));
        snprintf(link,match[1].rm_eo - match[1].rm_so + 1,"%s",mmap_data + match[1].rm_so);
     	snprintf(link,match[1].rm_eo - match[1].rm_so + 1,"%s",mmap_data + match[1].rm_so);
        mmap_data += match[0].rm_eo;
        printf("匹配结果, title = %s  link = %s\n", title, link);
    }
    regfree(&reg);
    return 0;
}
```



##### 效果：

![https://i.imgs.ovh/2026/09/17/a0755cfe5fefecfb01c76a4965609f8d.png](https://i.imgs.ovh/2026/09/17/a0755cfe5fefecfb01c76a4965609f8d.png)



## PROCESS 进程

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







### 进程源语

### 进程源语函数：

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

![](https://i.imgs.ovh/2026/09/25/763348fd53af2bfc724f35c6f4199150.png)

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





![](https://i.imgs.ovh/2026/09/25/b94f6909650abe31c34f8716a133cb3c.png)
