关于const,const可以理解为仅可读(read-only)，不可写操作。但是如果是一个指针常量，const int *p,它以为这*p是不可以被改变的，但是如果p指向的地址的值发生了变化，变相着*p发生了变化。在c语言中const并不是真的常量,还是一个变量。
代码：
	include <stdio.h>
	
	int main(int argc,char **argv)
	{
	
		const int limit = 10;
		const int  *limitp = &limit;
		printf("first value :%d \n",*limitp);
	
		int i = 27;
		limitp = &i;
		printf("second value :%d \n", *limitp);
	
		i = 40;
		printf("third value :%d \n", *limitp);
	
		return 0;
	}
运行结果：
	first value :10
	second value :27
	third value :40
从运行结果中可以看出，p地址的内容其实是在变化的，仿佛const没什么作用，but我们尝试直接改变*p试试
代码：
	#include <stdio.h>
	
	int main(int argc,char **argv)
	{
		const int limit = 10;
		const int  *limitp = &limit;
		printf("first value :%d \n",*limitp);
	
		int i = 27;
		limitp = &i;
		printf("second value :%d \n", *limitp);
	
		i = 40;
		printf("third value :%d \n", *limitp);
		
		*limitp = 30; /*new add*/
	
		return 0;
	}
编译的时候报错：
	const.c 
	const.c: In function ‘main’:
	const.c:16:2: error: assignment of read-only location ‘*limitp’
所以const的操作是禁止直接对*p进行修改，p这个变量被写保护了，但是不能保证p指向的地址数值发生改变。


malloc字符串空间的应使用：malloc(strlen(str)+1)，因为'\0'


对新分配空间分配的五种方法。比如在char *fun(char *)函数里定义了一个buff[],然后return buff,这个是错误的，因为buff[]在栈上面，出栈的时候，buff被释放掉了。代码(不合理的)：
	char *fun(char *data)
	{	
		char buff[255];
	
		strcpy(buff,data);
	
		return buff;
	}
所以我们要用正确的方法，对我们需要的空间进行正确的操作。
方法1：
  返回常量区数据，例如:return "hello";这个是在常量区，跟栈没关系
方法2：
  返回的是全局变量，全局变量存放在静态数据区。
方法3：
  返回的是静态变量，静态变量存放在静态数据区。
方法4：
  malloc出一块空间，返回堆地址。
  代码：
	char *fun(char *data)
	{
		char *buff = malloc(20);
		strcpy(buff,data);
		
		return buff;
	}
方法5(应该是最常用的)：
   先malloc出一块区域，再将地址指针传入。这样的好处是，是在调用完之后，释放地址空间，很直观。
   代码:
	void fun(char *buff,char *data)
	{
		strcpy(buff,data)
	}
	
		int foo(*data)
	{
		char *buff = malloc(size);
		fun(buff,data);
		……/*可以有很多操作*/
		free(buff);
		
		return 0;
	}


c语言声明的优先级规则
A：声明从它的名字开始读取，然后按照优先级顺序优先依次读取
B：优先级顺序从高到低依次是：
   B1：声明中被括号括起来的那部分
   B2：后缀操作符：
       括号()表示这是一个函数，而方括号[]表示这是一个数组
   B3：前缀操作符：星号*表示指向……的指针
C：如果const或volatile关键字的后面紧跟着类型说明符(如int,long),它作用于说明符。在其他情况下，const，volatile作用于它左边紧邻的指针星号。
如:char * const *(*next)();
先A，它的名字是next;
再B1、B3，*next是一个指针
B2(*next)(),next是一个指针，指向一个函数。
B3*(*next)()，next是一个指向函数的指针，函数返回值又指向另一个指针
最后C，其返回值的类型是一个字符常量
总结：next是一个指针，指向一个函数，函数的返回值是另一个指针，指针的类型是一个字符类型的常量指针


程序编译分七个部分：预处理、语法和语义分析、代码生成器、优化器、汇编程序、链接器.
gcc -E test.c -o test.i   生成的test.i为预处理之后的文件
gcc -S test.c -o test.s   生成的test.s为生成的汇编代码
gcc -c test.c -o test.o   生成的test.o为机器语言
gcc还有其他选项
比如头文件存放在../h中，可以这样使用：gcc -I ../h -c test.c -o test
显示警告信息：gcc -Wall test.c -o test
多文件编译: gcc -c test1.c test2.c -o test

编译完成后的执行文件a.out，分三段：BSS（block started by symbol）,数据段，文本段。
BSS:预处理阶段，没有初始化的全局变量
数据段：预处理阶段，已经初始化的全局变量
文本段:执行的代码
注意：局部变量是不存放在a.out里的。static 修饰的数据，存放与数据段中
可执行程序的内存分配，从高地址到低地址，依次为栈、（maybe 有一段空洞）、堆、BSS、数据段、文本段、未映射保留。
栈溢出，在编译器中默认的栈大小是8k，当栈大小超过这个数的时候，会导致栈溢出错误。

setjmp(jmp_buf)、longjmp(jmp_buf,int i),setjmp用于记录当前位置，longjmp使得pc返回到记录位置，并返回i。这对组合可用于异常处理、错误恢复。在C＋＋中演化成了catch,throw。

在c程序中嵌套入汇编代码:asm，或者__asm{}；

数据是整行(一般是16或者32字节)读入至cache的，so注意字节对齐

bus error，当内存数据没对齐的时候会导致总线错误，例如double类型的数据，所存放的地址必须是8的整数倍，当存放于非8的整数倍，就会导致bus error
代码：
	union{
		char a[10];
		int i;	
	}u;
	int *p = (int *)&(u.a[1]);
	*p = 17;/*bus error*/
因为int i和联合体，确保u地址是按照4字节对齐的，so u.a[1]的地址不是以4字节对齐的，所以当数据写入到这个地址的时候，会产生bus error。

char s[]和char* s是一样的，犹如char **argv and char *argv[]
两种调用形式也是定价的a[6] and 6[a]
指针和数组的几个规则:
1、表达式中的数组名就是指针
2、c语言将数组下标作为指针的偏移量
以上两个规则可以一起理解，比如
	int a[6];
	int *p = a;
那么就可以通过指针p来调用数组a中的任意一个元素。如p[i],*(p+i)
编译器自动将下标值的步长调整到数组元素的步长。so a＋1是按照int的字节长度作为地址步长，而并非是1，所以(a＋1)==&(a[1])==(p+1)。
3、作为函数参数的数组名等价于指针
比如函数声明：char foo(int a[])，这个函数调用所传入的参数，也可以是一个指针。	
如果你想数组从1～N，而非0～N－1，那么可以在传入参数的时候，传入地址0前面的单位地址，比如foo(&a[-1])

p[i][j]将会被编译器翻译成*(*(p+i)+j),可以理解成二重指针，先通过第一重指针，找到p+i的位置，再通过第二重指针，找到p[i]
char *p[4],是一个指针数组，一个容量为4的数组里面的元素全是指针
char (*p)[4],是一个数组指针，它是一个指针，步长为4，并非char的步长(1).

C++:
返回值 类名::函数名(参数)
构造函数和析构函数，构造函数的函数名跟类名一样，如Fish::Fish；析构函数则是函数名前面加～，如：Fish::~Fish；注意前面不需要任何修饰
继承发生在两个类之间，而非两个函数。如苹果类继承水果类。class Apple : public Fruit
多态，这个其实很复杂，更多的需要百度，简单的话，就是派生类中，如果要有跟基类重名函数或者其他的，需要自己的定义操作，需在基类相应数据钱加如virtual修饰词.它可以使得派生类的成员函数优先于基类同名函数的调用，但如果派生类对虚函数未曾定制，它也可以调用基类的成员函数。
