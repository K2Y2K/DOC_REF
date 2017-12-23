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

