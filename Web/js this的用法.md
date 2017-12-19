

# js this的用法

#### JS中this的用法

​	情况1：如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的严格版中this指向的不是window，但是我们这里不探讨严格版的问题，你想了解可以自行上网查找。

```
//在一般函数方法中使用this指代全局对象
function test(){
  this.x=1;
  alert(this.x);
}
test();//1
```

　　情况2：如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。

```
//作为对象方法调用，this 指代上级对象
function test(){
　　alert(this.x);
}
var o = {};
o.x = 1;
o.m = test;
o.m(); // 1
```

　　情况3：如果一个函数中有this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象，

​	this永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的，例子2中虽然函数fn是被对象b所引用，但是在将fn赋值给变量j的时候并没有执行所以最终指向的是window，这和例子1是不一样的，例子1是直接执行了fn。

例子1：

```
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
window.o.fn();
```

this指向的是o对象；



```
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn();
```

this指向的是b对象；



```
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();
```

this指向的是b对象，因为b对象不存在a变量，所以为未定义；

例子2：

```
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```

this指向的是window。例子2中虽然函数fn是被对象b所引用，但是在将fn赋值给变量j的时候并没有执行所以最终指向的是window。

例子3：

```
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
```



```
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this);　　//Window
}
window.a();
```

this最终指向的是调用它的对象，这里的函数a实际是被Window对象所点出来的，alert也是window的一个属性

例子4：

```
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();
```

this的对象是对象o;

例子5：**构造函数版this(**作为构造函数调用，this 指代new 出的对象**)：**



```
function Fn(){
    this.user = "追梦子";
}
var a = new Fn();
console.log(a.user); //追梦子
```

这里之所以对象a可以点出函数Fn里面的user是因为new关键字可以改变this的指向，将这个this指向对象a，为什么我说a是对象，因为用了new关键字就是创建一个对象实例，理解这句话可以想想我们的例子3，我们这里用变量a创建了一个Fn的实例（相当于复制了一份Fn到对象a里面），此时仅仅只是创建，并没有执行，而调用这个函数Fn的是对象a，那么this指向的自然是对象a，那么为什么对象a中会有user，因为你已经复制了一份Fn函数到对象a中，用了new关键字就等同于复制了一份。

例子6：

**更新一个小问题当this碰到return时**



```
function fn()  
{  
    this.user = '追梦子';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
```

再看一个



```
function fn()  
{  
    this.user = '追梦子';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined
```



再来

```
function fn()  
{  
    this.user = '追梦子';  
    return 1;
}
var a = new fn;  
console.log(a.user); //追梦子
```



```
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //追梦子
```

**如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。**

```
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a); //fn {user: "追梦子"}
```

还有一点就是虽然null也是对象，但是在这里this还是指向那个函数的实例，因为null比较特殊。

```
function fn()  
{  
    this.user = '追梦子';  
    return null;
}
var a = new fn;  
console.log(a.user); //追梦子
```



**知识点补充：**

**　　**1.在严格版中的默认的this不再是window，而是undefined。

　　2.new操作符会改变函数this的指向问题，虽然我们上面讲解过了，但是并没有深入的讨论这个问题，网上也很少说，所以在这里有必要说一下。

```
function fn(){
    this.num = 1;
}
var a = new fn();
console.log(a.num); //1
```

　　为什么this会指向a？首先new关键字会创建一个空的对象，然后会自动调用一个函数apply方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象替代。

　　注意: 当你new一个空对象的时候,js内部的实现并不一定是用的apply方法来改变this指向的,这里我只是打个比方而已.

　　if (this === 动态的\可改变的) return true;

（以上摘自http://www.cnblogs.com/pssp/p/5216085.html）

#### JS中call/apply.bind方法

自行改变this的指向请看[JavaScript中call,apply,bind方法的总结](http://www.cnblogs.com/pssp/p/5215621.html)



```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);
    }
}
var b = a.fn;
b(); //undefined
//我们是想打印对象a里面的user却打印出来undefined是怎么回事呢？如果我们直接执行a.fn()是可以的。
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);
    }
}
a.fn(); //追梦子
```

**将一个对象保存到另外的一个变量中，那么就可以通过以下方法：**

**1、call()**

```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
var b = a.fn;
b.call(a);
```

通过在call方法，给第一个参数添加要把b添加到哪个环境中，简单来说，this就会指向那个对象。

call方法除了第一个参数以外还可以添加多个参数，如下：

```
var a = {
    user:"追梦子",
    fn:function(e,ee){
        console.log(this.user); //追梦子
        console.log(e+ee); //3
    }
}
var b = a.fn;
b.call(a,1,2);
```

**2、apply()**

apply方法和call方法有些相似，它也可以改变this的指向

```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
var b = a.fn;
b.apply(a);
```

同样apply也可以有多个参数，但是不同的是，第二个参数必须是一个数组，如下：

```
var a = {
    user:"追梦子",
    fn:function(e,ee){
        console.log(this.user); //追梦子
        console.log(e+ee); //11
    }
}
var b = a.fn;
b.apply(a,[10,1]);
```

or

```
var a = {
    user:"追梦子",
    fn:function(e,ee){
        console.log(this.user); //追梦子
        console.log(e+ee); //520
    }
}
var b = a.fn;
var arr = [500,20];
b.apply(a,arr);
```

//注意如果call和apply的第一个参数写的是null，那么this指向的是window对象

```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this); //Window {external: Object, chrome: Object, document: document, a: Object, speechSynthesis: SpeechSynthesis…}
    }
}
var b = a.fn;
b.apply(null);
//输出：undefined
```

**3、bind()**

bind方法和call、apply方法有些不同，但是不管怎么说它们都可以用来改变this的指向。

先来说说它们的不同吧。

```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);
    }
}
var b = a.fn;
b.bind(a);
//无结果输出
```

我们发现代码没有被打印，对，这就是bind和call、apply方法的不同，实际上bind方法返回的是一个修改过后的函数。

var a = {

    var a = {
        user:"追梦子",
        fn:function(){
            console.log(this.user);
        }
    }
    var b = a.fn;
    var c = b.bind(a);
    console.log(c); //function() { [native code] }
    
    //输出结果：[Function: bound fn]
那么我们现在执行一下函数c看看，能不能打印出对象a里面的user：

```
var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
var b = a.fn;
var c = b.bind(a);
c();
```

ok，同样bind也可以有多个参数，并且参数可以执行的时候再次添加，但是要注意的是，参数是按照形参的顺序进行的。参数可以逐个列出，也可以写在数组中。

```
var a = {
    user:"追梦子",
    fn:function(e,d,f){
        console.log(this.user); //追梦子
        console.log(e,d,f); //10 1 2
    }
}
var b = a.fn;
var c = b.bind(a,10);
c(1,2);
```

总结：call和apply都是改变上下文中的this并立即执行这个函数，bind方法可以让对应的函数想什么时候调就什么时候调用，并且可以将参数在执行的时候添加，这是它们的区别，根据自己的实际情况来选择使用。

#### **JS中的Function原型及其prototype属性的简单应用:**

(http://www.cnblogs.com/amazingbook/p/7210310.html)

Function是由function关键字定义的函数对象的原型，在原型的基础上通过prototype新增属性或方法，则以该对象为原型的实例化对象中，必然存在新增的属性或方法，而且它的内容是静态不可重载的。

```
//每一个实例化的function，都会具备一个show方法。
1 Function.prototype.show = function() 
2 {
3     return "我是一个函数的新方法";    //函数需要有返回值!
4 }
5 Function.show();  //输出=>我是一个函数的新方法     
6 Function; //Function的原型为   =>function Function() { [native code] }
7 var f = new Function(); 
8 f.show();    //输出=>我是一个函数的新方法           
```

new Function()是函数原型的一个实例化,new Function(参数1,参数2,…,参数n,函数体)，它的本意其实是通过实例化一个Function原型，得到一个数据类型为function的对象，也就是一个函数，而该变量就是函数名。

```
1 var message = new Function('msg','alert(msg)');
2 // 等价于：
3 function message(msg) {
4     alert(msg);
5 }
```





```
var zlw = {
    name: "zlw",
    sayHello: function (age) {
         console.log("hello, i am ", this.name + " " + age " years old");
     }
};

var  xlj = {
    name: "xlj",
};

zlw.sayHello(24);// hello, i am zlw 24 years old

zlw.sayHello.call(xlj, 24);// hello, i am xlj 24 years old
zlw.sayHello.apply(xlj, [24]);// hello, i am xlj 24 years old

zlw.sayHello.bind(xlj, 24)(); //hello, i am xlj 24 years old
zlw.sayHello.bind(xlj, [24])(); //hello, i am xlj 24 years old
//bind方法与call、apply最大的不同就是前者返回一个绑定上下文的函数，而后两者是直接执行了函数。由于这个原因，上面的代码也可以这样写:

zlw.sayHello.bind(xlj)(24); //hello, i am xlj 24 years old
zlw.sayHello.bind(xlj)([24]); //hello, i am xlj 24 years old

```

bind方法还可以这样写 

```
fn.bind(obj, arg1)(arg2)
```

 用一句话总结bind的用法：**该方法创建一个新函数，称为绑定函数，绑定函数会以创建它时传入bind方法的第一个参数作为this，传入bind方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。**



#### **如何理解以下这段代码：**

(https://www.cnblogs.com/xljzlw/p/3775162.html)

```
var bind = Function.prototype.call.bind(Function.prototype.bind);
```

我们可以这样理解这段代码：

```
var bind = fn.bind(obj)
```

 fn 相当于 Function.prototype.call ， obj 相当于 Function.prototype.bind。而 fn.bind(obj) 一般可以写成这样 obj.fn ，为什么呢？因为 fn 绑定了 obj ， fn 中的 this 就指向了 obj 。我们知道，函数中 this 的指向一般是指向调用该函数的对象。所以那段代码可以写成这样:

```
var bind = Function.prototype.bind.call;
```

大家想一想 Function.prototype.call.bind(Function.prototype.bind) 返回的是什么？

```
console.log(Function.prototype.call.bind(Function.prototype.bind)) // call()
```

返回的是 call 函数，但这个 call 函数中的上下文的指向是 Function.prototype.bind 。这个 call 函数可以这样用



```
var bind = Function.prototype.call.bind(Function.prototype.bind);

var zlw = {
 name: "zlw" 
};

function hello () {
  console.log("hello, I am ", this.name);
}

bind(hello, zlw)() // hello, I am zlw[
```

大家可能会感到疑惑，为什么是这样写 bind(hello, zlw) 而不是这样写 bind(zlw, hello) ？既然 Function.prototype.call.bind(Function.prototype.bind) 相当于 Function.prototype.bind.call ，那么先来看下 Function.prototype.bind.call 怎么用。 call 的用法大家都知道：

```
Function.prototype.bind.call(obj, arg)
```

其实就相当于 obj.bind(arg) 。我们需要的是 hello 函数绑定对象 zlw ，即 hello.bind(zlw) 也就是 Function.prototype.bind.call(hello, zlw) ，所以应该这样写 bind(hello, zlw) 。

 

现在又有一个疑问，既然 Function.prototype.call.bind(Function.prototype.bind) 相当于 Function.prototype.bind.call ，我们为什么要这么写：

```
var bind = Function.prototype.call.bind(Function.prototype.bind);
```

而不直接这样写呢：

```
var bind = Function.prototype.bind.call;
```

先来看一个例子：



```
var name = "xlj";
var zlw = {
    name: "zlw" 
    hello: function () {
        console.log(this.name);
    }
};
zlw.hello(); // zlw

var hello = zlw.hello;
hello(); // xlj
```



有些人可能会意外， hello() 的结果应该是 zlw 才对啊。其实，将 zlw.hello 赋值给变量 hello ，再调用 hello() ， hello 函数中的 this 已经指向了 window ，与 zlw.hello 不再是同一个上下文，而全局变量 name 是 window 的一个属性，所以结果就是 xlj 。再看下面的代码：

```
var hello = zlw.hello.bind(zlw);
hello(); // zlw
```

结果是 zlw ，这时 hello 函数与 zlw.hello 是同一个上下文。其实上面的疑惑已经解开了，直接这样写：

```
var bind = Function.prototype.bind.call;
```

 bind 函数中的上下文已经与 Function.prototype.bind.call 中的不一样了，所以使用 bind 函数会出错。而这样写

```
var bind = Function.prototype.call.bind(Function.prototype.bind);
```

 bind 函数中的上下文与 Function.prototype.call.bind(Function.prototype.bind) 中是一样的。





#### [深入理解javascript原型和闭包（完结）](http://www.cnblogs.com/wangfupeng1988/p/3977924.html)

###### 一切（引用类型）都是对象，对象是**属性的集合**

```
 function show(x) {

            console.log(typeof x);    // undefined
            console.log(typeof 10);   // number
            console.log(typeof 'abc'); // string
            console.log(typeof true);  // boolean

            console.log(typeof function () {});  //function

            console.log(typeof [1, 'a', true]);  //object
            console.log(typeof { a: 10, b: 20 });  //object
            console.log(typeof null);  //object
            console.log(typeof new Number(10));  //object
        }
        show();
```

在typeof的输出类型中，其上面的四种（undefined, number, string, boolean）属于简单的值类型，不是对象。剩下的几种情况——函数、数组、对象、null、new Number(10)都是对象。（function和object都是对象）他们都是引用类型。

判断一个变量是不是对象非常简单。值类型的类型判断用typeof，引用类型的类型判断用instanceof。

```
var fn = function () { };
console.log(fn instanceof Object);  // true
```



###### 对象都是通过函数来创建的

```
       //var obj = { a: 10, b: 20 };
        //var arr = [5, 'x', true];

        var obj = new Object();
        obj.a = 10;
        obj.b = 20;

        var arr = new Array();
        arr[0] = 5;
        arr[1] = 'x';
        arr[2] = true;
```

而其中的 Object 和 Array 都是函数：

```
console.log(typeof (Object));  // function
console.log(typeof (Array));  // function
```



###### prototype原型

每个函数都有一个属性叫做prototype，这个prototype的属性值是一个对象（属性的集合，再次强调！），默认的只有一个叫做constructor的属性，指向这个函数本身。

```
  function Fn() { }
        Fn.prototype.name = '王福朋';
        Fn.prototype.getYear = function () {
            return 1988;
        };

        var fn = new Fn();
        console.log(fn.name);
        console.log(fn.getYear());
```

即，Fn是一个函数，fn对象是从Fn函数new出来的，这样fn对象就可以调用Fn.prototype中的属性。

###### 隐式原型

每个函数function都有一个prototype，即原型。这里再加一句话——每个对象都有一个__proto__，可成为隐式原型。

var obj={};

obj.___proto__和Object.prototype的属性一样！

每个对象都有一个__proto__属性，指向创建该对象的函数的prototype。

在说明“Object prototype”之前，先说一下自定义函数的prototype。自定义函数的prototype本质上就是和 var obj = {} 是一样的，都是被Object创建，所以它的__proto__指向的就是Object.prototype。

Function.prototype指向的对象，它的__proto__也指向Object.prototype。

但是Object.prototype确实一个特例——它的__proto__指向的是null，切记切记！

![img](http://images.cnitblog.com/blog/138012/201409/181512068463597.png)





###### 继承--instanceof

由于所有的对象的原型链都会找到Object.prototype，因此所有的对象都会有Object.prototype的方法。这就是所谓的“继承”。

![img](http://images.cnitblog.com/blog/138012/201409/181637013624694.png)



访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着__proto__这条链向上找，这就是原型链。



```
console.log(f1);//function f1(){}
function f1(){};//函数声明

console.log(f2);    //undefined
var f2=funlction(){ };//函数表达式

var a; //声明 默认值是undefined
console.log(a);
 a=10; //赋值  //undefined

console.log(this); //{}
```

我们总结一下，在“准备工作”中完成了哪些工作：

- 变量、函数表达式——变量声明，默认赋值为undefined；
- this——赋值；
- 函数声明——赋值；

这三种数据的准备情况我们称之为“执行上下文”或者“执行上下文环境”。

对于函数来说，上下文环境是在调用时创建的。

除了全局作用域，只有函数才能创建作用域。作用域在函数定义时就已经确定了，而不是在函数调用时确定。

在A作用域中使用的变量x，却没有在A作用域中声明（即在其他作用域中声明的），对于A作用域来说，x就是一个自由变量。

自由变量跨作用域取值，要到创建这个函数的那个作用域中取值——是“创建”，而不是“调用”，切记切记——其实这就是所谓的“静态作用域”。





###### 闭包

1.函数作为返回值，即读取函数内部的变量：

```
function fn(){
	var max=10;
	return function bar(x){
		if(x> max){
			console.log(x);
		}
	};
}
var f1=fn();
f1(15);
//输出结果为15
```

2.函数作为参数被传递，

```
var max =10,
fn=function(x){
	if(x>max){
		console.log(x);
	}
};
(function (f){
	var max =100;
	f(16);
})(fn);
//输出结果为16；
```

闭包是能够读取其他函数内部变量的函数，由于JavaScript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。所以，在本质上，闭包就是将函数内部和函数外部连接起来的桥梁。

```
　function f1(){
　　　　var n=999;
　　　　nAdd=function(){n+=1}
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　nAdd();
　　result(); // 1000
```

在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

#### AJAX

服务器请求：

get请求：

```
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

post请求：

```
xmlhttp.open("POST","demo_post.asp",true);
xmlhttp.send();
or
xmlhttp.open("POST","ajax_test.asp",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&lname=Gates");
```



| 方法                                 | 描述                                       |
| ---------------------------------- | ---------------------------------------- |
| setRequestHeader(*header*,*value*) | 向请求添加 HTTP 头。*header*: 规定头的名称*value*: 规定头的值 |



| 方法                           | 描述                                       |
| ---------------------------- | ---------------------------------------- |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。*method*：请求的类型；GET 或 POST*url*：文件在服务器上的位置*async*：true（异步）或 false（同步） |
| send(*string*)               | 将请求发送到服务器。*string*：仅用于 POST 请求           |

服务器响应：responseXML属性

```
<script type="text/javascript">
function loadXMLDoc()
{
var xmlhttp;
var txt,x,i;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    xmlDoc=xmlhttp.responseXML;
    txt="";
    x=xmlDoc.getElementsByTagName("title");
    for (i=0;i<x.length;i++)
      {
      txt=txt + x[i].childNodes[0].nodeValue + "<br />";
      }
    document.getElementById("myDiv").innerHTML=txt;
    }
  }
xmlhttp.open("GET","/example/xmle/books.xml",true);
xmlhttp.send();
}
</script>
<div id="myDiv"></div>
<button type="button" onclick="loadXMLDoc()">获得我的图书收藏列表</button>
```

服务器响应：responseText属性

```
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

服务器响应：onreadystatechange 事件

| 属性                 | 描述                                       |
| ------------------ | ---------------------------------------- |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。  |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。0: 请求未初始化1: 服务器连接已建立2: 请求已接收3: 请求处理中4: 请求已完成，且响应已就绪 |
| status             | 200: "OK"404: 未找到页面                      |

服务器响应：使用 Callback 函数

```
<script type="text/javascript">
var xmlhttp;
function loadXMLDoc(url,cfunc)
{
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=cfunc;
xmlhttp.open("GET",url,true);
xmlhttp.send();
}
function myFunction()
{
loadXMLDoc("/ajax/test1.txt",function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  });
}
</script>

<div id="myDiv"><h2>Let AJAX change this text</h2></div>
<button type="button" onclick="myFunction()">通过 AJAX 改变内容</button>

```

#### cookie

```
<html>
<head>
<script type="text/javascript">
function getCookie(c_name)
{
if (document.cookie.length>0)
{ 
c_start=document.cookie.indexOf(c_name + "=")
if (c_start!=-1)
{ 
c_start=c_start + c_name.length+1 
c_end=document.cookie.indexOf(";",c_start)
if (c_end==-1) c_end=document.cookie.length
return unescape(document.cookie.substring(c_start,c_end))
} 
}
return ""
}

function setCookie(c_name,value,expiredays)
{
var exdate=new Date()
exdate.setDate(exdate.getDate()+expiredays)
document.cookie=c_name+ "=" +escape(value)+
((expiredays==null) ? "" : "; expires="+exdate.toGMTString())
}

function checkCookie()
{
username=getCookie('username')
if (username!=null && username!="")
  {alert('Welcome again '+username+'!')}
else 
  {
  username=prompt('Please enter your name:',"")
  if (username!=null && username!="")
    {
    setCookie('username',username,365)
    }
  }
}
</script>
</head>
<body onLoad="checkCookie()">
</body>
</html>

```

#### Arguments

Javascript并没有重载函数的功能，但是Arguments对象能够模拟重载。Javascript中每个函数都会有一个Arguments对象实例arguments，它引用着函数的实参，可以用数组下标的方式"[]"引用arguments的元素。arguments.length为函数实参个数，arguments.callee引用函数自身。

arguments特性：

arguments对象和Function是分不开的。因为arguments这个对象不能显式创建，arguments对象只有函数开始时才可用。

arguments使用方法：

虽然arguments对象并不是一个数组，但是访问单个参数的方式与访问数组元素的方式相同

例如：

**arguments**[0],**arguments**[1],。。。。。。。。**arguments**[n],

 

在js中 不需要明确指出参数名，就能访问它们，例如：

```
function test() {
        var s = "";
        for (var i = 0; i < arguments.length; i++) {
            alert(arguments[i]);
            s += arguments[i] + ",";
        }
        return s;
    }
    test("name", "age")

输出结果：
name,age

```

用0到arguments.length-1来枚举每一个元素。下面我们来看看callee属性，返回正被执行的**Function **对象，也就是所指定的 **Function **对象的正文。**callee** 属性是 **arguments **对象的一个成员，仅当相关函数正在执行时才可用。
**callee** 属性的初始值就是正被执行的 **Function **对象，这允许匿名的递归函数。

```
var sum = function (n) {
        if (1 == n) {
            return 1;
        } else {
            return n + arguments.callee(n - 1);
        }
    }
    alert(sum(6));
    //结果为21
```

 通俗一点就是，**arguments**此对象大多用来针对同个方法多处调用并且传递参数个数不一样时进行使用。根据**arguments**的索引来判断执行的方法。

#### Arrow function

箭头函数是ECMAScript 6最受关注的更新内容之一。它引入了一种用「箭头」（=>）来定义函数的新语法。箭头函数与传统的JavaScript函数主要区别在于以下几点：
1.对 this 的关联。函数内置 this 的值，取决于箭头函数在哪儿定义，而非箭头函数执行的上下文环境。
2.new 不可用。箭头函数不能使用 new 关键字来实例化对象，不然会报错。
3.this 不可变。函数内置 this 不可变，在函数体内整个执行环境中为常量。
4.没有arguments对象。更不能通过arguments对象访问传入参数。只能使用显式命名或其他ES6新特性来完成。

箭头函数的确与传统函数有不同之处，但仍存在共同的特点。例如：
1.对箭头函数进行typeof操作会返回“function”。
2.箭头函数仍是Function的实例，故而instanceof的执行方式与传统函数一致。
3.call/apply/bind方法仍适用于箭头函数，但就算调用这些方法扩充当前作用域，this也依旧不会变化。
箭头函数与传统函数最大的不同之处在，禁用new操作。

语法：

```
var sum = (num1, num2) => { return num1 + num2; }
//等同于：
var sum = function(num1, num2) {
    return num1 + num2;
 };
 
 例子：
 var getTempItem = id = > ({
    id: id,
    name: "Temp"
});
// 等同于：
var getTempItem = function(id) {
    return {
        id: id,
        name: "Temp"
    };
};
```

使用例子：

```
错误：（运行时this指向了全局对象而不是PageHandler，从而造成this.doSomething()调用无效出现报错，因为全局对象中不存在doSomething方法。）
var PageHandler = {
    id: "123456",
    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type); // error
        }, false);
    },
    doSomething: function(type) {
        console.log("Handling " + type + " for " + this.id);
    }
};
正解：（通过调用函数的bind(this)，又创建了一个已关联现有this的新函数返回，就是说为了达到目的额外又包了一层。）
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type + " for " + this.id);
    }
};


```

箭头函数已经支持this关联：（this的取值即为init()内的this值。故而它等效于bind()。）

```
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
        event = > this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type + " for " + this.id);
    }
};
```

