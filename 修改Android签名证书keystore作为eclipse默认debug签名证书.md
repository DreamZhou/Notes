# Android Debug keystore 制作

[来源](http://blog.csdn.net/superbigcupid/article/details/48230675)

### Eclipse默认DebugKeystore格式要求

	> Keystore name: “debug.keystore”
	>
	> Keystore password: “android”
	>
	> Key alias: “android”
	>
	> Key password: “android”
	>
	> CN: “CN=Android Debug,O=Android,C=US”

### 制作DebugKeystore

- #### 复制一份正式证书出来作为要修改为的临时调试证书。

- #### 修改keystore密码的命令(“C:\Program Files\Java\jdk1.8.0_144\bin”)：

  > keytool -storepasswd -keystore Debug.keystore 

  `Debug.keystore`是复制出来的证书文件，执行后会提示输入证书的当前密码，和新密码以及重复新密码确认。这一步需要将密码改为`android`。


- #### 修改keystore的alias：

  > keytool -changealias -keystore Debug.keystore -alias my_name -destalias androiddebugkey

  这一步中，`my_name`是证书中当前的`alias，-destalias`指定的是要修改为的`alias`，这里按规矩来，改为`androiddebugkey`这个命令会先后提示输入`keystore`的密码和当前`alias`的密码。（注意在2.中已经修改密码为`android`）


- #### 修改alias的密码：

  > keytool -keypasswd -keystore Debug.keystore -alias androiddebugkey


- #### 使用

  `Window->Preferences->Android->Build->Custom debugkeystore`