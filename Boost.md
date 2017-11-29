# Boost 笔记

#### bcp 使用

- 拷贝`boost/regex.hpp`和所有依赖的代码，包括regex源码（libs/regex/src）和构建文件（libs/regex/build）到`/foo`目录。但是，不会拷贝regex文档，test,或示/样例代码。

```
bcp  boost/regex.hpp /foo
```

- 拷贝`所有`regex库（libs/regex），包括依赖的代码（比如，regex测试程序依赖的boost.test源码）到`/foo`。

```
bcp regex /foo
```

- 拷贝`所有`regex库（libs/regex），附加config库（libs/config）和构建系统（tools/build）以及所有的依赖代码到`/foo`。（`这个非常有用`）

  ```
  bcp regex config build /foo
  ```