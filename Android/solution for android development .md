# solution for android development 

**Problem :**

**Description:**

**Solution:**

### 1、

**Problem :**

Error running app: Instant Run requires 'Tools | Android | Enable ADB integration' to be enabled

**Description:**

instant run是即时安装,需要同时勾上两个地方才可以开启即时安装.

**Solution:**

step1:Preferences（win中对应“Set”）----->Build，Execution，Deployment----->Instant Run, checked for Enable Instant Run....

step2:checked for Tools -> Adnroid -> enable ADB integration;

### 2、

**Problem :**

Error obtaining UI hierarchy
Error while obtaining UI hierarchy XML file: com.android.ddmlib.SyncException: Remote object doesn't exist!

**Description:**

I am testing my app with adb, but i get this error when i execute "dump view hierarchy for uiautomator":

**Solution:**

```
adb reroot
or 
sudo adb kill-server
sudo adb start-server

or but I have no test
adb shell uiautomator dump
UI hierchary dumped to: /sdcard/window_dump.xml
把下面的代码放到bat脚本中(window 系统)，运行一次:
call adb shell uiautomator dumpcall 
call adb pull /storage/sdcard/window_dump.xml .
call window_dump.xml
```

### 3、

**Problem :**

Thread updates not enabled for selected client(use toobar button to enable)

**Description:**

AS中选择Tools—Android—Android Device Monitor）做线程分析，发现有这么的提示。

**Solution:**

Android Device Monitor下

Windows->Preferences->Android->DDMS to check the ‘Thread Updates 
Enabled by Default’

重启ADM即可。

### 4、

**Problem :**

open the ADM appears the following tips

unexpected error while parsing input: invalid ui automator hierarchy file

**Description:**

**Solution:**

未执行任何操作，正常了

网上给出的解决方案，但未实际操作：

C:\Users\Administrator\\.android中删除monitor-workspace文件，重新打开monitor就行。

即`rm -rf $HOME/.android/monitor-workspace` or remove the directory manually。