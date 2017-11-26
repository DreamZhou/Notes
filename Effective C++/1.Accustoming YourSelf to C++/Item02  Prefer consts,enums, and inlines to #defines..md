
```
#define ASPECT_RATIO 1.653
const double AspectRatio = 1.653   //用常量替换
```
宏名有可能没进入记号表(symbol table),不利于调试。
##### 1. 定义常量指针(constant pointers),由于常量的定义通常放在头文件内，有必要讲指针声明为const;
```
const char * const authorName = "Scott Meyers";
```
##### 2. class专属常量 声明为static 成员：

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

##### 3.  inline函数代替宏

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
