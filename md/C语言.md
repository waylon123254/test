

# C语言

## 数据存储

### 数据类型

#### 内置类型

#### 自定义类型（构建类型）

arm-linux-gcc 规定 char 为 unsigned char
 vc 编译器、x86上的 gcc 规定 char 为 signed char
 缺省情况下，编译器默认数据为signed类型，但是char类型除外。

格式符

格式控制符形式

%[{+,-}[0][{m,m.n}],[{l,h}]] <格式控制符>

%: 格式控制的起始符号，必不可少。格式控制起始位置
+/-:对齐标志， +:右对齐，-:左对齐，缺省：右对齐

```
int numA = 123456;
   printf("%12d\n", numA); // + 右对齐(默认右对齐不用写“+”
   printf("%-12d\n", numA); // - 左对齐
 
//      123456
//123456
```

0:实际长度没有格式控制的长度，用“0”补全内容

m/m.n: m输出展位宽，n表示取标识符的多少位输出-占位长度

```
float numB = 12333.456789;
    printf("%3.2f\n", numB); // 3.2 其中3是整数部分位宽, .2浮点数小数部分要求的位宽
    printf("%12.2f\n", numB); // 12.2 其中12是整数部分位宽 .2浮点数小数部分要求的位宽
// 如果给出数据超过则全部输出没有超过则默认右对齐空位输出
// 12333.46
//     12333.46
```



int numA = 123456;
    printf("%012d\n", numA); // + 右对齐(默认右对齐不用写“+”  
     // 输出 000000123456

整形家族；
char
	unsigned char
	signed char_
short
	unsigned short [int]
	signed short [int]
int
	unsigned int
	signed int
long
	unsigned long [int]
	signed long [int]

## 预处理命令

| 指令     | 说明                                                      |
| -------- | --------------------------------------------------------- |
| #        | 空指令，无任何效果                                        |
| #include | 包含一个源代码文件                                        |
| #define  | 定义宏                                                    |
| #undef   | 取消已定义的宏                                            |
| #if      | 如果给定条件为真，则编译下面代码                          |
| #ifdef   | 如果宏已经定义，则编译下面代码                            |
| #ifndef  | 如果宏没有定义，则编译下面代码                            |
| #elif    | 如果前面的#if给定条件不为真，当前条件为真，则编译下面代码 |
| #endif   | 结束一个#if……#else条件编译块                              |

## 指针

我们将内存中字节的编号称为地址（Address）或[指针](http://c.biancheng.net/c/80/)（Pointer）

CPU在运行C语言时数据和代码都以二进制的形式存储在内存中，计算机无法从格式上区分某块内存到底存储的是数据还是代码。当程序被加载到内存后，操作系统会给不同的内存块指定不同的权限，拥有读取和执行权限的内存块就是代码，而拥有读取和写入权限（也可能只有读取权限）的内存块就是数据。

数据在内存中的地址也称为[指针](http://c.biancheng.net/c/80/)，如果一个变量存储了一份数据的指针，我们就称它为**指针变量**。

先要理解地址和数据，你可以想象有很多盒子，每个盒子有对应的号码，那个号码叫做“地址”，而盒子里放的东西叫做“数据”。
上面就段理解了，*p和p的区别就不难解释了。
p是指针变量，用来存放地址，你可以认为是上面所说的盒子的号码，“ * ”是解引用操作符，你可以把它理解成打开盒子，p就是打开p号盒子，取出里面的数据。
简单来说，你记住，p存放的是地址，而p是让程序去那个地址取出数据。



- 乘号，用做乘法运算，例如5*6。
-  申明一个指针，在定义指针变量时使用，例如int *p;。***
- *间接运算符，取得指针所指向的内存中的值，例如printf("%d",*p);。

p是一个指针变量的名字，表示此指针变量指向的内存地址，如果使用%p来输出的话，它将是一个16进制数。

    *p表示此指针指向的内存地址中存放的内容，一般是一个和指针类型一致的变量或者常量。 而我们知道，&是取地址运算符，&p就是取指针p的地址。

区别：指针p同时也是个变量，既然是变量，编译器肯定要为其分配内存地址，就像程序中定义了一个int型的变量i，编译器要为其分配一块内存空间一样。

    &p就表示编译器为变量p分配的内存地址，而因为p是一个指针变量，这种特殊的身份注定了它要指向另外一个内存地址，程序员按照程序的需要
    让它指向一个内存地址，这个它指向的内存地址就用p表示。而且，p指向的地址中的内容就用*p表示。


```
int *p ：一级指针，表示p所指向的地址里面存放的是一个int类型的值
int **p ：二级指针，表示p所指向的地址里面存放的是一个指向int类型的指针（即p指向的地址里面存放的是一个指向int的一级指针）
例如：
int i=10; //定义了一个整型变量
int *p=&i; //定义了一个指针指向这个变量
int **p1=&p; //定义了一个二级指针指向p指针
那么取出10的值方式为：
printf(“i=[%d]\n”,*p);
printf(“i=[%d]\n”,**p1);
```

### 数组指针

```
int *p[n]//
定义了一个可以存储n个指针变量的执政数组
```

二维

````
char str1[] = "Hello,world!";
char str2[][128] = {"I miss you!", "It's the day of judgment!", "The insturment of the doom."};
/*提醒各位像我一样的萌新，注意在C语言中定义二维数组，第一个中括号内的数字可省略，而第二个不可省略。
而不同的语言在定义二维数组的时候可省略的是不一样的。请大家一定注意这种容易错的小细节*/
````

```
char *p1 = "I love you!";//上一篇文章中已经举出了类似的例子，指针可以像一维数组一样定义字符串

char *p2[3] = {"My love", "Let's clear the air!", "Open Fire!", "Bring down the hammer!"}
```

```
char *p = "Hello!";

p[1] = 'a';

printf("%s\n", p)；//猜猜结果是“Hallo”还是什么奇怪的东西？
```



数组指针，指的是数组名的指针，即数组首元素地址的指针。即是指向数组的指针。例：int (*p)[10]; p即为指向数组的指针，又称数组指针。（来源于百度百科）

## 作用域

C语言变量的作用域分为：

- 代码块作用域(代码块是{}之间的一段代码)
- 函数作用域
- 文件作用域

### 1、普通的局部变量

定义形式：在0里定义的变量，称为局部变量
作用范围：离最近的里面可以使用的
生命周期：最近的结束后，变量被释放
注意事项：局部变量未初始化值是随机的vs下不可以访问



不同的作用域下有同名的变量时候，优先考虑用最近的）内的变量，就近原则

### 2、普通的全局变量

定义形式：函数体外面定义的变量，称为普通全局变量
作用范围：当前文件中，或其他文件中都可以使用（加关键字extern)
生命周期：整个程序结束后释放
注意事项：普通全局变量未初始化，结果为0，可以访问

### 3、静态局部变量

定义形式：在0里定义，用static关键字修饰后，称为静态局部变量
作用范围：离最近的）里面可以使用的
生命周期：整个程序结束后释放
注意事项：静态局部变量未初始化结果0，只能初始化一次



1. 什么是static?
static 是 C/C++ 中很常用的修饰符，它被用来控制变量的存储方式和可见性。

1.1 static 的引入
我们知道在函数内部定义的变量，当程序执行到它的定义处时，编译器为它在栈上分配空间，函数在栈上分配的空间在此函数执行结束时会释放掉，这样就产生了一个问题: 如果想将函数中此变量的值保存至下一次调用时，如何实现？ 最容易想到的方法是定义为全局的变量，但定义一个全局变量有许多缺点，最明显的缺点是破坏了此变量的访问范围（使得在此函数中定义的变量，不仅仅只受此函数控制）。static 关键字则可以很好的解决这个问题。

另外，在 C++ 中，需要一个数据对象为整个类而非某个对象服务,同时又力求不破坏类的封装性,即要求此成员隐藏在类的内部，对外不可见时，可将其定义为静态数据。

## 内存管理

### 五大存储区〔常量区	代码区	栈区	静态区	堆区）

C代码经过预处理、编译、汇编、链接4步后生成一个可执行程序。

### 常量区

C语言中常量是占据内存的，系统将常量集中存储在一块连续的内存空间中，常量区的内存具备“只读性”，不能改写。这块存储常量的内存空间此做常量区。

### 代码区

存放函数体的二进制代码
开降时间：编译时
释放时间：程序结束后由系统释放
2.1关于函数指针
C语言的函数也是要占据内存的，系统会为这些函数的内存分配地址，我们把这个地址叫做函数指针，C语言中的函数名称称为函数指针常量（也称为函数的入口地址）

### 全局初始化数据区/静态数据区(data segment)

加载的是可执行文件数据段，存储于数据段（全局初始化，静态初始化数据，文字常量（只读））的数据的生存周期为整个程序运行过程。
栈区(stack)
栈是一种先进后出的内存结构，由编译器自动分配释放，存放函数的参数值、返回值、局部变量等。在程序运行过程中实时加载和释放，因此，局部变量的生存周期为申请到释放该段栈空间。

#### 堆区(heap)

堆是一个大容器，它的容量要远远大于栈，但没有栈那样先进后出的顺序。用于动态内存分配。堆在内存中位于BSS区和栈区之间。一般由程序员分配放，若程序员不释放，程序结束时由操作系统回收。

用maoc0或者calloc0函数创建的变量，
区别：
1>两者都是动态分配内存。主要的不同是mloc不初始化分配的内存，已分配的内存中可以是任意的值。caoc初始化已分配的内存为0.
2>calloc分配的是一个数组，而malloc分配的是一个对象，
要点：malloc和calloc分配的内存在堆区，堆内存必须通过free函数回收内存，否则造成内存泄漏。
在使用这两个函数前，要先包含头文件tinclude<stdlib.h>
开辟时间：执行到malloc或calloc函数时分配内存
销毁时间：执行到free函数时释放内存



## 第7章动态内存申请

### 7.1动态分配内存的概述

在数组一章中，介绍过数组的长度是预先定义好的，在整个程序中固定不变，但是在实际的编程中，往往会发生这种情况，即所需的内存空间取决于实际输入的数据，而无法预先确定。为了解决上述问题，C语言提供了一些内存管理函数，这些内存管理函数可以按需要动态的分配内存空间，也可把不再使用的空间回收再次利用。

### 7.2静态分配、动态分配



#### 静态分配

1、在程序编译或运行过程中，按事先规定大小分配内存空间的分配方式。int a[10]
2、必须事先知道所需空间的大小。
3、分配在栈区或全局变量区，一般以数组的形式。
4、按计划分配。

#### 动态分配

1、在程序运行过程中，根据需要大小自由分配所需空间。
2、按需分配。
3、分配在堆区，一般使用特定的函数进行分配。

### 动态分配函数

stdlib.h

1. malloc函数

   函数原型：
   void *malloc(unsigned int size);
   功能说明：
   在内存的动态存储区（堆区）中分配一块长度为sz字节的连续区域，用来存放类型说明符指定的类型。函数原型返回void*指针，使用时必须做相应的强制类型转换，分配的内存空间内容不确定，一般使用memset初始化，
   返回值：分配空间的起始地址（分配成功）
   		NULL	（分配失败）
   注意
   1、在调用malloc之后，一定要判断一下，是否申请内存成功。
   2、如果多次malloc申请的内存，第1次和第2次申请的内存不一定是连续的

2. 函数

   free函数（释放内存函数）
   头文件：include<stdlib.h>

   函数定义：void free(void *ptr)

   函数说明：free函数释放ptr指向的内存

   注意ptr指向的内存必须是malloe calloe relloe动态申请的内存

3. hans 

   3、calloc函数
   头文件：#include<stdlib.h>
   函数定义：void calloc(size_t nmemb,size t size)
   size_t实际是无符号整型，它是在头文件中，用ypedef定义出来的。
   函数的功能：在内存的堆中，申请memb块，每块的大小为size个字节的连续区域
   函数的返回值：
   返回	申请的内存的首地址	（申请成功）
   返回	NULL	（申请失败）
   注意：
   malloe和calloc函数都是用来申请内存的。
   区别：

   1)函数的名字不一样

   2)参数的个数不一样

   3)malloc申请的内存，内存中存放的内容是随机的，不确定的，而calloc函数申请的内存中的内容为0

4. hans

   4、realloc函数（重新申请内存）

   咱们调用malloc和calloc函数，单次申请的内存是连续的，两次申请的两块内存不一定连续。
   有些时候有这种需求，即我先用malloc或者calloc申请了一块内存，我还想在原先内存的基础上挨着继续申请内存。或者我开始时候使用alloc或calloc申请了一块内存，我想释放后边的一部分内存。为了解决这个问题，发明了realloc这个函数
   头文件include<stdlib.h>
   函数的定义：void*realloc(void*s,unsigned int newsize):
   函数的功能：
   在原先s指向的内存基础上重新申请内存，新的内存的大小为new_size个字节，如果原先内存后面有足够大的空间，就追加，如果后边的内存不够用，则realloc函数会在堆区找一个new_size个字节大小的内存申请，将原先内存中的内容拷贝过来，然后释放原先的内存，最后返回
   新内存的地址。
   如果newsize比原先的内存小，则会释放原先内存的后面的存储空间，只留前面的newsize个字节
   返回值：新申请的内存的首地址

5. f

   注意：malloc calloc relloc动态申请的内存，只有在free或程序结束的时候才释放。

6. 内存泄漏





# 一、局部变量与全局变量

## 1.1、局部变量

函数内部或块内定义的变量。

## 1.2、局部变量的[作用域](https://so.csdn.net/so/search?q=作用域&spm=1001.2101.3001.7020)(作用范围)

- 主函数中定义的变量只在主函数中有效，因为主函数也是一个函数，它与其他函数是平行关系。
- 不同的函数中可以使用相同的变量名，因为它们代表不同的变量，它们之间互不干扰。
- 在一个函数内部，在复合语句(块)中定义变量，这些变量只在本复合语句中有效。
- 如果局部变量的有效范围重叠，则有效范围小的优先。

```cpp
#include "stdio.h"



void main()



{



int i=2,j=3,k;



k=j+i;



 



{



int h=8;



printf("%d\n",h);



 



}



printf("%d\n",k);



}
```

## 1.3、全局变量

在函数外面定义的变量为全局变量

## 1.4、全局变量的作用域

全局变量为文本中的其他函数共有，它的有效范围是从定义点开始，直到源文件结束，全局变量又称为外部变量。

```cpp
/*打印10个100~200之间的随机数并输出最大和最小数*/



#include "stdio.h"



#include "math.h"



#include "stdlib.h"



int min;  // 全局变量



int fid()



{



    int max,r,i;  // max局部变量



	r=rand()%101+100;



	//printf(" %d",r);



	max=r;min=r;



	for(i=1;i<10;i++){



		r=rand()%101+100;   // 产生100 ~200之间的随机数



	if(r>max)



		max=r;



	if(r<min)



		min=r;



 



	printf("%d\t",r);



	}



	return max;



 



}



void main()



{



  int m = fid();



  printf("\n最大数：%d\n,最小数：%d\n",m,min);



}
```

```cpp
/*局部变量与全局变量同名案例



当局部变量与全局变量同名，在局部变量的作用范围内，全局变量不起作用，局部变量优先



*/



#include "stdio.h"



//#include "math.h"



//#include "stdlib.h"



int a=3,b=5;        // 全局变量 a,b



int max(int a, int b)  // 局部变量 a,b



{



int c;



c=a>b?a:b;



return c;



}



void main()



{



int a=8;   // 局部变量a



printf("%d\n",max(a,b));



}



 



/*



不建议使用全局变量的原因：

为了方便区别全局变量和局部变量，将局部变量的第一个字母用大写表示

1、全局变量在程序的全部执行过程中都占用存储单元，而不是在需要时才开辟单元。

2、它使等函数的通用性降低。


3、全局变量过多，会降低程序的清晰性
```

## .5、全局变量的注意事项

为了方便区别全局变量和局部变量，将局部变量的第一个字母用大写表示

1、全局变量在程序的全部执行过程中都占用存储单元，而不是在需要时才开辟单元。
 2、它使等函数的通用性降低。
 3、全局变量过多，会降低程序的清晰性

# 二、变量的存储类别

按变量的存在时间(生命周期)来划分，可以分为静态存储变量和动态存储变量。

动态存储变量(这时一种节省内存空间的存储方式)：当程序运行进入定义它的函数或复合语句时才被分配存储空间；程序运行结束离开此函数或复合语句时，释放动态动态存储变量所占用的空间。

静态存储变量：在程序运行的整个过程中，始终占用固定的内存空间，直到程序运行结束，才释放占用的内存空间。静态存储类别的变量被放于内在空间的静态存取区域

c程序运行时，占用三部分的内存空间

| 程序代码区 |
| ---------- |
| 静态存储区 |
| 动态存储区 |

- 程序运行时数据分别存储在静态存储区和动态存储区；
- 静态存储区用来存放程序运行周期内所占用固定存储单元的变量，如全局变量；
- 动态存储区用来存放不需要长期占用内存的变量，如函数的形参等；
- 变量的存储类别有四种：自动类型(auto),寄存器类型(register),静态类型(static)和外部类型(extern)。
- 动态变量：自动类型、寄存器类型
- 静态变量：静态类型，外部类型

## 2.1、自动类型变量(auto)

用关键字auto说明变量是自动类型变量


 格式：
 auto 类型  变量名;

 自动类型变量属于动态局部变量，存储在动态存储区，定义时可以加auto也可以不加，由此可知局部变量都是自动类型变量
  自动类型的分配和释放存储空间的工作是由编译系统自动处理的

```cpp
int function1(int a)



{



auto int b,c=3;



/*形参a,变量b,c都是自动类型变量



在调用该函数时，系统给它们分配存储空间调用结束时自动释放存储空间*/



}
```

## 2.3、寄存器类型变量(register)

用关键字register说明变量是寄存器类型
 格式：
 register 类型  变量名;
 register int a;

寄存器变量是动态局部变量，存放于CPU的寄存器或动态存储区中，以提高存取速度，寄存器的存取速度比内存快。
 该类变量的作用域、生存期与自动类型变量相同；如果没有存放在通用寄存器中，按自动类型变量处理


 计数机中寄存器个数是有限的，使用register的注意事项

- 寄存器类型变量不宜过多，一般将频繁使用的变量放在寄存器中（循环涉及的内部变量），以提高程序执行的速度
- 变量的长度应该与常用寄存器的长度相当，一般为int型或char型。
- 寄存器变量的定义通常是不必要的，当今优化的编译系统能够识别频繁使用的变量，并能够在不需要编译人员说明变量是register的时候，把频繁使用的变量放到寄存器中。

```cpp
#include "stdio.h"



//#include "math.h"



//#include "stdlib.h"



void main()



{



 



	int x=5,y=10,k;     // 自动类型变量



	for(k=1;k<=2;k++)



	{



	register int m=0,n=0;



	m=m+1;



	n=n+y;



	printf("m=%d\t=%d\n",m,n);



	}



}



 



/*



用关键字register说明变量是寄存器类型



格式：



register 类型  变量名;



register int a;







寄存器变量是动态局部变量，存放于CPU的寄存器或动态存储区中，以提高存取速度，寄存器的存取速度比内存快。



该类变量的作用域、生存期与自动类型变量相同；如果没有存放在通用寄存器中，按自动类型变量处理











  计数机中寄存器个数是有限的，使用register的注意事项



  1、寄存器类型变量不宜过多，一般将频繁使用的变量放在寄存器中（循环涉及的内部变量），以提高程序执行的速度



   2、变量的长度应该与常用寄存器的长度相当，一般为int型或char型。



 */
```

## 2.4、静态变量(static)

- 静态类型的变量占用静态存储区，用static关键字来说明
- static 类型 变量名；
- static int a;
- 静态变量=静态局部变量+静态全局变量
- c语言规定了静态局部变量(静态全局变量)有默认值：
- int类型等于0
- float型等于0.0
- 自动变量和寄存器变量没有默认值，值为随机数
- char型为'\0'

静态局部变量

静态局部变量是存储在静态存储区的，所以在整个程序开始的时候就被分配固定的存储单元，整个程序运行期间内不再被重新分配，故其生存期是整个程序的运行期间。

静态局部变量本身也是局部变量，具有局部变量的性质，即其作用域是局限在定义它的本函数内的，如果离开了定义它的函数，该变量就不再起作用，但其值任在，因为存储空间并未释放。

静态局部变量赋初值的时间是编译阶段，并且只被初值一次，即使它所有的函数调用结束，也不释放存储单元，因此不管调用多少此该静态局部变量的函数，它任保留上一次调用函数时的值。

## 2.5、外部变量

- 定义在任何函数之外的变量都叫做外部变量。外部变量通常用关键字extern说明。
- 一般形式:  extern  类型  变量名；
- extern  int a;
- extern double k;

  
    一个文件中定义的全局变量默认为外部的，即extern关键字可以省略。但是如果其他文件要使用这个文件中定义的全局变量，则必须在使用前用extern作为外部声明，外部声明通常放在文件的开头

# 三、案例

```cpp
#include <stdio.h>



extern int days();



extern int year,month,day;



void main(){



printf("输入年，月，日:\n");



scanf("%d%d%d",&year,&month,&day);



printf("%d月%d日是%d年的第%d\n",month,day,year,days());



}



int year,month,day;



int days()



{



 



int i,count=0;



int a[13]={0,31,28,31,30,31,30,31,31,30,31,30,31};



if((year%100)&&!(year%4)||!(year%400))



a[2]=20;



for(i=0;i<month;i++)



count+=a[i];



count=count+day;



return count;



}
```

c语言五大内存分区

1. > 栈区（stack）:存放函数形参和局部变量（auto类型），由编译器自动分配和释放

2. > 堆区（heap）:该区由程序员申请后使用，需要手动释放否则会造成内存泄漏。如果程序员没有手动释放，那么程序结束时可能由OS回收。

3. > 全局/静态存储区：存放全局变量和静态变量（包括静态全局变量与静态局部变量），初始化的全局变量和静态局部变量放在一块，未初始化的放在另一块

4. > 文字常量区：常量在统一运行被创建，常量区的内存是只读的，程序结束后由系统释放。

5. > 程序代码区：存放程序的二进制代码，内存由系统管理

　1.一个程序的3个基本段：text段，dtae段，bss段

- text段在内存中被映射为只读，但date段与bss段是可写的
- text段：代码段，就是放程序代码的，编译时确定，只读
- date段：存放在编译阶段（而非运行时）就能确定的数据，可读可写。也就是通常所说的静态存储区，赋了初值的全局变量和赋初值的静态变量存放在这个区域，常量也存在这个区域
- bss段：已经定义但没赋初值的全局变量和静态变量存放在这个区域。

> 两者之间区别是：代码段，数据段，堆栈段是cpu级别的概念，五大分区属于语言级别的概念，两者是不同的概念。

 



左边是UNIX系统的执行文件，右边是进程对应的逻辑地址空间的划分情况

- 首先是栈区（堆栈区stack）,堆栈是由编译器自动分配释放，存放函数的参数和局部变量的值（auto类型），操作方式类似于数据结构中的栈。栈的申请是由系统自动分配，如在函数内部申请一个局部变量int h,同时判断所申请空间是否小于栈的剩余空间，如果小于则为其开辟空间，为程序提供内存，否则将报异常提示栈溢出。
- 堆（heap），堆一般由程序员分配释放，若程序员不释放，程序结束可能由OS回收。它与数据结构中的堆是两回事，分配方式类似于链表，申请则是程序员自己操作使用malloc或new。申请过程比较复杂，当系统收到程序的申请时，会遍历记录空闲内存地址的链表，以求寻找第一个空间大于所申请空间的堆节点，然后将该节点从空闲节点链表中删除，并将该节点的空间分配给程序，有些情况下，新申请的内存块的首地址记录本次分配的内存块的大小，这样在delete尤其是delete[]时能正确的释放内存空间。
- 下边是全局静态存储区，全局变量与静态变量的存储是放在一块的，初始化的全局变量与静态变量存放在一块区域，未初始化的全局变量与未初始化的静态变量存放在相邻的另一块区域。
- 文字常量区，常量字符串就是放在该部分，只读存储区，程序结束后由系统释放
- 程序代码区，存放程序的二进制代码区。 



五、堆与栈的区别

　　　　　　1.申请方式

- stack:栈;由系统自动分配，自动开辟空间
- heap:由程序员自己申请并指明大小，c中malloc,c++中new。如p1=(char*)malloc(10);p2=(char*)new(10);但需要注意的是p1,p2本事是在栈中的

　　　　　　2.申请后系统的响应

- 栈：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出
- 堆：首先操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个大于所申请空间的堆节点，然后将该节点从空闲节点链表中删除，并将该节点的空间分配给程序。另外对于大部分系统，会在这块内存空间中的首地址处记录本次分配的大小，这样代码中的delete语句才能正确的释放本内存空间。另外由于找到的堆节点大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

　　　　　　3.申请大小的限制

- 栈：在windows下栈是向低地址扩展的数据结构，是一块连续的内存区域。所以栈的栈顶地址和最大容量是系统预先设定好的。在windows下栈的大小是2M.因此能从栈获得的空间比较小。
- 堆：堆是向高地址扩展的数据结构，是不连续的内存区域。这是是由于系统用链表来存储空闲内存地址的，所以是不连续的。而链表的遍历方向是由低地址到高地址。堆得大小受限于计算机系统中有效的虚拟内存大小。相比较而言堆获得的空间比较灵活，也比较大。

　　　　　　4.申请效率的比较

- 栈：由系统自动分配，速度较快，但程序员是无法控制的。
- 堆：由new分配的内存，一般速度比较慢，而且比较容易产生内存碎片，不过用起来最方便。

　　　　　　5.堆和栈中的存储内容

- 栈：在函数调用时，第一个进栈的是主函数中的下一条指令（函数调用语句的下一条可执行语句）的地址，然后是函数的各个参数。在大多数c编译器中，参数是由右往左压栈的，然后是函数中的局部变量。静态变量是不入栈的。当函数调用结束后，局部变量先出栈，然后是参数，最后栈顶指针指向最开始存的地址，，也就是主函数的下一条指令，程序由该点继续执行。
- 堆：一般是在堆的头部用一个字节存放堆得大小，其他内容自己安排。

　　　　　　6.存取效率的比较

- ```
  1 char str1[]="aaaaaa";
  2 char *str2="cccccc";
  ```

- 第一行是在运行时刻赋值的，第二行是在编译时就已经确定的，但在以后的存取过程中，在栈上的数组比指针指向的字符串快。

 

结构体的定义

结构体(struct)是由一系列具有相同类型或不同类型的数据构成的数据集合，也叫结构。

结构体和其他类型基础数据类型一样，例如int类型，char类型只不过结构体可以做成你想要的数据类型。以方便日后的使用。

在实际项目中，结构体是大量存在的。研发人员常使用结构体来封装一些属性来组成新的类型。由于C语言无法操作数据库，所以在项目中通过对结构体内部变量的操作将大量的数据存储在内存中，以完成对数据的存储和操作。

在实际问题中有时候我们需要几种数据类型一起来修饰某个变量。

例如一个学生的信息就需要学号（字符串），姓名（字符串），年龄（整形）等等。

这些数据类型都不同但是他们又是表示一个整体，要存在联系，那么我们就需要一个新的数据类型。

——结构体，它就将不同类型的数据存放在一起，作为一个整体进行处理。


结构体在函数中的作用不是简便，其最主要的作用就是封装。封装的好处就是可以再次利用。让使用者不必关心这个是什么，只要根据定义使用就可以了。


结构体的大小不是结构体元素单纯相加就行的，因为我们现在主流的计算机使用的都是32Bit字长的CPU，对这类型的CPU取4个字节的数要比取一个字节要高效，也更方便。所以在结构体中每个成员的首地址都是4的整数倍的话，取数据元素时就会相对更高效，这就是内存对齐的由来。每个特定平台上的编译器都有自己的默认“对齐系数”(也叫对齐模数)。程序员可以通过预编译命令#pragmapack(n)，n=1,2,4,8,16来改变这一系数，其中的n就是你要指定的“对齐系数”。
规则


1、数据成员对齐规则：结构(struct)(或联合(union))的数据成员，第一个数据成员放在offset为0的地方，以后每个数据成员的对齐按照#pragmapack指定的数值和这个数据成员自身长度中，比较小的那个进行。


2、结构(或联合)的整体对齐规则：在数据成员完成各自对齐之后，结构(或联合)本身也要进行对齐，对齐将按照#pragmapack指定的数值和结构(或联合)最大数据成员长度中，比较小的那个进行。


3、结合1、2可推断：当#pragmapack的n值等于或超过所有数据成员长度的时候，这个n值的大小将不产生任何效果。


在C语言中，可以定义结构体类型，将多个相关的变量包装成为一个整体使用。在结构体中的变量，可以是相同、部分相同，或完全不同的数据类型。在C语言中，结构体不能包含函数。在面向对象的程序设计中，对象具有状态（属性）和行为，状态保存在成员变量中，行为通过成员方法（函数）来实现。C语言中的结构体只能描述一个对象的状态，不能描述一个对象的行为。在C++中，考虑到C语言到C++语言过渡的连续性，对结构体进行了扩展，C++的结构体可以包含函数，这样，C++的结构体也具有类的功能，与class不同的是，结构体包含的函数默认为public，而不是private。

(a) 为什么存在
内存开辟的方式有：

int val = 20;  //在栈空间上开辟四个字节
char arr[10] = {0};  //在栈空间上开辟10字节连续空间
但是上述的开辟空间的方式有两个特点：

空间开辟大小是固定的。
数组在申明的时候，必须指定数组的长度，它所需要的内存在编译时分配。
但是对于空间的需求，不仅仅是上述的情况。有时候我们需要的空间大小在程序运行的时候才能知道，那数组的编译时开辟空间的方式就不能满足了。这时就需要使用动态内存管理。

C 语言提供的动态内存管理函数主要有四个：

malloc
– 申请动态内存空间
free
– 释放动态内存空间
calloc
– 申请并初始化一系列内存空间
realloc
– 重新分配内存空间

(B) 动态内存管理函数
(a) malloc
函数原型：

```
void *malloc(size_t size);
```

malloc 函数向系统申请分配连续可用的 size 个字节的内存空间，并返回一个指向这块空间的指针。

如果函数调用成功，返回一个指向申请的内存空间的指针，由于返回类型是 void 指针 (void*)，所以它可以被转换成任何类型的数据；如果函数调用失败，返回值是 NULL。另外，如果 size 参数设置为 0，返回值也可能是 NULL，但这并不意味着函数调用失败。
补充：
malloc 开辟空间是在堆上开辟空间。
之前栈上开辟数组大小在编译时就已经确定，malloc 是函数，所以堆空间只能在运行时确定。
malloc 开辟空间是程序员自己申请，自己释放，不释放就会出现内存泄漏问题。

函数原型：

```
void *free(void *ptr);
```

free 函数释放 ptr 参数指向的内存空间。该内存空间必须是由 malloc、calloc 或 realloc 函数申请的。否则，该函数将导致未定义行为。如果 ptr 参数是 NULL，则不执行任何操作。
注意：该函数并不会修改 ptr 参数的值，所以调用后它仍然指向原来的地方(变为非法空间)。

实例：
malloc 和 free 都证明在 stdlib.h 头文件中。

```
#include <stdio.h>
#include <stdlib.h>

int main()
{
    //代码1
    int num = 0;
    scanf("%d",&num);
    int arr[num] = {0};
    
    //代码2
    int *ptr = NULL;
    ptr = (int*)malloc(num*sizeof(int));
    if(NULL != ptr)//判断ptr指针是否为空
    {
        int i = 0;
        for(i=0;i<num;i++)
        {
            *(ptr+i) = 0;
        }
    }
    free(ptr);//释放ptr所指向的动态内存
    ptr = NULL;
    return 0;
}
```

free 时释放的是指针和对应内存块的关系





## 一、什么是结构体

结构体（struct）：是在C语言编程中，一种用户自定义可使用的数据类型，且是由多个相同或不同数据类型的数据项构成的一个集合。所有的数据项组合起来表示一条记录。（如：学生的结构体，数据项有学号、姓名、班级等等）

常用于定义的数据项类型：char、int、short、long、float、double、数组、指针、结构体等等。(结构体的成员变量数据类型）

## 二、结构体定义的方式

### 1.结构定义步骤：

①使用结构体struct语句(形式如下) ②确定定义结构体的内容 ③完成定义

```c
struct 结构体名称{
	char a;
	int b;	 //a,b,c……皆为结构体成员变量(结构体内容)
	double c;
	…………
}结构体变量;
PS:结构体名称、结构体内容、结构体变量，三者必有其二才能构成结构体
```

### 2.结构定义方式

例：学生结构体 (snumber为学号，sname为姓名，sclass为班级)

#### (1) 一般定义方式：

```c
#include<stdio.h>
/*最标准的定义方式*/
struct Student{	  //结构体定义与变量声明分开
	char snumber[16];
	char sname[12];
	char sclass[8];
};

int main(){
	struct Student s1 = {"100001","张三","一班"};//声明结构体变量
	printf("%s %s %s\n",s1.snumber,s1.sname,s1.sclass);//打印
	return 0;
}
PS：结构体定义的时候不定义变量。(最常用的定义方式)
```

(2) 一般不用的定义方式：

```c
#include<stdio.h>
/*一般不用的定义方式*/
struct Student{	  //结构体定义与变量声明一起
	char snumber[16];
	char sname[12];
	char sclass[8];
}s1 = {"100001","张三","一班"}; //声明结构体变量s1

int main(){
	printf("%s %s %s\n",s1.snumber,s1.sname,s1.sclass);//打印 s1 
	
	struct Student s2 = {"100002","李四","二班"};//声明结构体变量s2
	printf("%s %s %s",s2.snumber,s2.sname,s2.sclass);//打印 s2 
	return 0;
}
PS：结构体定义的时候声明变量。
```

(3) 最不提倡用的定义方式：

```c
#include<stdio.h>
/*最不提倡用的定义方式*/
struct{	  //结构体定义与变量声明一起，但没有结构体名称 
	char snumber[16];
	char sname[12];
	char sclass[8];
}s1 = {"100001","张三","一班"};//此结构体就只能用一次 

int main(){
	printf("%s %s %s\n",s1.snumber,s1.sname,s1.sclass);//打印
	return 0;
}
PS：结构体定义的时候无结构体名称。(即此结构体只能用一次，浪费资源)
```

#### (4) 带 typedef 的结构体：

##### ①typedef 关键字作用：相当于给已有的数据类型取个其它的名字。如下：(使用方法)

```c
#include<stdio.h>
typedef int ZhengShu;//给 int 取个新名字 ZhengShu
int main(){
	ZhengShu a = 2;//声明整数变量
	printf("%d",a); //打印
	return 0;
}
//结果输出为：2
```

##### ②使用 typedef 定义的结构体：(三种方法等价，书上常见第一种*)*

```
//第一种
/*用 typedef 定义结构体,无结构体名称*/ 
typedef struct{	  
	char snumber[16];
	char sname[12];
	char sclass[8];
}SStudent; //给结构体取别名
```

```	
//第二种
/*用 typedef 定义结构体,有结构体名称*/ 
typedef struct Student{
	char snumber[16];
	char sname[12];
	char sclass[8];
}SStudent; //给结构体取别名
```

```
//第三种
/*一般定义结构体的方法,之后再用 typedef*/ 
struct Student{
	char snumber[16];
	char sname[12];
	char sclass[8];
};
typedef struct Student SStudent;//用 typedef 给结构体取别名
```

```
/*以上三种结构体声明变量的方法相同*/
int main(){
	SStudent s1 = {"100001","张三","一班"}; //声明一个结构体变量 
	printf("%s %s %s\n",s1.snumber,s1.sname,s1.sclass);//打印
	return 0;
}
```

```
#include<stdio.h>
struct Score{ //成绩结构体 
	int Math;
	int Chinese;
	int English; 	
}; 

/*Score结构体必须比Student先定义或声明*/
struct Student{ //学生结构体 
	char snumber[16];
	char sname[12];
	char sclass[8];
	struct Score sscore;
};

//用 typedef 给结构体取别名 
typedef struct Student SStudent;
typedef struct Score SScore; 

int main(){
	SScore score = {92,88,82};  //声明一个成绩结构体变量 
	SStudent s1 = {"100001","张三","一班",score}; //声明学生一个结构体变量,并存入成绩 
	printf("信息：%s %s %s\n 成绩：%d %d %d\n",s1.snumber,s1.sname,s1.sclass, //打印学生信息 
				s1.sscore.Math,s1.sscore.Chinese,s1.sscore.English);		//打印成绩 
	return 0;
}
```

```
#include<stdio.h>
/*一般很少用,了解定义结构就行了*/

struct B;//结构体 B 必须有不完整声明 
struct A{
	struct B *p; //结构体 A 中有结构体 B 的指针 
};

struct B{
	struct A *p; //结构体 B 中有结构体 A 的指针 
};
```

3.结构体定义中使用自身结构体 (链表的结构体定义，后面有完整的链表创建使用方法)

```
#include<stdio.h>
/*简单地创建使用链表*/
struct Node{ //结构体中使用自身结构体 
	int velue;
	struct Node *next;//结构体指针 (struct可以省略)
};
```

四、结构体指针
1.普通指针：是一种用来存放内存地址的变量。(如下)

```
#include<stdio.h>
/*指针的简单使用*/ 
int main(){
	int x = 6; 
	int *p;	//一个整型指针
	p = &x;
	printf(" 整数的地址:%p\n p指针存储的地址:%p\n p指针自己的地址:%p",&x,p,&p); 
	return 0;
}
```



2.结构体指针 (配合 结构体中使用的结构体的方法一起创建 链表)

```
#include<stdio.h>
#include <stdlib.h>
/*简单地创建使用链表*/
struct Node{ //结构体中使用自身结构体 
	int velue;
	struct Node *next;//结构体指针 (struct可以省略)
};

//用 typedef 给结构体取别名 
typedef struct Node* list; //链表 
typedef struct Node Node_p;  //节点 

//创建链表 
list MakeList(){
	Node_p* head = (Node_p*)malloc(sizeof(struct Node));//结构体指针
	if(head == NULL)printf("内存不足！");
	//头节点 
	head->velue = 0;
	head->next = NULL;
	return head; 
}

//判空
bool IsEmpty(list L){
	return L->next == NULL;
} 

//插入 
void Insert(int x,list L){
	Node_p *temp,*p;//结构体指针
	temp = L;
	//到达位节点处 
	while(temp->next != NULL)temp = temp->next;
	//动态分配空间 
	p = (Node_p*)malloc(sizeof(struct Node));
	if(p == NULL)printf("内存不足！");
	//插入节点 
	p->velue = x;
	p->next = NULL;
	temp->next = p; 
}

//遍历
void PrintAll(list L){
	Node_p* temp = L->next;
	int i = 1;
	while(temp != NULL){
		printf("第%d次=%d\n",i,temp->velue);
		temp = temp->next;
		i++;
	}
} 

//主函数 
int main(){
	list L;
	L = MakeList();
	
	//判断表是否为空
	if(IsEmpty(L))printf("此链表为空！\n");
	
	//添加元素 
	for(int i = 1;i <= 5; i++){
		Insert(i,L);
	}
	
	//遍历（弹栈）
	PrintAll(L);

	return 0;
}
```

五、如何访问结构体变量
1.结构体访问成员变量时的符号：
①" . "(点)
②" → "(箭头)

2.使用方法 (要访问结构体成员时)
①如果是结构体指针，则用箭头运算符访问。
②如果是结构体一般变量，则用点运算符。

PS：对比上面 学生结构体 和 链表结构体 ，试着交换一下访问符号试试

