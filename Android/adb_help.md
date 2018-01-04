# adb命令

### 1、Android录制视频

在命令行显示log,参数: `--verbose`:

```
adb shell screenrecord --verbose /sdcard/demo.mp4
```

默认录制时间为180s，如果需要提前完成录制按下 `Ctrl+C` 即可完成录制。

限制录制时间，参数：--time-limit,限制视频录制时间为10s：

```
adb shell screenrecord --time-limit 10 /sdcard/demo.mp4
```

指定视频分辨率大小，参数： `--size`，分辨率为1280x720，如果不指定默认使用手机的分辨率,为获得最佳效果，请使用设备上的高级视频编码（AVC）支持的大小：

```
adb shell screenrecord --size 1280x720 /sdcard/demo.mp4
```

指定视频的比特率,参数: `--bit-rate`,比特率为6Mbps,如果不指定,默认为4Mbps.你可以增加比特率以提高视频质量或为了让文件更小而降低比特率 ：

```
adb shell screenrecord --bit-rate 6000000 /sdcard/demo.mp4
```

导出视频：

```
adb pull /sdcard/demo.mp4
```

查看帮助命令，参数：`--help`:

```
adb shell screenrecord --help
```

### 2、android 手机截屏

```
adb shell screencap /sdcard/1.png
or
adb shell /system/bin/screencap -p /sdcard/screenshot.png（保存到SDCard）
adb pull /sdcard/screenshot.png d:/screenshot.png（保存到电脑）
```

### 3、Android系统信息获取

```
//系统配置信息
android.os.Build
STARNAUTE4:/ $ getprop ro.build.id
           NRD90M
STARNAUTE4:/system $ cat build.prop
STARNAUTE4:/ $ getprop ro.build.type
//软硬件信息
SystemProperty
STARNAUTE4:/proc $ cat cpuinfo


```

### 4、AM命令

```
lee@lee:~$ adb shell am start -n com.example.lee.myapplication/com.example.lee.myapplication.MainActivity
Starting: Intent { cmp=com.example.lee.myapplication/.MainActivity }
lee@lee:~$ adb shell am start -N com.example.lee.myapplication/com.example.lee.myapplication.MainActivity
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.example.lee.myapplication/.MainActivity }


$ adb install-multiple -r -t -p com.example.lidongxue.myapplication /home/lidongxue/AndroidStudioProjects/MyApplication5/app/build/outputs/apk/debug/app-debug.apk 
$ adb shell am start -n "com.example.lee.myapplication/com.example.lee.myapplication.MainActivity" -a android.intent.action.MAIN -c android.intent.category.LAUNCHER

```

### 5、PM命令

```
//输出所有已安装的应用
adb shell pm list packages -f 
```

### 6、使用命令对APK签名

1.创建keystore库，JDK的安装目录下的bin子目录下提供了keytool.exe工具来生成数字证书。在命令行窗口输入如下命令：

```
lee@lee:~/Documents/apksigned$ keytool -genkeypair -alias lee -keyalg RSA -validity 400 -keystore lee.jks
Enter keystore password:  
Re-enter new password: 
What is your first and last name?
  [Unknown]:  lee
What is the name of your organizational unit?
  [Unknown]:  lee.org
What is the name of your organization?
  [Unknown]:  lee.org
What is the name of your City or Locality?
  [Unknown]:  hangzhou
What is the name of your State or Province?
  [Unknown]:  zhejiang
What is the two-letter country code for this unit?
  [Unknown]:  CN
Is CN=lee, OU=lee.org, O=lee.org, L=hangzhou, ST=zhejiang, C=CN correct?
  [no]:  y

Enter key password for <lee>
	(RETURN if same as keystore password):  
Re-enter new password: 

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore lee.jks -destkeystore lee.jks -deststoretype pkcs12".

lee@lee:~/Documents/apksigned$ keytool -importkeystore -srckeystore lee.jks -srcstorepass lee123 -destkeystore lee.jks -deststoretype pkcs12  -deststorepass lee0123
Enter key password for <lee>  【lee0123】
Entry for alias lee successfully imported.
Import command completed:  1 entries successfully imported, 0 entries failed or cancelled

Warning:
Migrated "lee.jks" to Non JKS/JCEKS. The JKS keystore is backed up as "lee.jks.old".
lee@lee:~/Documents/apksigned$ jarsigner -verbose -keystore /XXX/lee.jks -signedjar hi.apk /XXX/app-debug.apk lee
Enter Passphrase for keystore: xxxxxxx

```

上面命令中各选项说明如下。
-genkeypair:指定生成数字证书
-alias:指定生成数字证书的别名
-keyalg:指定生成数字证书的算法，使用RSA算法
-validity:指定生成的数字证书的有效期(400代表400天)
-keystore:指定所生成的数字证书的储存路径
输入上面的命令后按回车，会出现以交互方式让用户输入数字证书keystore的密码/作者/公司等详细信息。
备注：这一步时生成属于你们公司/你的数字证书，这一步只需要做一次即可。一旦数字证书创建成功之后，只要在该证书有效期内，可以一直重复使用该证书。

2.生成未签名的APK安装包。在Eclipse中右击Android项目，在弹出的菜单中找到“Android Tools -->Export Unsigned Application Package...“菜单项，Eclipse弹出一个保存文件的对话框，当用户选择储存文件后单击”Finish”按钮即可生成一个未签名的APK安装包。在Android studio的Build-->Make Project 即可生成未签名的APK安装包。

备注：这一步是生成一个未签名的APK按转包，如果已经有未签名的安装包，那么该步骤可以跳过

3.使用jarsigner命令对未签名的APK安装包进行签名。JDK的安装目录下的bin子目录下提供了jarsigner.exe工具进行签名。在命令行窗口输入如下命令：

```
lee@lee:~/Documents/apksigned$ jarsigner -verbose -keystore /XXX/lee.jks -signedjar hi.apk /XXX/app-debug.apk lee
```

上面命令中各选项说明如下。

-verbose:指定生成详细输出

-keystore:指定数字证书的储存路径

-signedjar:该选项的三个参数分别未签名后的APK包/未签名的APK包/数字证书的别名

输入上面命令后按回车健，将会以交互的方式让用户输入数字证书keystore的密码

```
//查看apk签名是否成功
jarsigner -verify -verbose -certs hi.apk > log.txt
```

4.使用zipalign.exe工具优化APK安装包。zipalign.exe是Android自带的一个档案整理工具，它可用于优化APK安装包，从而提升Android应用与系统之间的交互效率，提升应用程序的运行速度。

在命令行窗口输入如下命令

```
zipalign -f -v 4 hi.apk hi_zip.apk  
```

上面的命令中各选项说明如下

-f:指定强制覆盖已有的文件

-v:指定生成详细输出

 4是指定档案整理所基于的字节数，通常指定为4，也就是基于32位进行整理

hi.apk和 hi_zip.apk  分别指定整理前的APK和整理后生成的APK

运行上面命令，将会在当前目录下生成一个hi_zip.apk文件，这就是签名完成且经过优化的APK安装包，该安装包可以对外发布了。