# Android knowledge

### 1、Parcelabel序列化

I am using following code to covert my `Drawable` to `byte array`,

```
 Bitmap bitmap = (Bitmap)((BitmapDrawable) appIcon).getBitmap();
 ByteArrayOutputStream stream = new ByteArrayOutputStream();
 bitmap.compress(Bitmap.CompressFormat.JPEG, 100, stream);
 byte[]byteArray = stream.toByteArray();
 parcel.writeInt(byteArray.length);
 parcel.writeByteArray(byteArray);

```

And to convert byte array to Drawable i am using following code:

```
  PMAppInfo pmAppInfo=new PMAppInfo();
   //将bite类型转换成Drawable类型
  final int contentBytesLen = in.readInt();
  byte[] contentBytes = new byte[contentBytesLen];
  in.readByteArray(contentBytes);
  pmAppInfo.appIcon= new BitmapDrawable(BitmapFactory.decodeByteArray(contentBytes, 0, contentBytes.length));
  pmAppInfo.appLable=in.readString();//读取appLable
```

step1:类PMAppInfo序列化之Parcelable

```
public class PMAppInfo implements Parcelable{

    private String appLable;
    private Drawable appIcon;
    private String pkgName;
    public PMAppInfo(){

    }

    protected PMAppInfo(Parcel in) {
        appLable = in.readString();
        pkgName = in.readString();
    }

    public static final Creator<PMAppInfo> CREATOR = new Creator<PMAppInfo>() {
        @Override
        public PMAppInfo createFromParcel(Parcel in) {
            PMAppInfo pmAppInfo=new PMAppInfo();
            //将bite类型转换成Drawable类型
            final int contentBytesLen = in.readInt();
            byte[] contentBytes = new byte[contentBytesLen];
            in.readByteArray(contentBytes);
            pmAppInfo.appIcon= new BitmapDrawable(BitmapFactory.decodeByteArray(contentBytes, 0, contentBytes.length));
            pmAppInfo.appLable=in.readString();//读取appLable
            pmAppInfo.pkgName=in.readString();//读取pkgName
            return  pmAppInfo;
        }

        @Override
        public PMAppInfo[] newArray(int size) {
            return new PMAppInfo[size];
        }
    };

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel parcel, int i) {
        //将Drawable转换成bite类型存储
        Bitmap bitmap = (Bitmap)((BitmapDrawable) appIcon).getBitmap();
        ByteArrayOutputStream stream = new ByteArrayOutputStream();
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, stream);
        byte[]byteArray = stream.toByteArray();
        parcel.writeInt(byteArray.length);
        parcel.writeByteArray(byteArray);
        parcel.writeString(appLable);
        parcel.writeString(pkgName);

    }

    public String getAppLable() {
        return appLable;
    }

    public void setAppLable(String appLable) {
        this.appLable = appLable;
    }

    public Drawable getAppIcon() {
        return appIcon;
    }

    public void setAppIcon(Drawable appIcon) {
        this.appIcon = appIcon;
    }

    public String getPkgName() {
        return pkgName;
    }

    public void setPkgName(String pkgName) {
        this.pkgName = pkgName;
    }

}
```

step2:传递Parcelable对象：

```
PMAppInfo appInfos=new PMAppInfo();
Intent intent=new Intent(getApplicationContext(),AppMoreInfoActivity.class);
intent.putExtra("APP_INFO", (Parcelable) appInfos);
startActivity(intent);
 }
```

step3:解析Parcelable对象：

```
ImageView appIcon;
TextView appPkgName,appLable;
//获取intent数据
Intent intent=getIntent();
Bundle bundle=intent.getExtras();
PMAppInfo appmoreinfo=bundle.getParcelable("APP_INFO");
//获取对象中方法
appIcon=findViewById(R.id.app_icon);
appPkgName=findViewById(R.id.appPkgName);
appLable=findViewById(R.id.appLable);
appIcon.setImageDrawable(appmoreinfo.getAppIcon());
appPkgName.setText(appmoreinfo.getPkgName());
appLable.setText(appmoreinfo.getAppLable());
```

2、Button控件的点击事件之三种实现方式：

method1:匿名内部类

```
bn1.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                // 在此处监听
                System.out.println("我的按钮被点击了");
            }
        });
```



method2:外部类

新建一个外部类，实现OnClickListener接口，并重写onClick方法。

```
class MyOnClickListener implements OnClickListener{
    @Override
    public void onClick(View v) {
        Log.i("tag", "???");
    }

}
```

然后在OnCreate函数中，创建该对象实例并使用。

```
MyOnClickListener listener =  new MyOnClickListener(){
                @Override
                public void onClick(View v) {
                    // TODO Auto-generated method stub
                    super.onClick(v);
                    Toast.makeText(MainActivity.this, "!!!", 1).show();
                }
            }
    bn1.setOnClickListener(listener);
```

method3:接口方式

```
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button bn1=findViewById(R.id.bn1);
        bn1.setOnClickListener(this);
    }
  @Override
    public void onClick(View view) {
      switch (view.getId()){
            case R.id.bn1:
                flag=ALL_APP;
                break;
            case R.id.bn2:
                flag=SYSTEM_APP;
                break;
            default:
                flag=0;
                break;
        }
        appAdapter=new AppAdapter(getApplicationContext(),getAppInfo(flag));
        app_list.setAdapter(appAdapter);
    }
}
```

### 2、反编译APK

Android的反编译分成两个部分：

1. 一个是对**代码**反编译，也就是java文件的反编译。
2. 一个是对**资源**反编译，也就是res文件的反编译。

所需工具：

![img](https://upload-images.jianshu.io/upload_images/1727535-cb086dc8637fdcb3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```
Android Studio：安卓开发IDE
下载地址：https://developer.android.com/studio/index.html

反编译代码的工具：

dex2jar: 把dex文件转成jar文件
下载地址：https://sourceforge.net/projects/dex2jar/files/

jd-gui: 这个工具用于将jar文件转换成java代码
下载地址：http://jd.benow.ca/

反编译资源的工具：

APKTool: APK逆向工具，使用简单
下载地址: http://ibotpeaches.github.io/Apktool/install/

```

**反编译代码：**

`AndroidStudio`打包好的`APK文件`的后缀，需改为`.zip`，然后`解压`。从解压的文件中找到**classes.dex**文件，并将其放入**dex2jar**同一目录下，(或者与d2j_invoke.sh、d2j-dex2jar.sh、lib文件放入新文件夹seeapp下)，执行

```
lee@lee:~/seeapp$ sh d2j-dex2jar.sh classes.dex
dex2jar classes.dex -> ./classes-dex2jar.jar
lee@lee:~/seeapp$ 
```

然后我们会得到一个classes-dex2jar.jar文件，我们借助JD-GUI工具打开即可。

**反编译资源：**

**解码**

apktool下载完成后有一个.sh文件和.jar文件，

```
sh apktool.sh d see.apk
or
sh apktool.sh apktool d see.apk
```

d是decode的意思，表示我们要对see.apk解码.

生成对应的see文件夹。包含res、smail、original文件夹和AndroidManifest.xml、apktool.yml文件。

主要目录说明：

- AndroidManifest.xml：描述文件
- res：资源文件
- smail：反编译出来的所有代码，语法与java不同，类似汇编，是Android虚拟机所使用的寄存器语言

**重新打包**

```
sh apktool.sh b see -o newsee.apk
```

其中b是build的意思，表示我们要将see文件夹打包成APK文件，-o用于指定新生成的APK文件名，这里新的文件叫作Newsee.apk。但这个apk 还不可以安装，需要对它进行签名。

重新签名

利用已有的签名文件签名

```
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore 签名文件名 -storepass 签名密码 待签名的APK文件名 签名的别名
【jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore lee.jks -storepass lee0123 -signedjar  newseea.apk  newsee.apk lee】
or
jarsigner -verbose -keystore 签名文件名 -signedjar 签名apk的新名字 待签名的APK文件名 签名的别名
```

