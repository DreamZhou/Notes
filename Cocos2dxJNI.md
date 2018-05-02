## C++代码

[参考来源](https://blog.csdn.net/sgn132/article/details/50511912)

- 头文件包含

  ```C++
  #if CC_TARGET_PLATFORM == CC_PLATFORM_ANDROID
  //#include <Jni.h>                                  
  #include "platform/android/jni/JniHelper.h"
  #endif

  //在Cocos中用JNI调用JAVA程序时，jni.h这个头文件是必须的，好在JniHelper.h中已经帮我们包含了，
  //所以我们只需要包含JniHelper.h这个头文件就可以了。
  ```

- 示例

  ```C++
  typedef struct JniMethodInfo_
  {
      JNIEnv *    env;  				//线程相关的结构体，里面保存的是 JNI结构体。  		
      jclass      classID;
      jmethodID   methodID;
  } JniMethodInfo;
  ```

  TestJni.java 路径:  src/org/cocos2dx/cpp 

  ```java
      package org.cocos2dx.cpp;

      import android.util.Log;

      public class TestJni {
          public static void func1(){
              Log.d("success","java jni called succeed");
          }
      }
  ```

  C++ 代码

  ```C++
  #if(CC_TARGET_PLATFORM == CC_PLATFORM_ANDROID)
      JniMethodInfo info;
      //getStaticMethodInfo判断java定义的静态函数是否存在，返回bool
      bool ret = JniHelper::getStaticMethodInfo(info,"org/cocos2dx/cpp/TestJni","func1","()V");
      if(ret)
      {
          log("call void func1() succeed");
          //传入类ID和方法ID，小心方法名写错，第一个字母是大写
          info.env->CallStaticVoidMethod(info.classID,info.methodID);
      }
  #endif
  ```

  ```C++
      static bool getStaticMethodInfo(JniMethodInfo &methodinfo,
                                      const char *className,
                                      const char *methodName,
                                      const char *paramCode);
      static bool getMethodInfo(JniMethodInfo &methodinfo,
                                const char *className,
                                const char *methodName,
                                const char *paramCode);
  //@ methodinfo   
  //@ className	类名
  //@ methodName	方法名
  //@ paramCode	方法签名	调用方法传递的参数和返回值类型
  ```

- JNI 传递自定义数据类型

  JNI 的规范签名格式：

  > ```
  > (参数1类型标示参数2类型标示...参数n类型标示)返回值类型标示。
  >
  > //举例：
  >     void play(String singer,String album)
  >     (Ljava/lang/String;Ljava/lang/String;)V
  > ```

  | 标示类型                 | Java类型   |
  | -------------------- | -------- |
  | Z                    | boolean  |
  | B                    | byte     |
  | C                    | char     |
  | S                    | short    |
  | I                    | int      |
  | J                    | long     |
  | F                    | float    |
  | D                    | double   |
  | L/java/lang/String;  | String   |
  | [I                   | int[]    |
  | [L/java/lang/object; | Object[] |

  如果Java类型是数组，在左侧加上[, 另外引用类型(除基本类型数组外),标识最后一个`;`

  ​

| 函数签名                  | Java函数                |
| --------------------- | --------------------- |
| ()Ljava/lang/String;  | string f()            |
| (ILjava/lang/Class;)J | long f(int i,Class c) |
| ([B)V                 | void f(byte[] bytes)  |

