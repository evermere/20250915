# Linux 高阶

## 正则表达式

正则技术是经典的数据查询技术，通过贪心算法对数据进行快速的过滤匹配(模式匹配)，查询符合数据规则的数据(模糊查询)，例如mySQL，如果要从数据文本中查找某些片段需要使用正则技术，mysgl提供了正则表达式的支持，例如旧的str函数相关，提供一些简单的字符串处理函数 strcmp，这类函数是逐字符偏移比较，效率低下，而且不支持模糊查询，只能查找特定数据，正则技术查询效率高，按数据规则，持续向后匹配。

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
  
  * \+ 以前一个表达式为参照，表示该表达式出现1次或多次例如a\+，`需要转义`
  
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

正则表达式：

```bash
grep '^[a-zA-Z0-9]\{8,15\}@[a-zA-Z0-9]\+\.com$' test
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
    const char * regstr = "<a href="\\([^\"]\\+\\?\\">\\([^<]\\+\\?\))</a>"; //匹配超连接标签的表达式
	regcomp(&reg,regstr,0);
```

* 参数
  * regex_t* reg, 传入正则类型地址
  * char* regstr, 正则字符串的地址
  * int cflag，选项，默认传0即可

将regstr转换成正则类型存储在reg中，便于后续匹配查询



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



regexec 匹配查询

* 正则数量？
  * 表达式中分为规则表达式(父表达式)，和关键数据表达式(子表达式)，例如匹配a标签的为规则表达式，匹配内容的即是关键数据表达式，可以使用()将关键数据表达式括起来，每加一对括号，表达式数量+1，表达式的总数是n+1,n指括号的数量，使用()标记的数据，是后续
    我们要匹配提取的数据

![https://i.imgs.ovh/2026/09/17/7226c675df1ea50c7d45166b9db86434.png](https://i.imgs.ovh/2026/09/17/7226c675df1ea50c7d45166b9db86434.png)



```c
#include <regex.h>
<name>张三丰</name>
regex_t reg;//包含正则表达式
int reg_num = 2;//正则数量
char * regstr ="<name>\\([^<]\\+\\?\\</name>"   //[^<]代表所有非'<'的任意字符，'+?'非贪婪模式匹配,一直到<匹配结束
regmatch_t match[reg_num];//传出位置数组，数组长度取决于正则数量
regexec(&reg，mmap_ptr，intregnum，match，0);//匹配函数,调用一次匹配一条结果，匹配成功返回0， 匹配失败返回REG_NOMATCH，用户需要自行while， 循环匹配
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



![https://i.imgs.ovh/2026/09/17/26b4bcb9bd4a5d04543b914cc663bcf9.png](https://i.imgs.ovh/2026/09/17/26b4bcb9bd4a5d04543b914cc663bcf9.png)



![https://i.imgs.ovh/2026/09/17/2a24d0331f0a6ceaefebff883c4ef82f.png](https://i.imgs.ovh/2026/09/17/2a24d0331f0a6ceaefebff883c4ef82f.png)





![https://i.imgs.ovh/2026/09/17/5ab132068e43ab663c3d6374829111db.png](https://i.imgs.ovh/2026/09/17/5ab132068e43ab663c3d6374829111db.png)
