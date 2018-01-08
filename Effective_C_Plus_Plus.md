[TOC]



# 1. Accustoming Yourself to C++

##Item01  View C plus plus as a federation of languages

#### C++ 是一个多重泛型编程语言
1. 过程式       procedural;
2. 面向对象形式 object-oriented;
3. 函数式       functional;
4. 泛型形式     generic;
5. 元编程形式   metaprogramming;

##Item02  Prefer consts,enums, and inlines to #defines.
```
#define ASPECT_RATIO 1.653
const double AspectRatio = 1.653   //用常量替换
```
宏名有可能没进入记号表(symbol table),不利于调试。
#### 1. 定义常量指针(constant pointers),由于常量的定义通常放在头文件内，有必要讲指针声明为const;
```
const char * const authorName = "Scott Meyers";
```
#### 2. class专属常量 声明为static 成员：

```
class GamePlayer{
    static const int NumTurns = 5;     //常量声明
    int scores[NumTurns];
    ...
};
```
如果要取class专属常量的地址，必须提供定义式：

```
const int GamePlayer::NumTurns;   //NumTurns 的定义   因为已在声明时获得初始值，故不等再设初始值
                                  // 放到一个实现文件而不是头文件
```
the enum hack  "一个属于枚举类型的(enumerated type)的数值可权充ints被使用", GamePlayer 可被定义如下：

```
class GamePlayer{
private:
    enum { NumTurns = 5 };      //the enum hack 令NumTurns成为5的一个记号名称.
    int scores[NumTurns];
    ...
}
```

#### 3.  inline函数代替宏

```
// 以a,b 最大值调用f
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))
```

```
int a = 5, b = 0;
CALL_WITH_MAX(++a, b);          // a 被累加二次;
CALL_WITH_MAX(++a, b+10);       // a 被累加一次;
```
   template inline 函数代替

```
template<typename T>
inline void callWithMax(const T& a, const T& b)
{
    f(a > b ? a: b);
}
```

##Item03  Use const whenever possible.
const 可以在classes 外部修饰global 或 namespace 作用域中的常量,或修饰文件、函数、区块作用域中悲伤为static的变量：

```
char greeting[] = "Hello";          
char* p = greeting;                     //non-const pointer, non-const data
const char* p = greeting;               //non-const pointer, const data
char* const p = greeting;               //const pointer, non-const data
const char* const p = greeting;         //const pointer, const data
```
关键字出现在 * 左边表示被指物是常量，出现在 * 右边，表示指针本身是常量；
出现在 * 两边表示被指物和指针本身都是常量。
类型之前和 类型之后 意义一样。

```
void f1(const Widget* pw);              //f1 同f2 意思一样 都是指向一个常量 const
void f2(Widget const* pw);
```
#### const 成员函数
2个成员函数如果只是常量性不同（constness）不同，可以被重载。

```
class TextBlock{
public:
    ...
    const char& operator[](std::size_t position) const
    { return text[position];}
    char& operator[](std::size_t position)
    { return text[position];}
private:
std::string text;
};
```
non-const operator[] 返回类型是 referance to char ,否则下面的语句无法通过编译：

```
tb[0] = 'x';
```
#### 2种流行概念：
bitwise constness (physical constness) 和 logical constness。

#### 在const 和 non-const 成员函数中避免重复

##Item04 Make sure that objects are initialized before they're used
#### 初始化

- 内置类型不会被初始化，除非是static。
- 区分赋值(assignmeng) 和初始化(iniitialization)。
- 对象的成员变量初始化发生在进入构造函数本体之前。
- 总是使用成员初值列(member initialization list) 初始化。
- 总是在初值列中列出所有成员变量。
- 初始化顺序：base classes > derived classes 。
- class 的成员变量总是以其声明次序初始化。

#### Static 对象

> ​	static 对象其寿命从构造出来直到程序结束，包括global 对象，定义于namespace作用域内的对象，在class内、在函数内以及在file作用域内被声明为static的对象。
>
> 函数内的static对象为 local static 
>
> 其他static 对象为 non-local static 

- 为避免跨编译单元的初始化顺序问题，以local static 代替 non-local static 对象
- C++ 对于定义于不同编译单元内的non-local static 对象的初始化相对顺序无明确定义。
- C++ 保证函数内的local static 对象会在该函数调用期间，首次遇上该对象的定义式时被初始化。

```c++
class FileSystem {...}
FileSystem& tfs()
{
    static FileSystem fs;
    retrun fs;
}
```

# 2. Constructors Destructors, and Assignment Oprators

##Item05 Konw what functions C++ silently writes and calls

- C++ 编译器会 生成 default 构造函数，copy 构造函数，copy assignment 操作符和析构函数,如果未什么任何构造函数，编译器回声明一个default构造函数。
- 编译器产出的 析构函数 是个 non-virtual ,除非这个class 的 base class 有 virtual 析构函数
- 默认生成的 copy构造函数和copy assignment 函数只是将源对象的每一个non-static 成员变量copy到目标对象。


## Item06 

#### 将copy ctor 和 copy assignment 声明为private 而不实现

```c++

class HomeForSale{
public:
 // ...
private:
  HomeForSale(const HomeForSale&);
  HomeForSale& operator=(const HomeForSale&);
};
```

```C++
class Uncopyable{
protected:
  Uncopyable(){}
  ~Uncopyable(){}
private:
  Uncopyable(const Uncopyable&);
  Uncopyable& operator=(const Uncopyable&);
};
```

## Item07 Declare destructors virtual in polymorphic base class 

- 当`derived class`对象经由 `base class`指针删除时，如果base 带着一个 non-virtual 析构函数，其结果未定义。 对象的derived部分未销毁，而base class 部分被销毁
- 任何`class` 只要带有virtual 函数几乎确定需要virtual 析构函数
- `class` 不含 virtual 函数 ，不用 virtual 析构函数 -> virtual 函数回产生 virtual table
- `STL`容器 ，`std::string`  都不带virtual 析构函数，不能被继承。

## item08 Prevent exceptions from leaving destructors

