示例代码:
[源代码地址](https://github.com/unanao/unanao.github.io/tree/master/examples/2010/linux-c-compile-procedure)

```
#include <stdio.h>

/*
 * Example for compile
 */

#define PET "little hedgehog"

int main() {
	printf("Hello lovely %s!\n", PET);

	return 0;
}

```
编译命令：
```
gcc hello.c -o hello
```

上面的这个编译命令包含了四个阶段的处理，即预处理(也称预编译，Preprocessing)、编译(Compilation)、汇编 (Assembly)和连接(Linking)。


## 1 预处理

作用： 预处理的作用主要是读入源代码，检查包含预处理指令的语句和宏定义，并对源代码进行响应的转换。预处理过程还会删除程序中的注释和多余的空白字符。

对象： 预处理指令是以“#”开头的，预处理的处理对象主要包括以下方面：

(1) #define　　宏定义  
(2) #运算符    #运算符作用是把跟在其后的参数转换成一个字符串  
(3) ##运算符　　##运算符的作用用于把参数连接到一起  
(4) 条件编译指令 #ifdef  
(5) 头文件包含指令 #include  
(6) 特殊符号 __FILE__和__LINE__ 等  


上面的hello.c文件的预处理指令是

```
gcc -E hello.c -o hello.i
```


## 2. 编译-编译成汇编语言
编译命令:
```
gcc -S hello.i -o hello.s
```

## 3. 汇编

作用：将上面的汇编指令编译生成目标文件

汇编命令:
```
gcc -c hello.s -o hello.o
```


## 4. 链接

链接的主要目的是将程序的目标文件与所需要附加的目标文件链接起来，最终生成可执行文件。目标文件也包括了所需要的库文件（静态链接库和动态链接库）

链接命令：
```
gcc hello.o -o hello
```
最终生成的test文件就是最终系统可以执行的文件。

对于程序的编译，我们一般把它认为“编译”和“链接”两部分也足够了，这里的编译已经包括了预处理，编译成汇编语言和编译成目标文件三个步骤了。只要头文件完整，语法无误，编译一般都能通过。只要有完整的目标文件和功能库文件，链接也可以成功。只要编译通过了，链接也通过了，整个项目的编译就算完成了。