---

---

## 					第一部分 类型和语法

### 第一章 类型

#### 1.2.内置类型

> javascript有7种内置类型
>
> 1. 空值（null）
> 2. 未定义(undefined)
> 3. 布尔值(boolean)
> 4. 数字(number)
> 5. 字符串(string)
> 6. 对象(object)
> 7. 符号(symbol)

typeof运算符查看值得类型，返回类型的字符串值

> typeof undefined === "undefined" //true
> typeof true === "boolean" //true
> typeof null === "object" //true

`复合检测条件检测null值的类型：`

> var a = null;
> (!a&&typeof a==="object") //true

`还有一种情况：`

> typeof function a(){}==='function'  //true

 **这样看来function也是javasript的一个内置类型，但是实际上是object的一个子类型**

> function a(b,c){}
> a.length //2

`函数对象的length属性是其声明的参数的个数`

`再来看数组`

> typeof [1,2,3]==='object' //true

它也是object的一个子类型

#### 1.3.值和类型

`javascipt的变量是没有类型的，只有值才有，变量是可以随时持有任何类型的值，也就是语言不要求变量总是持有与其初始值同类型的值，一个变量可以现在被赋值为字符串，随后又被赋值为数字类型`



### 第二章 值

#### 2.1数组

**数组可以容纳任何类型的值，可以是字符串，数字和对象**

`创建稀疏数组时要注意：`

> var a = []
>
> a[0] ==1;
>
> a[2] = [3];
>
> a[1];//undefined   不赋值默认uundefined，和自己赋值undefined还是有区别的
>
> a.length //3 

`数组的索引还可以是字符串，但是这些并不计算在长度中`

> var a = []
>
> a[0] = 1;
>
> a["foobar"] =2;
>
> a.length //1
>
> a["foobar"] //2
>
> a.foobar //2

`还有一个问题要特别注意，如果字符串键值能够被强制类型转换成十进制的话，就会被当做数字索引来处理`

> var a = []
>
> a["13"] = 42;
>
> a.length //14

**类数组(一组通过数组索引的值)**

转换成真正的数组： 

> function foo(){
>
> ​     var arr = Array.prototype.slice.call(arguments);
>
> ​	arr.push("bam")
>
> }
>
> foo("bar","baz")  //["bar","baz","bam"]



#### 2.2字符串

**字符串和数组都是类数组，都有length属性以及IndexOf和concat方法，但是JS中字符串是不可变的，数组是可变的，不可变的意思是字符串的成员函数不会改变原始值，而是穿件并返回一个新的字符串，数组的成员函数是在其原始值上进行操作，另外一个不同点在于字符串反转，数组有一个字符串没有的reverse(),字符串的做法**：

> var c =a.split("").reverse().join("")  将a的值转换为字符数组，将数组中的字符进行反转，再将数组中的字符拼接回字符串



#### 2.3数字

**方法：tofixed()方法可指定小数部分的显示位数**

> var a = 42.59;
>
> a.toFixed(0); //"43"
>
> a.toFixed(1);//42.6

怎样判断0.1+0.2和0.3是否相等？

> Math.abs(n1-n2)<Number.EPSILON

**特殊的数字**

NaN是一个特殊值，它和自身不相等，是唯一一个非自反的值

> NaN!=NaN //true

区分正0和负0

> function isNegZeo(n){
>
> ​	n = Number(n);
>
> ​	return (n===0)&&(1/n === -Infinity);
>
> }
>
> isNegZeo(-0); //true
>
> isNegZeo(0); //false

**特殊的等式**

ES6中引入一个工具方法`Object.is()`来判断两个值是否绝对相等

> var a = 2/"foo";
>
> var b = -3*0;
>
> Object.is(a,NaN); //true
>
> Object.is(b,-0) //true
>
> Object(b,0); //false

**值和引用**

> function foo(x){
>
> ​	x.push(4);
>
> ​	x;//[1,2,3,4]
>
> ​	x = [4,5,6];  //如果要改变a的值，必须更改x指向的数组，而不是为x赋新值
>
> ​	x.push(7);
>
> ​	x;//[4,5,6,7]
>
> }
>
> var a = [1,2,3];
>
> foo(a);
>
> a;//[1,2,3,4]

> function foo(x){
>
> ​	x.push(4);
>
> ​	x;//[1,2,3,4]
>
> ​	x.length = 0;//清空数组
>
> ​	x.push(4,5,6,7);
>
> ​	x;//[4,5,6,7]
>
> }
>
> var a = [1,2,3]
>
> foo(a);
>
> a;//[4,5,6,7]

`基本类型通过值复制来赋值，对象通过引用复制来赋值。`

### 第三章 原生函数

> **常见的原生函数有:**
>
> String()
>
> Number()
>
> Boolean()
>
> Array()
>
> Object()
>
> Function()
>
> RegExp()
>
> Date()
>
> Error()
>
> Symbol()

**原生函数可以被当做构造函数来使用**

> var a = new String("abc")
>
> typeof a //"object",不是"String"
>
> a instanceof String //true
>
> Object.prototype.toString.call(a); //"[object String]"

new String("abc")创建的是字符串"abc"的封装对象，而不是基本类型

#### 3.1内部属性

> Object.prototype.toString.call(null) ;//"[object Null]"
>
> Object.prototype.toString.call(undefined);//"[object Undefined]"

#### 3.2封装对象包装

​	由于基本类型没有.length和.toString()这样的这样的属性和方法，需要通过封装对象才能访问，此时js会自动为基本类型封装对象：

> var a = "abc"
>
> a.length; //3
>
> a.toUpperCase();//abc

**封装对象注意**

> var a = new Boolean(false);
>
> if(!a){
>
> ​	console.log("a") //执行不到这里
>
> }

**为false创建了一个封装对象，但是该值总是返回boolean,包装之后返回的结果就不一样**

如果想要自行封装基本类型值，可以使用Object()函数，不用new关键字

> var a = "abc"
>
> typeof a; //"string"
>
> a.instanceof String //true

#### 3.3拆封

如果想要得到封装对象的中的基本类型，使用valueOf()函数

> var a = new String("abc")
>
> var b = new Number(43)
>
> a.valueof() //"abc"
>
> b.valueof() //43

#### 3.4.Date()

获取Unix时间戳

> (new Date()).getTime()

### 第四章 强制类型转换

#### 4.1.JSON字符串化

JSON字符串化并非严格意义上的强制类型转换，对于大多数简单之来说，JSON字符串化和toSting()

的效果基本相同

> JSON.stringify(42) //"42"
>
> JSON.stringify(null) ;//"null"
>
> JSON.stringify(true)//"true"

不安全的JSON值：undefined,function,symbol和包含循环引用都不符合JSON结构标准

> JSON.stringify(undefined) //undefined
>
> JSON.sttringify(function(){})  //undefined
>
> var a = {
>
> ​	b: 42;
>
> ​	d: function(){}
>
> }
>
> JSON.stringify(a); //循环引用这里会产生错误
>
> 

#### 4.2.ToNumber

ES5规范是先检查该值是否有valueof()方法，如果有并且返回基本类型值，就使用该值进行强制类型转换，没有就使用toString()的返回值进行强制类型转换

> var a = {
>
> ​	valueOf:function(){
>
> ​		return "42";
>
> ​	}
>
> }
>
> var b = {
>
> ​	toString: function(){
>
> ​		return "42";
>
> ​	}
>
> }
>
> var c = [4,2]
>
> c.toString = function(){
>
> ​	return this.join(""); //"42"
>
> }
>
> Number(a); //42
>
> Number(b); //42
>
> Number(c); //42
>
> Number("") ; 0
>
> Number([]); //0
>
> Number(["abc"]) //NaN

4.3.ToBoolean

​	一下布尔强制类型转换的结果是false

`undefined`

`null`

`false`

`+0 -0和NaN`

`""`

**假值对象**

> var a = new Boolean(false);
>
> var b = new Number(0);
>
> var c = new String("")
>
> var d = Boolean(a&&b&&c);
>
> d;//true

**真值**

> var a = "false"
>
> var b = "0"
>
> var c = "''"
>
> var d= Boolean(a&&b&&c)
>
> d//true

#### 4.3.强制类型转换

**日期显式转换为数字**

一元运算符将日期转数字

> var d = new Date("Mon, 18 Aug 2014 08:53:06 CDT")
>
> +d  //1408369986000

获取当前时间戳的办法：`Date.now()`，获取指定时间戳：`Date(..).getTime()`

**~运算符**

用法：

> var a = "Hello World";
>
> if(~a.indexOf("lo")){
>
> ​	//找到匹配
>
> }

**显式解析数字字符串**

> var a = "42"
>
> var b = "42px"
>
> Number(a); //42
>
> parseInt(a); //42
>
> Number(b);//NaN
>
> parseInt(b); //42

`解析允许字符串中含有非数字字符，解析按从左到右的顺序，如果遇到非数字字符就停止，而转换不允许出现非数字字符，否则会失败并返回NaN`

ES5之前的parseInt()有一个坑，如果没有第二个参数来指定转换的基数，parseInt()会根据字符串中的第一个字符来决定基数。从ES5开始parseInt()默认转换成十进制。

**字符串和数字之前的隐式强制类型转换**

> var a = "42"
>
> var b = "0"
>
> var c = 42
>
> a+b;//"420"
>
> c+d;//42

> var a = [1,2]
>
> var b = [3,4]
>
> a+b;//"1,23,4"

> var a ={
>     valueOf: function(){return 42;},
>     toString: function(){return 4;}
>
> }
> a+"";//"42"
> String(a);//"4"  直接调用toString

**||和&&** 类似于操作选择器

> var a = 42;
>
> var b = "abc";
>
> var c = null;
>
> a||b;//42
>
> a&&b;//“abc”
>
> c||b;//“abc"
>
> c&&b;//null

a||b大致相当于 a?a:b;

a&&b大致相当于a?b:a;

下面是一个十分常见||的用法：

> function foo(a,b){
>
> ​	a = a||"hello";
>
> ​	b = b||"world"
>
> ​	console.log(a+""+b);
>
> }
>
> foo();//"hello world"
>
> foo("yeah","yeah") //"yeah yeah!"

**Symbol的强制类型转换**

ES6允许从符号到字符串的显式强制类型转换，隐式类型转换会产生错误

> var a1 = Symbol("cool");
>
> String(s1); //"Symbol(cool)"
>
> var s2 = Symbol("not cool")
>
> s2+"" //TypeError

其他类型和布尔类型之前的比较

> var a = "42"
>
> var b = true
>
> a==b //false

规范是这样说的：

如果Type(x)是布尔类型，则返回ToNumber(x)==y的结果

如果Type(y)是布尔类型，返回x == ToNumber(y)的结果

**null和undefined之间的相等比较**

规范：a.x为null，y为undefined，结果为true

b.x为undefined,y为null,结果为true

> var a = null;
>
> var b ;
>
> a==b; //true
>
> a==null;//true
>
> b==null;//true
>
> a==false; //false
>
> b==false;
>
> a=="";//false
>
> b=="";//false
>
> a==0;//false
>
> b==0;//false

### 第五章 语法

#### 5.1语句的结果值

ES7提案

> var a,b;
>
> a = do{
>
> ​	if(true){
>
> ​		b = 4+38
>
> ​	}
>
> a;//43

**语句的副作用**

> var a = 42;
>
> var b = a++;
>
> a;//43
>
> b；//42

**try..finally**

finally中的代码总是会在try之后执行，如果有catch就在catch之后执行，也可以将finally中的代码看作是一个回调函数，无论出现什么情况一定会被调用

如果try中有return 语句：

> function foo(){
>
> ​	try{
>
> ​		return 42;	
>
> ​	}
>
> ​	finally{
>
> ​		console.log("Hello")
>
> ​	}
>
> ​	console.log("never runs")
>
> }
>
> console.log(foo())
>
> //Hello
>
> //42

如果finally中抛出异常，函数就会在此处终止，如果此前try中已经有return设置了返回值，则会被丢弃

> function foo(){
>
> ​	try{
>
> ​		return 42;	
>
> ​	}
>
> ​	finally{
>
> ​		throw "Op!"
>
> ​	}
>
> ​	console.log("never runs")
>
> }
>
> console.log(foo()
>
> //Uncaught Exception:Op!

finally中的return 会覆盖try和catch中return 的返回值

> function foo(){
>
> ​	try{
>
> ​		return 42;
>
> ​	}
>
> ​	finally{
>
> ​		return;
>
> ​	}
>
> }
>
> foo() ;//undefined 覆盖前面的return 42

通常来说，在函数中省略return的结果和return;及return undefined结果是一样的，但是在finally中省略return会返回前面return 设定的返回值

> function foo(){
>
> ​	bar: {
>
> ​		try{
>
> ​			return 42;
>
> ​		}
>
> ​		finally{
>
> ​			break bar;
>
> ​		}
>
> ​		console.log("Crazy");
>
> ​		return "Hello"
>
> ​	}
>
> }
>
> console.log(foo()) 
>
> //Crazy
>
> //Hello

## 第二部分 异步和性能

#### 第三章 Promise

**Promise.all([])**

假定你要同时发送两个Ajax请求，等它们不管以什么顺序全部完成之后，再发送第三个Ajax请求

> var p1 = request("xx")
>
> var p2 = request("yy")
>
> Promise.all([p1,p2]).then(function(){
>
> ​	return request('zz')
>
> }).then(function(msg){
>
> ​	console.log(msg)
>
> }

Promise.all([..])返回的主Promise在且仅在所有的成员promise都完成后才会完成，如果这些promise中有任何一个被拒绝的话，主Promise.all([])就会立即被拒绝，并丢弃来自其他所有promise的结果。

**Promise.race([..])**

> var p1 = request("xx")
>
> var p2 = request("yy")
>
> Promise.race([p1,p2]).then(function(){
>
> ​	return request('zz')
>
> }).then(function(msg){
>
> ​	console.log(msg)
>
> }

**finally**

promise需要一个finally()回调注册，这个回调Promise决议后总是会被调用，并且允许你执行任何必要的清理工作。

**promise的缺点**

Promise 可以很好地处理单一异步结果，不适用于：

- `多次触发的事件：如果要处理这种情况，可以了解一下响应式编程（ reactive programming ），这是一种很聪明的链式的处理普通事件的方法。`
- `数据流：支持此种情形的[标准](https://streams.spec.whatwg.org/)正在制定中。`

ECMAScript 6 Promise 缺少两个有时很有用的特性：

- `不能取消执行。`
- `无法获取当前执行的进度信息（比如，要在用户界面展示进度条）。`