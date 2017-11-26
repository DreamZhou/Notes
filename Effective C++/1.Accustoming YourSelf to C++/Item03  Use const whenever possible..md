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
### const 成员函数
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
#### 2中流行概念：
bitwise constness (physical constness) 和 logical constness。

### 在const 和 non-const 成员函数中避免重复


