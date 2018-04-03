### 1.粗心大意

1. iOS上安装了2个相同包名的App（应该是打包签名不同、或者是报名不一样），使用微信登录、截图发朋友圈后发回，打开另一个App。

   ​

### 2. 编译选项

 1.  代码中使用了TCHAR 字符串宏，结果条件编译宏中设置的与之前不同。故出错。

     ```c++
     //为了存储这样的通用字符，就有了TCHAR：

     //当定义了_UNICODE宏时
     #ifdef  UNICODE 
     typedef WCHAR TCHAR;
     //当没有定义_UNICODE宏时
     #else
     typedef char TCHAR;
     #endif
     ```

     ​