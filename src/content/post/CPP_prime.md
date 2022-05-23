---
title: "《C++Primer》笔记"
date: "2022-05-07"
tags : ["cpp"]
categories : ["读书笔记"]
author: "sparrowyang"
draft: false 
---
# 变量
## 变量初始化

```cpp
int n = 0;
int n = {0};
int n{0};
int n(0);
```

花括号（列表初始化）会对初始化时类型转换进行检查，若数据信息丢失（舍去）会报错.C++11.

在函数体外的内置变量会自动初始化，通常为0，。在函数体内部的不会初始化，即值是未定义的。

## 声明&定义

```cpp
extern int n; //声明
int n; //声明+定义
extern int n = 1;//定义
```

>函数体内部初始化extern修饰 的变量，会出错。为什么？

全局变量和局部变量同名时（不提倡），在局部访问全局。
```cpp
int n =1;
void f(){
    int n = 2;
    cout<<::n;//1
}
```

### const常量

>书中说const修饰的会在编译阶段*本文件*内的全部替换为所对于应的值。那么和define有什么区别？？？

### 常量表达式

c++11.修饰符：`constexpr`。让编译器验证赋值右边的表达式是否是常量表达式。（快懵了T_T）

## 引用和取地址

- 两者符号一样。
- 引用就是变量/对象的别名。
- 取地址操作放在右值。
- 引用在定义式。
- *引用不是对象。*
- 不能定义引用的指针（因为引用不是对象）。

### 常引用（readonly）
const 修饰的引用。常引用的类型要和引用的对象一致。

但是在初始化的时候，右值可以是*任意表达式*。表达式结果可以转成对应类型就行。
```cpp
int i = 0;
const int &r1 = i;
const int &r2 = 5;
const int &r3 = r1 * 2;
int &r4 = r1*2; //error,普通引用不能引用常引用。
```

## 指针
void* 指针。可以接受任意类型对象地址。用于指针比较、函数的输入输出。由于类型未知，不能操作对象。

*指针是对象*，可以定义指针的引用。QAQ。
```cpp
int n = 1;
int* p;
//int* 类型引用
int* &r = p;
//p = &n
r = &n
//*p = 0
*r = 0;
```

### const 指针
1. 顶层const  用来修饰指针本身
2. 底层const  用来修饰指向的的对象


```cpp
int n = 0;
const int m = 0;
//pn 不能变，一直指向n,但是指向的对象能变。
int* const pn = &n;
//pm1 pm2 能变（指向别的地址），但所指的地址不能变
const int* pm1 = &m;
const int* pm2 = &n;
//pm和其指向的对是象都不能变
const int* const pm3 = &m;

```




# 别名
别名除了`typedef`外，新标准提供另一种定义别名方法
```cpp
using dec = double;
```

# auto
auto会会忽略顶层const,保留底层

# decltupe类型指示符
`decltupe`可以根据表达式判断类型，而不需要将表达式计算出来了判断。
```cpp
decltype(fun()) n = 0;
```
fun()函数不会实际执行，n的类型为fun()函数返回类型。


# 字符判断
`cctype`提供了很多判断字符的函数，可用于判断字母、大小写、空白、数字、控制字符等。


# 异常
```cpp
try{
    //code
}catch(runtime_error err){
    cout<<err.what();
}
```
# 异常类型
stdexcept头文件定义。

- exception：常见异常
- runtiome_error：运行时
- range_error：结果超出有意义范围
- overflow_error
- underflow_error
- domain_error：参数对应结果值不存在
- invalid_argument
- length_error：超出该类型最大长度对象
- out_of_range


# 可变形参的函数，C++11
## initializer_list
参数类型一致时使用。
```cpp
void func(initializer_list<string> li){
    for(auto beg = li.begin(); beg != li.end(); ++beg){
        cout<<*beg;
    }
}

void funcc(int error_code,initializer_list<string> li){
    for(auto beg = li.begin(); beg != li.end(); ++beg){
        cout<<error_code<<*beg;
    }
}
func({"asd","qwe","zxc"});
func("asdfag");
funcc(0,{"asd","qwe","zxc"})

```

## 引用返回左值（不建议）
写出一般人看不懂的代码
```cpp
char &f(string &s,int i){return s[i]}
string ss = "abcdef";
f(ss,0)='A';
cout<<ss;//Abcdef
```

## 返回一个列表
```cpp
vector<int> f(int n){
    if(n==1){
        return {1};
    }else if(n==2){
        return {2,2};
    }else if(n>=3){
        return {3,3,3};
    }else{
        return {};
    }
}
```

# assert & NDEBUG
调试帮助，估计很少用到。

> p227,更新中
