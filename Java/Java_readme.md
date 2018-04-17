Java_readme

### 一、arraylist中的add()方法是将对象添加到arraylist的结尾处：

List list = new ArrayList();
for (int i=0; i<100000; i++)
list.add(i);//将十万个数字添加到 ArrayList 的结尾处;

set()方法是设置第X个元素为Ｘ：

// 设置第2个元素为10
list.set(1, "10");

### 二、notifyDataSetChanged() 动态更新ListView 通过 Handler AsyncTask两种方式：

（https://www.pocketdigi.com/20100827/75.html）；

### 三、字符串分隔

String str;
str＝str.substring(int beginIndex);截取掉str从首字母起长度为beginIndex的字符串，将剩余字符串赋值给str；

str＝str.substring(int beginIndex，int endIndex);截取str中从beginIndex开始至endIndex结束时的字符串，并将其赋值给str;
indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。

 示例：
查找数组中的 "Apple" 元素：
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
a 结果输出：２
以上输出结果意味着 "Apple" 元素位于数组中的第 2 个位置。

split()用法：

例如: 

想用 | 竖线去分割某字符，因 | 本身是正则表达式中的一部分，所以需要 \ 去转义，因转义使用 \, 而这个 \ 正好也是正则表达式的字符，所以还得用一个 \ , 所以需要两个 \\。

    String str="5678|XYZ";  
    String[] b = str.split("\\|");  //注意这里用两个 \\，而不是一个\  
    System.out.println("处理结果: "+b[0]+","+b[1]);   //输出的是: 处理结果:5678,XYZ。

再如：
    String str="5678|XYZ";  
    String[] b = str.split("|");  //注意直接使用|，该字符是正则表达式的一部分，  
    String x="处理结果: ";  
    for(int i=0;i<b.length;i++){  
        x=x+b[i]+",";  
    }  
    System.out.println(x);   //输出的是: 处理结果: 5,6,7,8,|,X,Y,Z,  。

### 四、charSequence

charSequence是一个接口，表示char值的一个可读序列。此接口对许多不同种类的char序列提供统一的自读访问。
此接口不修改该equals和hashCode方法的常规协定，因此，通常未定义比较实现 CharSequence 的两个对象的结果。
他有几个实现类：CharBuffer、String、StringBuffer、StringBuilder。


　　CharSequence与String都能用于定义字符串，但CharSequence的值是可读可写序列，而String的值是只读序列。


　　对于一个抽象类或者是接口类，不能使用new来进行赋值，但是可以通过以下的方式来进行实例的创建：
　　CharSequence cs="hello";

实例：

[java] view plain copy

    CharSequence cs;    //声明一个CharSequence类型的变量cs  
    Resources res=context.getResources();        //定义一个Resources类型的变量res，并给它赋值  
    cs=res.getText(R.string.styled_text);        //获得R类中指定变量的值  
    tv=(TextView)findViewById(R.id.styled_text);    //同上  
    tv.setText(cs);            //设置值  

