## 3.1 :: 作用域运算符（c++独有）
局部变量优先级高于全局变量
表明归属性（数据、方法）
## 3.2命名空间 namespace 解决命名冲突
3.2.1 ：namespace A  命名为A的空间 可放变量和函数（也叫方法）
命名空间的函数可以在空间外面定义 ==A::func==，要在命名空间内声明
3.2.2 ：无名命名空间 命名空间中的标识符只能在本文件访问（static）
命名空间别名 namespace shortname = longname
3.2.3 函数重载 函数名+参数 组合代表是函数的入口地址	3.2.4 using指明 使用 A中的func 会对 func起作用 有命名冲突     using指明 使用具体的命名空间的成员 函数不可以要用作用域
但容易和局部变量冲突，不会和全局变量冲突(::a)
using碰到函数重载 [^1]using A::func  对A中所有func起作用 ^01b5f2
```c++
[[include]] <iostream>

using namespace std;
  
void func(){cout<< "无参的func="<<endl;}

void func(int a){cout<< "int的func="<<endl;}

void func(int a ,int b){cout<< "int int的func"<<endl;}


int main(int argc, char *argv[])

{

    func();

    func(10);

    func(10,20);

    //cout << "Hello World!" << endl;

    return 0;

}
```

^2a81c0


3.2.5 不同命名空间中 同名成员 使用的时候注意 二义性
### 3.2.6 总结
1不能再函数内定义命名空间
2 使用命名空间的成员 最安全方式 命名空间::成员名
3 using namespace 命名空间名 [^1]
4 单独使用 ：using 命名空间::成员名;

## 3.3全局变量检查增强（2023/7/23）
```c
int a = 10;//有赋值 定义
int a;//同名 没有赋值 声明
```
c++不允许如上操作

## 3.4c++中所有变量和参数必须有类型
```c

void fun1(i)//c++中i必须要严格定义i的类型 没有参数写void
{
	printf("i = %d",i);
}

void fun2(i)//没有写类型 
{
	printf("%s\n",i);
}
```

## 3.5更严格的类型转换

^6ed919

在c++，不同类型的变量一般不能直接赋值，需要相应强转
```c
typedef enum COLOR{ GREEN,RED,YELLOW} color;
int main()
{
color mycolor = GREEN;
mycolor = 10;//c++中不允许
}
```


## 3.6c++对结构体的增强
1、c中定义结构体变量需要加struct关键字
2、c中结构体只能定义成员变量不能定义函数，c++两者均可
```cpp
struct stu

{

    int num;

    char name[32];

    void func(void)

    {

        printf_s("dadadada");

    }

};

  

  

void test01()

{

    //c语言必须加struct

    struct stu lucy ={100,"lucy"};

    lucy.func();//调用结构体中成员的方法

}
```




## 3.7c++新增bool类型
```cpp
void text02()

{

    bool mybool;

    cout<<"sizeof(mybool) = "<<sizeof(mybool)<<endl;//1个字节

    mybool = false;

    cout<<"false = "<<false<<endl;//0

    cout<<"true = "<<true<<endl;//1

 }
```


## 3.8三目运算符功能增强
1.c语言三目运算符表达式==返回值==为数据值，为右值（等号右边的值），不能赋值
```c
void text03()

{

    int a=10;

    int b=20;

    printf("c语言:%d\n",a>b?a:b);//a>b?a:b整体结果 右值（b=20） 不能被赋值

  //  a>b?a:b = 100 错误

}
```
2.c++语言三目运算符返回值为数值本身（引用）为左值 可以赋值

```c++
void text03()

{

    int a = 10;

    int b = 20;

    cout <<"c++语言:%d"<< a > b ? a : b <<endl;

    //a>b?a:b整体结果是变量本身（引用） 左值 （能被赋值）

    a>b?a:b = 100;

}
```

c++中可以放到赋值操作符左边的的是左值，右边的是右值


==能被赋值的就是左值，不能被赋值的就是右值==

## 3.9c++中的const
用来限定一个变量不允许改变，将一个对象转换成一个常量（c++）
### 1.c语言中const
```c
const int a=10;//不要把a看成常量， a的本质 是变量 只是 只读变量
a = 100 //
```
2.c语言的const 修饰全局变量 默认是外部链接的
```fun.c
//c语言的const 修饰全局变量 默认是外部链接的

//外部链接：其他源文件可以使用

const int num =100;//只读的全局变量 内存放在文字常量区（内存空间是只读的）
```

```c
//对func.c的num进行声明(不要赋值)

extern const int num ;

void text04()

{

    printf("num = %d\n",num);

  

    //num = 200；错误 num只读

  

    //c语言中const修饰变量名 说明变量名为只读（用户不能通过变量名data进行赋初值）

    const int data = 100;//局部只读变量 内存在栈区（内存可读可写）

    //data = 200；错误

    printf("data = %d\n",data);//200

    //但是如果知道data的地址 可以通过地址间接修改所对data应空间的内容

    int *p = (int *)&data;

    *p = 2000;

    printf("data = %d\n",data);//2000

  

}
```
总结
1.const 修饰全局变量num 变量名只读 内存空间在文字常量区（只读）不能通过num的地址 修改空间内容
2.const 修饰局部变量data 变量名只读 内存空间在栈区（可读可写）可以通过data地址间接修改空间内容

### c++中的const 深入理解

```c++
fun.c
//const 修饰的全局变量 默认内部连接 只在当前源文件有效 不能直接用于其他源文件
//const int num =100;
//如果必须用在其他源文件 使用只读的全局变量 必须加extern将num转换成外部链接
extern const int num =100;


```

```c++
main.cpp

extern const int num;//不识别 num 声明

struct person

{

    int num;

    char name[32];

};

void text04()

{

    cout<< "全局num = "<<num<<endl;

  

    //c++中 对于基础类型 系统不会给data开空间 data放到符号表中

    const int data = 10;

    //data = 100 ;err data只读

    cout<<"data = "<<data<<endl;

    //c++中 对data 取地址的时候 系统就会给data开空间

    int *p =(int *)&data;

    *p = 2000;

    cout<<"*p = "<<*p<<endl;//空间内容修改成功 2000

  

    cout<<"data = "<<data<<endl;//data 还是 10 符号表地址中的data不变

  

  

  

    //2.当以变量的形式 初始化 const修饰的变量 系统会为其开辟空间

    int b = 200;

    const int a = b;//系统直接为a开辟空间 而不会把a放入符号表

    p = (int *)&a;

    *p = 3000;

    cout<<"*p = "<<*p<<endl;//3000

    cout<<"a = "<<a<<endl;//3000

  

    //3.const 自定义数据类型（结构体 对象） 系统会分配空间

  

    const person per = {100,"lucy"};

    //per.num = 1000;err

    cout<<"num = "<<per.num<<", name ="<<per.name<<endl;

    person *p1 = (person *)&per;

    p1->num = 2000;

    cout<<"num = "<<per.num<<", name ="<<per.name<<endl;

}
```

![[Pasted image 20230728234623.png]]

c++中
const int data=10;data先放入符号表
如果对data取地址 系统才会给data开辟空间
const int a = b ; b是变量名 系统直接给a开辟空间 不放入符号表
const 修饰自定义数据 系统直接给自定义数据开辟空间

## 3.10const替换#define
const 和  [[define]] 区别总结：
1.const有类型 可进行编译器类型安全检查 [[define]] 没有类型 不可进行编译器类型安全检查
2.const有作用域 [[define]] 不重视作用域 默认定义处到文件结尾 如果定义在指定作用域下的有效常量 [[define]] 就不能用
案例1.  宏没有类型 const 有

```c++
[[define]] MAX  1024

const short my_max = 1024;

void func(short i)

{

    cout<<"short函数"<<endl;

}

void func(int i)

{

    cout<<"int函数"<<endl;

}

void text05()

{

    func(MAX);// int 函数

    func(my_max);// short 函数

}
```



案例2. 宏的作用域是整个文件 const的作用域以定义情况决定
```cpp
void my_func(void)

{

    //作用范围是 当前复合语句

    const int my_num = 10;

  

    //作用范围是 当前位置到 文件结束

    [[define]] MY_NUM  10

}

void text06()

{

    //cout<<"my_num = "<<my_num<<endl;//err 不识别

    cout<<"MY_NUM = "<<MY_NUM<<endl;

  

}
```

3.宏不能作为命名空间的成员 const可以


```c++
namespace A {

    const int my_a = 100;//const 可以作为成员

    //MY_A 属于文件 不属于A

     [[define]] MY_A  100

}
void text07()

{

    cout<<"my_a = "<<A::my_a<<endl;

    //cout<<"MY_A = "<<A::MY_A<<endl;

    cout<<"MY_A = "<<MY_A<<endl;

}
```

  

## 3.11引用 
### 给==已有==变量取别名
语法：
1.&和别名 结合 表引用
2.给某个变量取别名 就定义某个变量
3.从上往下替换
```c++
int num = 10;
int& a = num；//此处&不是取地址 而是表明 a是引用变量（a是num的别名）

```
注意1 ： 引用必须初始化
注意2 ： 引用一旦初始化 就不能再次修改别名
```c++
int num = 10;
int& a = num;

int data = 20;
a = data;//不是data别名为a 而是将data值赋值给a（num）
```

```c++
void text01()

{

    int a = 10;

    int& b = a;//b就是a的别名 b==a

    cout<<"a = "<<a<<endl;//10

    cout<<"b = "<<b<<endl;//10

    b = 100;

    cout<<"a = "<<a<<endl;//100

    //b是a的别名 所以a和b有相同的地址空间

    cout<<"a的地址 = "<<&a<<endl;

    cout<<"b的地址 = "<<&b<<endl;

}
```

==一个变量可以有n个别名==

### 引用给数组取别名
1. 方式1 
```c++

void text02()
{
    int arr[5] = {10,20,30,40,50};
    //需求:给arr取个别名
    int (&my_arr)[5] = arr;//my_arr就是数组arr的别名

    int i = 0;
    for(i=0;i<5;i++)
    {
        cout<<my_arr[i]<<" ";
    }
    cout<<endl;
}
```

2. 方式2配合typedef
```c++
void text03()

{

    int arr[5] = {10,20,30,40,50};

    //1、用typedef 给数组类型 取个别名

    //TYPE_ARR就是一个数组类型（有5个元素，每个元素为int）

    typedef int TYPE_ARR[5];

    TYPE_ARR &myArr = arr;//myArr就是数组arr

    int i = 0;

    for(i=0;i<5;i++)

    {

        cout<<myArr[i]<<" ";

    }

    cout<<endl;

}

```




### 引用作为函数的参数


```c++
void my_swap1(int a,int b)

{

    int tmp=a;

    a=b;

    b=tmp;

}

  

void my_swap2(int *a,int *b)//a=&data1,b=&data2

{

    int tmp = *a;

    *a = *b;

    *b = tmp;

}

  

void my_swap3(int &a,int &b)//a=data1,b=data2

{

    int tmp = a;

    a = b;

    b = tmp;

}

  

void text04()

{

    int data1 =10, data2 = 20;

    cout<<"data1 = "<<data1<<",data2 = "<<data2<<endl;

    //my_swap1(data1,data2);//交换失败

    //cout<<"data1 = "<<data1<<",data2 = "<<data2<<endl;

    // my_swap2(&data1,&data2);//交换成功

    //cout<<"data1 = "<<data1<<",data2 = "<<data2<<endl;

    my_swap3(data1,data2);//交换成功

    cout<<"data1 = "<<data1<<",data2 = "<<data2<<endl;

  

  

}
```

 

### 给函数的返回值取别名
```c++
int& my_data1(void)

{

    int num = 100;

    return num;//函数返回啥变量 引用就是该变量的别名

    //函数的返回值是引用时 不要返回局部变量

}

  

int& my_data2(void)

{

    static int num = 200;

    return num;

}

void text05()

{

   int &ret = my_data1();//ret是别名 ret是num的别名

   //cout<<"ret = "<<ret<<endl;//非法访问内存

   int &ret1 = my_data2();//ret1是num的别名

   cout<<"ret = "<<ret1<<endl;

}
```

不能返回局部变量的引用
当函数为==左值== 那么函数的返回值类型必须是==引用==
```c++
int& my_data3(void)

{

    static int num = 10;

    cout<<"num = "<<num<<endl;

    return num;

}

  

void text06()

{

    //函数的返回值 作为左值

    my_data3() = 2000;

    my_data3();

}
```

### 引用的本质（了解）
在c++内部实现一个指针常量
type& ref = val;//type*const ref = &val
c++编译器在编译过程中使用长指针为引用的内部实现，因此引用所占用的空间大小于指针相同，只是这个过程是编译器内部实现，用户不可见
```c++
int data = 10;
inta &a = data;
//编译器内存转换 ： int * const a = &data；

a=100;//等价于data = 100
//*a = 100；//*a == data

```

### 指针的引用
```c++
[[include]]<stdlib.h>

[[include]]<string.h>//包含strcpy

void my_str1(char **p_str)//p_str = &str

{

    //*p_str == *&str == str

    *p_str = (char *)calloc(1,32);

    strcpy(*p_str,"hello world");

    return;

}

  

void  my_str2(char* (&my_str))//char* (&my_str) = str;my_str等价str

{

    my_str = (char *)calloc(1,32);

    strcpy(my_str,"hello world");

    return;

  

}


void text07()

{

    char *str = NULL;

    //需求：封装一个函数 从堆区 给str申请一个空间 并赋值“hello world”

    //my_str1(&str);

    my_str2(str);

    cout<<"str = "<<str<<endl;

    free(str);

}
```















### 常引用
1.引导出常引用
```c++
void myPrintSTU1(STU tmp)//普通结构体变量作为形参 开销太大

{

    cout<<sizeof(tmp)<<endl;

    cout<<"学号："<<tmp.num<<",姓名: "<<tmp.name<<endl;

}

  

  

void myPrintSTU2(const STU &tmp)//STU &tmp = lucy;tmp是lucy的别名 tmp没有开辟独立空间

{

    cout<<sizeof(lucy)<<endl;

    cout<<"学号："<<tmp.num<<",姓名: "<<tmp.name<<endl;

}

void text08()

{

    STU lucy = {100,"lucy"};

  

    //需求 定义一个函数 遍历lucy成员

   //myPrintSTU1(lucy);

    myPrintSTU2(lucy);

}
```
2.常量的引用
```c++
void text09()

{

    //给常量10取个别名叫 num

    //int &针对的是int 类型 ，10 是const int类型

    const int &num = 10;

    cout<<"num = "<<num<<endl;

}
```

## 3.12内联函数inline（了解）
宏函数（带参数的宏）的缺点：
1.在c中会有错误，宏看起来像一个函数调用，但是会有隐藏一些难以发现的错误。
2.问题是c++独有的，预处理器不允许访问类的成员，就是说预处理器宏不能作为类的成员函数
内联函数：内联函数为了继承宏函数的效率，没有函数调用时开销，然后又可以像普通函数那样，可以进行参数，返回值类型的安全检查，又可以作为成员函数
内联函数：是一个真正的函数.函数的替换 发生在编译阶段
```c++
inline int my_add(int x,int y)

{

    return x * y;

}

  

void test01()

{

  

    cout<<"my_add = "<<my_add(10+10,20+20)<<endl;

  

}
```
任何在类内部定义的函数自动成为内联函数
```c++
class Person
{
	public:
	Person(){cout<<"构造函数"<<endl;}
	void PrintPerson(){cout<<"输出Person"endl;}
}
```
内联函数的条件：
不能存在任何形式的循环语句
不能存在过多的条件判断语句
函数体不能太过庞大 不能对函数进取址操作

内联仅仅是给编译器一个建议，编译器不一定接受这种建议，如果你没有将函数声明为内联函数，那么编译器也可能将此函数做内联函数。一个好的编译器将会内联小的、简单的函数。
## 3.13函数的默认（缺省）参数
c++在声明函数原型的时候可为一个或者多个参数指定默认（缺省）的参数值，当函数调用的时候如果没有传递改参数值，编译器会自动用默认值代替
```c++
void test02()

{

    //如果函数传参 各自默认函数值无效

    cout<<"my_add = "<<my_add(100,200)<<endl;//300

  

    //如果某个参数未传参 该参数将启用默认值y=20

    cout<<"my_add = "<<my_add(100)<<endl;//120

    //参数未传参 x=10 y=20

    cout<<"my_add = "<<my_add()<<endl;//30

}
```

注意点
1.函数的默认参数从左向右，如果一个参数设置了默认参数，那么这个参数之后的参数都必须设置默认参数
```c++
int func01(int x,int y =20,int z=30)

{

    return x+y+z;

}

  

void test03()

{

  

    cout<<func01(100,200)<<endl;//330

    cout<<func01(100)<<endl;//150

    //cout<<func01()<<endl;//err x没有设置默认参数 必须传参

}
```
2.如果函数声明和函数定义分开写，函数声明和函数定义不能同时设置默认参数 
建议：函数声明处设置缺省参数
```fun.cpp
int func02(int x,int y ,int z)

{

    return x+y+z;

}
```
```c++
//分文件 函数定义处的默认参数 是无效的

//建议： 分文件是 在声明 给默认参数

extern int func02(int x,int y =25,int z=35);

//extern int func02(int x,int y ,int z);//err

void test04()

{

    cout<<func02(100,200)<<endl;//335

    cout<<func02(100)<<endl;//160

}
```

## 3.14函数的占位参数（了解）
函数的参数只有类型名 没有形参名 这个参数就是占位参数
由于有类型名 所以函数调用的时候必须给占位参数传参 由于没有形参名 所以函数内部是无法使用占位参数
```c++
void func03(int x,int y,int)

{

    cout<<"x = "<<x<<",y = "<<y<<endl;

    return;

}

  

void test05()

{

    func03(10,30,"hehe");//err hehe 和int类型不符

    func03(10,30,40);

}
```
==操作符重载的后置++要用到这个==

## 3.15函数重载 c++的多态的特性
函数重载：同一个函数名在不同场景下可以有不同含义
意义：方便使用函数名
条件：同一个作用域 参数个数不同 参数类型不同 参数顺序不同 与形参名无关
```c++
void my_func(int a)

{

    cout<<"int的my_func"<<endl;

  

}

void my_func(int a,int b)

{

    cout<<"int ,int的my_func"<<endl;

  

}

void my_func(int a,double b)

{

    cout<<"int ,double的my_func"<<endl;

  

}

void my_func(double a,int b)

{

    cout<<"double ,int的my_func"<<endl;

  

}

void test06()

{

    my_func(10);//int

    my_func(10,20);//int int

    my_func(10,20.2);//int double

    my_func(20.2,20);//double int

}
```
注意：
1.函数的返回值类型 不能作为 函数重载的依据
```c++
void my_func(double a,int b)

{

    cout<<"double ,int的my_func"<<endl;

  

}

/*

int my_func(double a,int b)//不能重载

{

    cout<<"double ,int的my_func"<<endl;

  

}

*/
```

2.函数重载和默认函数一起使用 需要额外注意二义性问题的产生
```c++
void my_func02(int a)

{

    cout<<"int的my_func"<<endl;

  

}

void my_func02(int a,int b=10)//默认函数

{

    cout<<"int ,int的my_func"<<endl;

  

}

void test07()

{

    //my_func02(int a) 和 my_func02(int a,int b=10)都能识别

    my_func02(10);//err 二义性产生

}
```


## 3.16 c++ 和c混合编程
c库的
```c

fun.c
[[include]]<stdio.h>

int my_add(int x,int y)

{

    return x + y;

}

int my_sub(int x,int y)

{

    return x - y;

}
```

```c
fun.h
[[ifndef]] FUN_H

[[define]] FUN_H

  

[[if]] __cplusplus

extern"C"{

[[endif]]

    extern int my_add(int x,int y);

    extern int my_sub(int x,int y);

[[if]] __cplusplus

}

[[endif]]

  

[[endif]] // FUN_H
```


```c++
[[include]] <iostream>

[[include]]"fun.h"

using namespace std;

  

int main(int argc, char *argv[])

{

    cout<<my_add(100,200)<<endl;

    cout<<my_sub(100,200)<<endl;

    return 0;

}


```
fun.c fun.h main.cpp
混合编译步骤:
gcc -c fun.c -o fun.o
g++ main.cpp fun.o -o main

## 补充说明3.01extern
==extern 用来链接指定用来声明==
extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外extern也可用来进行链接指定。







