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
//软硬件信息
SystemProperty
STARNAUTE4:/proc $ cat cpuinfo


```

### 4、AM命令

```
lidongxue@lidongxue:~$ adb shell am start -n com.example.lidongxue.myapplication/com.example.lidongxue.myapplication.MainActivity
Starting: Intent { cmp=com.example.lidongxue.myapplication/.MainActivity }
lidongxue@lidongxue:~$ adb shell am start -N com.example.lidongxue.myapplication/com.example.lidongxue.myapplication.MainActivity
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.example.lidongxue.myapplication/.MainActivity }

```

