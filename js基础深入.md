一.js基础知识

### 1.变量类型和计算

#### **1.1js中使用typeof能得到哪些类型**？

```
答案：string、number、boolean、object、function、undefined
考点：js变量类型

补充：js 数据类型共7种  number,string,boolean,null,undefined,引用数据类型(Array,Object,Function)，Symbol (ES6 新增，表示独一无二的值)
```

##### 变量类型/存储类型：值类型和引用类型

```
{
    var a = 100
    var b = a
    a = 200
    console.log(b)//100
}
{
    var a = {age:20}
    var b = a
    b.age = 21
    console.log(a.age)//21
}
```

值类型(数值、布尔值、null、undefined )：值是保存在栈内，copy是栈中开辟一个新空间，生成一个新的地址，不会影响原数据

引用类型：对象、数组、函数

是对象的指针，值保存在堆内，在栈中也开辟空间 ，但是在栈上存放的是在堆中的地址(指针)，复制的只是指向储存对象内存的地址(引用)，没有生成新的数据，改变它的值，会影响复制了它的值的变量

##### typeof运算符详解

有6种结果

```
string、number、boolean、object、function、undefined
{
	typeof undefined //undefined
    typeof 'abv' //string
    typeof 123 //number
    typeof true //boolean
    typeof {} //object
    typeof [] //object
    typeof null //object
    typeof console.log //function
} 
```

由上可知，typeof只能区分值类型，区分不了引用类型(可以区分function)

#### 1.2何时使用 === 和 ==

```
答案：
'==='完成没有类型转换
'=='有类型转换
if(obj.a == null){
 //这里相当于obj.a === null || obj.a === undefined 简写形式
 //这是jquery源码中推荐的写法
}
除此之外，全部用===
考点：js变量计算
```

##### 变量计算：强制类型转换

```
    console.log(!!windoe.abc) //false 
{
    var a = 100 + 10 //110
    var b = 100 + '10'//10010 (相当于拼接)
}
{
    100 == '100' //true
    0 == '' //true
    null = undefined //true
}
{
    var b = 100
    if (b) {
        //if语句中，数字转换成布尔值
    }
    
    var c = ''
    if (c) {
        //if语句中，空字符串转换成false
    }
}
{
	//a&&b a为true返回b
    console.log(10 && 0) // true && 0 => 0
    
    console.log('' || 'abc') //false || 'abc' => 'abc'
   
    console.log(!windoe.abc) //true 
    console.log(!!windoe.abc) //false 
    var a = '';
    console.log(!!a) //false 
}
```

由上可知，以下4种情况会强制类型转换

- 字符串拼接 
- ==运算符(慎用==，如果换成===就不会有以上问题)
- if语句
- 逻辑运算

规则：

1. 拼接的时候，数字转换为字符串
2. ==运算符的时候，0和空字符串，null和undefinef都转换成false
3. if语句时，空字符串/0/NaN/null/undefined/false变量转换为false，非0数字都会转换成true
4. 判断一个变量是true还是false =>  !!变量

#### 1.3**js中有哪些内置函数--数据封装类对象** 

```
答案：
Object
Array
Boolean
Number
String
Fouction
Date
RegExp
Error
```

#### 1.4如何理解JSON

是一个JS对象而已

常用API

JSON.stringify  把json对象转换成数字串

JSON.parse 把将字符串转化json对象 



### 2.原型和原型链

#### **创建对象有几种方法** 

```
{
    1、从结果上看这两种算是一类（语法糖），写法不一样，一般用第一种
       var o1 ={name:'o1'};//字面量对象--默认这个对象原型链指向Object
       var o2 = new Object({name:'o2'});  //通过new Object声明的一个对象
    
    2、使用显式构造函数创建对象（通过构造函数）
       var M = function(name){ this.name = name;};
       var o3 = new M('o3');
    
    3、Object.create方法创建,是用原型链来连接的
       var P ={name:'p'};
       
       //o4创建出来是一个空对象，空对象本身没有name这个属性，它是在它的原型对象上
       var o4 = Object.create(P);
       console.log(o4); //空对象{}
       console.log(o4.__proto__ === P.prototype); //true  o4.__proto__指向的就是P对象
       console.log(o4.__proto__); //{name: "p"} 
}
```

#### 构造函数

##### 实例

只要是对象就是一个实例

##### 构造函数

任何一个函数只要被new使用了，后面这个函数就可以被叫做构造函数。
构造函数也是函数，不用new就是普通函数       

```
{
    function Foo(name,age){
        this.name = name
        this.age = age
        this.class = 'class-1;
        //默认有return this
    }
    var f =new Foo('zs',20)//可以创建多个对象
    console.log(f.name,f.age)//zs 20
}
```

##### 构造函数扩展

- var a={}其实是var a=new Object()的语法糖
- var a=[]其实是var a=new Array()的语法糖   
- function Foo(){...}其实是var Foo=new Function(...)

平常开发中，都是前面的写法

#### 原型规则和实例(是学习原型链的基础)

- 所有的引用类型(数组，对象，函数)，都具有对象特性，即可自由扩展属性（除了null）
- 所有的引用类型（数组，函数，对象），都有一个 ____proto____属性，属性值是一个普通对象
- 所有的**函数**，都有一个 prototype 属性，属性值也是一个普通的对象，这个**prototype** 指的就是**原型对象**
- 函数的 **prototype** 称**显式原型**，引用类型的**____proto____** 成为**隐式原型**
- **所有的引用类型（数组，函数，对象）/ 实例对象**，其____proto____  属性值都指向**(等于)**其**构造函数的 prototype** 属性值
- 当试图获取一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的____proto____ (即它的构造函数的prototype)中寻找

- 原型对象（构造函数.prototype）的____proto____指向的是Object的prototype

  构造函数.prototype.____proto____=== Object.prototype

- **原型的原理**：任何一个实例对象通过原型链找到它上面的原型对象，上面的方法和属性都是被实例所共享的

```
         var obj={};obj.a=100;
         var arr=[];arr.a=100;
         function fn(){}
         fn.a=100;
 		//引用类型的隐式原型
         console.log(obj.__proto__);
         console.log(arr.__proto__);
         console.log(fn.__proto__);
 		//函数的显式原型
         console.log(fn.prototype)
         
 		//obj的构造函数是Object，普通对象的构造函数是Object！！！！！！！！！！
 		隐式原型指向(等于)构造函数的显式	
         console.log(obj.__proto__===Object.prototype)//true
 
 
        function Foo(name,age){
            this.name=name;
        }             
        Foo.prototype.alertName=function(){
             alert(this.name)
        }
        var f=new Foo('zhangsan');
        f.printName=function(){
            console.log(this.name);
        }
        f.printName();//zhangsan
        //对象f本身没有alertName函数，会去它的隐式原型__proto__中寻找
        f.alertName();//zhangsan
```

```
{
      //循环对象自身的属性
        var item;
        for(item in f){
     //高级浏览器已经在for in中屏蔽了来自原型的属性
     //但是这里建议大家还是加上这个判断，保证程序的健壮性,hasOwnProperty返回一个布尔值,判断对象是否包含特定的自身(非继承)属性 
          if(f.hasOwnProperty(item))
              console.log(item);
        }
  }
```

#### 原型链

构造函数.prototype （原型对象）包含 constructor 和 _ proto_ 两个属性 

所有普通对象的原型链最终都会指向内置的 Object.**prototype**，而Object.prototype还会去 _ proto_ 里找，直到null

```
//接上一部分代码
f.toString()//[object Object]
//要去f.__proto__.__proto__中查找
//先去f对象的隐式原型__proto__上去找，因为f.__proto__指向f的构造函数Foo的显式原型prototype（上一级原型对象去找），prototype属性值也是一个对象，对象上也有__proto__属性，所有又向它的隐式原型__proto__上去找，直到找到Object的原型对象:Object.prototype
```

![](.\img\1.JPG)

#### instanceof

用于判断**引用类型**属于哪个**构造函数**的方法

**原理**：（实例对象的.____proto____）instanceof （构造函数的prototype）

f instanceof Foo的判断逻辑是：

- f的____proto____ 一层一层往上，能否对应到Foo.prototype
- 再试着判断 f instanceof Object

#### constructor

constructor来判断一个实例对象从严谨意义上是不是构造函数直接生成的实例对象

构造函数.prototype（原型对象）有constructor属性，这个属性指向了一个函数，这个函数就是构造函数。

构造函数.prototype.**constructor** === 构造函数



#### 2.1如何准确判断一个变量是数组类型

```
var arr = []
arr instanceof Array    // true
typeof无法判断 数组 返回为object

实例对象 instanceof 构造函数;//true
```

#### 2.2写一个原型链继承的例子

```
//理解简单例子
{
    function Animal(){
        this.eat = function () {
            console.log('animal eat')
        }
    }
    function Dog(){
        this.bark = function () {
            console.log('animal eat')
        }
    }
    //通过Dog原型的拓展实现了继承
    Dog.prototype = new Animal()//使dog有animal的eat属性，//Dog的原型为Animal
    var hashiqi = new Dog()
}

//补充es6写法
{
    class Animal(){
    	constructor(name) {
            this.name = name
    	}
        eat () {
            console.log(`${this.name} eat`)
        }
    }
    //继承
    class Dog extends Animal{
        constructor(name) {
        	super(name);
         	this.name = name
    	}
        bark () {
            console.log(`${this.name}`)
        }
    }
    const dog = new Dog('hashiqi')
}
```

```

实例-写一个封装DOM查询的例子
{
    function Elem(id){
        this.elem = document.getElementById(id)
    }
    Elem.prototype.html = function(val){
        var elem = this.elem
        if(val){
        	//如果有值
            elem.innerHTML = val
            return this//链式操作
        }else{
            return elem.innerHTML
        }
    }
    //扩展
    Elem.prototype.on = function(type,fn){
        var elem = this.elem
    	 if (!!type && !!fn) {
            elem.addEventListener(type, fn)
        }
    	return this
    }
    var div1 = new Elem('div1')
    //赋值
    div1.html('这是修改后的内容').on('click',function () {
        console.log('触发单击事件')
    }).html('这是修改后的内容')
}
```

#### 2.3描述new一个对象的过程

- 创建一个新对象
- this指向这个对象
- 执行代码（对this赋值）
- 返回this

#### 2.4zepto或其他框架源码如何使用原型链

**养成阅读源码的好习惯**

<http://www.imooc.com/learn/745> 

**zepto源码如何使用原型链**

```
// my-zepto.js 文件，zepto原理 
(function(window) {
    var zepto = {};
    
    //$入口函数   
    var $ = function (selector) {
        return zepto.init(selector);
    }
    //初始化函数用来转换dom形式
    zepto.init = function(selector) {
        // querySelectorAll 得到的是一个类数组
        // 通过 slice 方法把它变成一个数组dom方便使用
        var slice = Array.prototype.slice;
        //返回的dom元素是一个数组的形式
        var dom = slice.call(document.querySelectorAll(selector)); 
        return zepto.Z(dom, selector);
    }
    //zepto.Z函数初始化构造函数
    zepto.Z = function(dom, selector) {
    	//return new Z返回的构造函数Z的一个实例对象
        return new Z(dom, selector);
    }
    // Z 构造函数
    function Z(dom, selector) {
        var i, len = dom ? dom.length : 0;
        for(i = 0; i < len; i++) {
            this[i] = dom[i];
        }
        this.length = len;
        this.selector = selector || '';
    }
    
    //原型
    $.fn = {
        constructor: zepto.Z,
        
        css: function (key, value) {
            // ...
        },
        html: function(value) {
            // ...
        }
    }
    // zepto.Z 返回的就是Z的实例对象
    // Z.prototype 也就是Z的原型设置为$.fn，那么Z的所有实例就都可以使用$.fn中的方法了。
    zepto.Z.prototype = Z.prototype = $.fn
    
    window.$ = $;
})(window)

```

**jq源码如何使用原型链**   

    // my-jquery.js 文件，jquery原理 
    
    (function(window) {
    
        // 这个 jQuery === $
        
        //jQuery对象初始化构造函数
        var jQuery = function(selector) {
    
            return new jQuery.fn.init(selector);
    
        }
      	//构造函数
        var init = jQuery.fn.init = function(selector) {
            // querySelectorAll 得到的是一个类数组
            // 通过 slice 方法把它变成一个数组dom方便使用
            var slice = Array.prototype.slice;
            var dom = slice.call(document.querySelectorAll(selector));
    
            var i, len = dom ? dom.length : 0;
            for(i = 0; i < len; i++) {
                this[i] = dom[i];
            }
            this.length = len;
            this.selector = selector || '';
        }
        //原型
        jQuery.fn = jQuery.prototype = {
            constructor: jQuery,
            
            css: function (key, value) {
                // ...
            },
            html: function(value) {
                // ...
            }
    	};
        //定义原型 构造函数的原型 == jQuery.fn
        init.prototype = jQuery.fn;
    
        window.$ = jQuery;
    })(window)

#### 2.5插件机制

为什么把原型方法放在$.fn（jQuery.fn）上，而不直接给构造函数的原型赋值

![](F:\gongsi\img\8.JPG)

- 只有$/jquery会暴露在window全局
- 将插件统一扩展到$.fn.xxx这一个接口，方便使用





### 3.作用域和闭包

#### 执行上下文

- 范围：一段 <script> 或者一个**函数**之内都会生成一个上下文
- 全局：变量定义，函数声明 //执行之前，一段<script>会生成全局上下文
- 函数：变量定义，函数声明，this，arguments //函数执行之前会生成函数上下文，this/arguments在执行前就会存在确定值
```
{
    console.log(a);//undefined

    var a=100

    fn('zhangsan') //'zhangsan' 20

    function fn(name){

       age=20;

       console.log(name,age);

       var age;//在函数内声明，自动提升到函数内第一行，再执行age=20

    }

}
```

#### **this** 

- this要在执行时才能确认值，定义时无法确认

```
    var a={
       name:"A",
       fn:function(){
               console.log(this.name)
           }
        }
    a.fn()  //this===a
    a.fn.call({name:B}) //this==={name:'B'}
    var fn1=a.fn;
    fn1() //this===window
```

#### 作用域

- es5没有块级作用域，但es6有
- es5只有函数和全局作用域

#### 作用域链

- 自由变量：当前作用域没有定义的变量
- 作用域链 :函数的父级作用域是函数定义时候的作用域，不是函数执行时候的作用域,也就是说那个作用域定义了这个函数，这个函数的父级作用域就是谁，跟函数执行没有关系，函数自由变量要到父级作用域中找，就形成了作用域链 

```
{
      var a=100;
       function F1(){
          var b=200;
          function F2(){
              var c=300;
              console.log(a);//a是自由变量
              console.log(b);//b是自由变量
              console.log(c);
          }
          F2()
       }
       F1();
}
```

#### 闭包

**闭包必要条件**：

1、**内部**函数应**访问其外部**函数**中**声明的**私有变量**、参数或其他函数内部函数。（延长作用域链）

2、外部函数“**return**”该内部函数

3、定义与该外部函数同级的变量，把该外部函数**赋值**给它

```
{
     function F1(){
          var a=100;
          //返回一个函数(函数作为返回值)
          return function(){
              console.log(a);//自由变量，父作用域寻找a=100
          }
     }
     //f1得到一个函数
     var f1=F1();
     var a=200;
     f1();//100
}
```

**闭包的使用场景：**

①函数作为返回值

②函数作为参数传递(函数自由变量要到父级作用域中找)

```
{
    function fn (x) {
        var a = x
        var b = function() {
            return a    // 访问外部变量a
        }
        a++
        return b	// 需要把闭包体抛出
    }
    var c = fn(10)
	console.log(c)    // 11
}
```

#### 3.1对变量提升的理解

 ①变量的定义

 ②函数的声明（注意和函数表达式的区别）

#### 3.2说明this几种不同的使用场景

​    ①作为构造函数执行

​    ②作为对象属性执行

​    ③作为普通函数执行

​    ④call apply bind

```
{
    使用场景
    ①作为构造函数执行，构造函数的this指向创建的实例对象
       function Foo(name){
       	  console.log(this)//Foo {}
          //this={};//this指向这个对象
          this.name=name;//this赋值
          //return this//返回this
       }
       var f=new Foo('zhangsan')//创建一个新对象
    ②作为对象属性执行，this指向调用它的对象
        var obj={
            name:'A',
            printName:function(){
                console.log(this.name)
            }
        }
        obj.printName();
    ③作为普通函数执行，指向window
       function fn(){
          console.log(this)  //this===window
       }
       fn()
    ④call apply bind 指向绑定this的对象
       function fn1(name,age){
          alert(name);
          console.log(this)  //this===window
       }
       fn1.call({x:100},'zhangsan',20)//{x: 100}
       fn1.apply({x:100},['zhangsan',20])//{x: 100}
       var fn2=function(name,age){
          alert(name);
          console.log(this)  //this==={x:100}
       }.bind({x:100})//bind只能用函数表达式，函数声明不可用，会报错
       fn2('zhangsan',200)
}
```

#### 3.创建10个<a>标签，点击的时候弹出来对应的序号

```(
{
//立即执行函数，不用调用，自己执行
    for(var i=0;i<10;i++){
        function(i){
        	//函数作用域
            var a = document.createElement('a');
            a.innerHTML = i + '</br>';
            a.addEventListener('click',function(e){
                e.preventDefault;//阻止事件默认行为
                alert(i);//自由变量，去父作用域获取值
            })
            document.body.appendChild(a)
        }(i)
    }
}
```

#### 3.4如何理解作用域

①自由变量

②作用域链，即自由变量的查找

③闭包的两个场景

#### 3.5实际开发中闭包的作用

- 闭包实际应用中主要用于缓存数据(变量)，封装变量，收敛权限 

```
//判断数有无重复数据
//如果不用闭包return的话，_list需要为全局变量才能缓存数据
{
    function isFirstLoad(){
             var _list=[];
              return function(id){
                 if(_list.indexOf(id)>=0){
                     return false;
                 }else{
                    _list.push(id);
                    return true;
                 }
              }
      }
      //使用
      var firstLoad=isFirstLoad();
      firstLoad(10);
      firstLoad(10);
      firstLoad(20);
}
```

### 4.异步和单线程

#### 单线程

- js 是单线程的语言，只有一个线程，同一时间只能做一件事，两端js不能同时执行
- 原因：避免dom渲染的冲突（浏览器需要渲染dom，js可以修改dom，js执行的时候，浏览器dom渲染暂停）
- 所以需要异步 
- *webworker* 支持多线程，但不能访问dom

```
{
    console.log(100) 
    setTimeout(function(){ 
    	console.log(200) 
    }，100) 
    setTimeout(function(){ 
    	console.log(300) 
    }) //100ms后放到异步队列中
    console.log(400) //100 400 300 200
}
```

1. 执行第一行打印100
2. setTimeout 异步执行，先暂存起来
3. 打印 300
4. 待所有程序执行完，处于空闲状态时，拿到暂存的函数在指定的时间后执行

#### 异步

**1.何时需要异步**

- 在可能发生等待的情况下
- 等待过程中不能像alert一样阻塞程序运行
- 因此，所以的”**等待的情况**”都需要异步

**2.前端使用异步的场景**

- 定时任务：setTimeout，setInterval
- 网络请求：ajax请求，动态加载
- 事件绑定

```
{
	//定时任务：setTimeout，setInterva
    console.log('start');
    $.get('./data1.json',function(data1){
           console.log(data1);
    })
    console.log('end')//'start'  'end'   data1
}
{
    console.log(start);
    var img=document.createElement('img')
    img.οnlοad=function(){
            console.log('loaded')
    }//图片加载完执行
    img.src='/xx.png';
    console.log('end');//start end loaded
}
{
    console.log('start')
    document.getElementById('btn1').addEventListener('click',function(){
           alert('clicked');
    })//点击时才会执行
    console.log('end');//start end clicked
}
```

**3.异步存在的问题**

①没有按照书写方式执行，可读性差

②callback中不容易模块化

#### event-loop事件循环

- js实现异步的具体解决方案

- 同步代码直接执行

- 异步代码先放在异步队列中

- 待同步函数执行完毕，轮询执行异步队列的函数

- **js在执行宏任务前先会把微任务执行完清空,执行完一个宏任务去清空一次微任务** 

  - 宏任务:代码块，setTimeout，setInterval等,宏任务是放到事件触发队列里面的

  - 微任务:Promise，process.nextTick等,是放到微任务队列里面的

```
{
    $.ajax({
        url:'xxx',
        success:function(){
    		console.log('a')//ajax的success时间不确定
        }
    })
    setTimeout(function(){ 
    	console.log('b') 
    }，100) 
    setTimeout(function(){ 
    	console.log('c') 
    }) 
    console.log('d') 
    //dcab或者dcba
}

{
    setTimeout(function() {
    	console.log(1)//异步
    }, 0);
    new Promise(function(a, b) {
    	console.log(2);//同步
        for(var i = 0; i < 10; i++) {
        	i == 9 && a();
        }
    	console.log(3);//同步
    }).then(function() {
    	console.log(4)//异步
    });
    console.log(5)//同步
}
//23541
```

#### 补充jquery-deffered(延迟)对象 

- promise来源于jquery-deffered思想，deffered即等待
- 本质还是js异步和单线程
- 写法上杜绝回调函数
- 一种语法糖，但是解耦了代码
- 开放封闭原则

![](.\img\5.JPG)

**.done/.fail写法**

![](.\img\7.JPG)

**.then写法**

![](F:.\img\6.JPG)

##### jquery-deferred应用

```
{
/*这里面有三层函数，第一层函数是waitHandle，第二层是wait函数，第三层是task函数。这里有两个return，第一个传进dtd。然后进行一系列加工，返回了dtd*/
    function waitHandle(){
      var dtd = $.Deferred(); // 创建一个deferred对象dtd

      var wait = function(dtd){ // 要求传入一个 deferred对象dtd
        var task = function(){
          console.log('执行完成');
          dtd.resolve(); // 表示异步任务已经完成
          // dtd.reject(); // 表示异步任务失败或出错
        }
        setTimeout(task, 2000);
            return dtd; // 要求返回deferred对象那个
        }

      // 注意，这里一定要有返回值
      return wait(dtd);
    }
}
```

使用

```
var w = waitHandle();//实例化
w.then(function(){
　　console.log('success1');
},function(){
　　console.log('error');
})
.then(function(){
　　console.log('success2')
},function(){
　　console.log('error2')
});
// 还有w.done 和 w.fail
```

总结，dtd的api可分成两类，用意不同

第一类：dtd.resolve dtd.reject

第二类：dtd.then dtd.done dtd.fail

**返回 Promise 对象，调用者不能改变延迟对象** 

```
function waitHandle(){
　　var dtd = $.Deferred(); // 创建一个deferred对象

　　var wait = function(dtd){ // 要求传入一个 deferred对象
　　　　var task = function(){
　　　　　　console.log('执行完成');
　　　　　　dtd.resolve(); // 表示异步任务已经完成
　　　　　　// dtd.reject(); // 表示异步任务失败或出错
　　　　}
　　　　setTimeout(task, 2000);
　　　　return dtd.promise(); // 这里返回promise，而不是直接返回deferred对象
　　　　//防止对象w直接使用w.reject()，而只能用监听的方法then,done,fail
　　}

　　// 注意，这里一定要有返回值
　　return wait(dtd);
}


var w = waitHandle();
w.then(function(){
　　console.log('success1');
},function(){
　　console.log('error');
}).then(function(){
　　console.log('success2')
},function(){
　　console.log('error2')
});


或者直接用$.when(wait())
	   .then(                                         
            function(){},   // 回调函数1，resolve执行
            function(){}    // 回调函数2，reject执行
         );
```

#### 4.1同步和异步的区别是什么？分别举一个同步和异步的例子 

①同步会阻塞代码执行，而异步不会 

②alert是同步，有alert会阻塞当前代码，不继续向下执行，setTimeout是异步

#### 4.2一个关于setTimeout的笔试题 

```
{
    console.log(1) 

    setTimeout(function(){ 

        console.log(2) 

    },0) //

    console.log(3) 

    setTimeout(function(){ 

        console.log(4) 

    },1000) 

    console.log(5) 

    //1 3 5 2 4
   //当你定义setTimeout那一刻起（不管时间是不是0），js并不会直接去执行这段代码，而是把它扔到一个事件队列里面，当页面中所有同步任务都干完了以后，才会去执行事件队列里面的代码
}
```

#### 4.3前端使用异步的场景有哪些 

①定时任务：setTimeout，setInterval 
②网络请求：ajax请求，动态加载 
③事件绑定

### 5.日期和Math函数

#### **日期** 

```
Date.now() //获取当前时间毫秒数 

var dt=new Date() 
console.log(dt)//这个时间对象会自动加toString方法，生成一串当前时间的字符串
dt.getTime()   //获取毫秒数 
new Date().getTime()//当前时间时间戳

dt.getFullYear()   //年

dt.getMonth()  //月(0-11) ps:只有月份要加1！！！！！！！！
dt.getDate()  //日(1-31)
dt.getHours()   //小时(0-23) 

dt.getMinutes() //分钟(0-59) 

dt.getSeconds() //(0-59)

```

#### Math 

Math.random()
**random 在实际中的应用有清除缓存的作用，是每次访问到的都不是缓存**

#### 数组API

##### forEach  遍历所有元素

```
{
    var arr=[1,2,3]

     arr.forEach(function(item,index){

     //遍历数组的所有元素

     console.log(index,item)

  })

}
```

#####  every  判断所有元素是否都符合条件

```
{
   var arr=[1,2,3]

   var result=arr.every(function(item,index){

      //用来判断所有的数组元素，都满足一个条件

        if(item<4){

           return true;

        }

    })

   console.log(result);//返回布尔值，true

}
```

##### some  判断是否至少一个元素符合条件

```
{
    var arr=[1,2,3]

    var result=arr.some(function(item,index){

      //用来判断所有的数组元素，只要有一个满足条件即可

        if(item<3){

            return true;

       }

    })

    console.log(result);//返回布尔值，true 无论数组长度多少，只返回一个布尔值

}
```

##### sort  排序

```
{
   var arr=[1,4,2,3,5]
   var arr2=arr.sort(function(a,b){
         //从小到大排序
         return a-b

         //从大到小排序
         //return b-a

   })

   console.log(arr2)
}

{
	 //ps：箭头函数写法 升序
	let arr=[1,4,2,3,5];
	let arr2 = arr.sort((a,b) => {return a-b})
	console.log(arr2)
}
```

##### map  对元素重新组装，生成新数组

ps：map遇见空数组位，会跳过空位，但会保留这个值

```
 var arr=[1,2,3,4]

   var arr2=arr.map(function(item,index){

        //将元素重新组装，并返回拼接好的新数组对象

        return '<b>'+item+'</b>'

   })

   console.log(arr2)


//es6写法

var arr2=arr.map(item => {

	//将元素重新组装，并返回拼接好的新数组对象

	return '<b>'+item+'</b>'

})
```

##### filter过滤符合条件的元素

```
{
   var arr=[1,2,3]

   var arr2=arr.filter(function(item,index){

       //通过某一个条件过滤数组

       if(item>=2){

          return true;//[2,3]会返回符合条件的新的数组对象

       }

   })

   console.log(arr2)

}
```

#### 对象API

##### for....in

```
{
	  var obj={

          x:100,

          y:200,

          z:300

       };

       var key;
       //注意js中没有块级作用域，千万不要写成var key in obj 
       //不然在for循环外还可以打印出key最后一个属性，
       //或者用es6的let key in obj，for循环外还可以打印key为undefined，因为它通过babel把for循环里外编译成不完全同名的key
       for(key in obj){

          //注意这里的hasOwnProperty（指对象自身或原生的属性，而不是原型上继承的属性）

          if(obj.hasOwnProperty(key)){

                 console.log(key,obj[key])
                 // x 100
                    y 200
                    z 300

          }

}
```

#### 5.1获取2017-06-10格式的日期 

```
{
     function formatDate(dt){
          if(!dt){
          	//如果参数没有的情况
              dt = new Date()
          }
          var year = dt.getFullYear();
          var month = dt.getMonth()+1;
          var date = dt.getDate();
          if(month < 10){
             //强制类型转换
             month="0"+month;
          }
          if(date < 10){
              //强制类型转换
              date="0"+month;
          }
          return year+"-"+month+"-"+date
     }
     var dt=new Date()
     var formatDate=formatDate(dt)
     console.log(formatDate)
}
```

#### 5.2获取随机数，要求是长度一致的字符串格式 

```
{
      var random=Math.random()
      var random=random+'0000000000'   //后面加上10个零 防止为0空字符串报错
      //let random=random.padEnd(10, "0")
      var random=random.slice(0,10)
      console.log(random)
      
      
      
     var random=Math.random().toString().slice(0,10);
     //es6padEnd补齐字符串
     var random=random.padEnd(12, "0")
     console.log(random)
}
```

#### 5.3写一个能遍历对象和数组的通用forEach函数

```
{
	  //fn为forEach的回调函数
       function forEach(obj,fn){
           var key
           //准确判断是不是数据
           if(obj instanceof Array){
                  obj.forEach(function(item,inex){
                  		//fn是传进来的参数
                         fn(index,item)
                 })
           }else{
                //不是数组就是对象
                for(key in obj){
                     fn(key,obj[key])
                }
           }
       }
 
       var arr=[1,2,3];
       //注意，这里参数的顺序换了，为了和对象的遍历格式一致
       forEach(arr,function(index,item){
             console.log(index,item)
       })
 
       var obj={x:100,y:200};
       forEach(obj,function(key,value){
             console.log(key,value)
       })
}
```

## 二.JS-WEB-API

常说的JS（浏览器执行的JS）包含两部分：

-  JS基础知识（**ECMA262标准**）表面看起来并不能用于实际开发
  1. 内置函数：Object Array RegExp Function Error Date Number Boolean String…
  2. 内置对象：Math JSON…
-  JS-Web-API（**W3C标准**）W3C没有规定任何JS基础相关的东西，不管什么变量类型、原型、作用域和异步，只管定义用于浏览器JS操作页面的API和全局变量 （**window、document、navigator.uerAgent...**）
  1. Dom操作
  2. Bom操作
  3. 事件绑定
  4. Ajax请求(包括http协议)
  5. 存储

### 1.DOM

#### **DOM 本质**

Document+Object+Model(文档对象模型) /js对象

浏览器把拿到的html代码(html是xml的一种特殊结构)，结构化一个浏览器能识别并且js可操作性的一个模型而已

#### **DOM 节点操作**

```
{
    //获取DOM节点 
    var div1=document.getElementById('div1')；//元素 
    var divList=document.getElementsByTagName('div1'); //集合 
    console.log(divList.length) //集合的长度
    console.log(divList[0])

    var containerList=document.getElementsByClassName(".contaner")//集合 
    var pList=document.querySelectorAll('p')//集合 
    
    //property 
    var pList=document.querySelectorAll('all'); 
    var p=pList[0]; 
    console.log(p.style.width) 
    p.style.width='100px'
    console.log(p.className) 
    p.className='p1'
    
    //attibute
    ele1.getAttribute('style');
    ele1.getAttribute('data-name')
    ele1.setAttribute('style', 'font-size:12px') 
}
```

```
{
    var div1 = document.getElementById('div1')
    // 添加新节点
    var p1 = document.createElement('p')
    p1.innerHTML = 'this is new p'
    div1.appendChild(p1)    // 添加新创建的元素
    // 移动已有节点(原节点被删除)
    var p2 = document.getElementById('p2')
    div1.appendChild(p2)
}
{
    var div2 = document.getElementById('div2')
    var parent = div2.parentElement //获取父元素
    var child = div2.childNodes     // 集合(获取子节点nodeList) 
    /// 加入子元素中含有空格/回车之类的字符，得到的集合会多若干个text的子元素
    // 解决办法：使用nodeType判断子节点的类型
    console.log(child[0].nodeType)  // text或其他元素类型
}
{
    div2.removeChild(child[1])      // 删除第一个子元素
}
```

#### **DOM事件** 

##### DOM事件级别

```
DOM0    element.onclick=function(){};
DOM1标准指定的时候没有涉及任何和事件相关的东西；
//-->true,false指定冒泡还是捕获
//IE attachEvent
DOM2    element.addEventListener('click',function(){},false);
//事件类型增加了很多；
DOM3    element.addEventListener('keyup',function(){},false);
```

##### DOM事件模型

- 捕获（从上往下）
- 冒泡（从目标元素往上）

##### **DOM事件流（页面中接受事件的顺序）** 

- 捕获阶段
- 目标阶段     事件通过捕获到达目标元素
- 冒泡阶段     从目标元素上传到window对象   

##### **Event对象的常见应用** 

```
event.preventDefault()：阻止默认事件
eg:a标签绑定了click事件，在响应函数中设置了preventDefault，就阻止了链接默认跳转的行为

event.stopPropagation()：阻止事件冒泡

event.stopImmediatePropagation()：阻止事件执行(事件响应优先级)
一个按钮绑定了两个click事件1和2，第一个响应函数是a，
第二个响应函数是b，依次注册了a、b两个事件，想通过优先级的方式让执行a之后完不在执行b了；
a响应函数中加上event.stopImmediatePropagation();就会成功的阻止b执行；

event.currentTarget：当前所绑定的事件对象
把子元素的事件代理都转移到父元素上，只绑定一次事件就可以了。做响应的时候就要区分是哪个元素被触发
event.target：当前被点击的元素
```

##### **自定义事件/模拟事件** 

```
1.Event('事件名')
<div id="ev">目标元素</div>
<script>
    var ev=document.getElementById('ev');
    var event=new Event('custome');
    ev.addEventListener('custome',function () {
        console.log('custome');
    });
    ev.dispatchEvent(event);
</script>


2.CustomEvent('事件名',obj)：可以增加一个obj参数对象
  detail：提供有关事件的自定义信息的子对象。
<div id="ev">目标元素</div>
<script>
    var ev=document.getElementById('ev');
    var eve=new CustomEvent('test',{detail:{a:1, b:2}});
    ev.addEventListener('test',function (e) {
        console.log(e.detail.a)
    });
    ev.dispatchEvent(eve);
</script>
```


#### 1.1dom是哪种基本的数据结构 

树 

#### 1.2Dom操作的常用API有哪些？ 

```
1、获取DOM节点/属性：.getElementById(还有class和tag)、.querySelectorAll、.getAttribute

2、获取父元素/子节点：.parentElements、.childNodes

3、新增/删除节点：.appendChild()、.removeChild()

PS:nodeType代表dom节点类型

1---元素节点  2---属性节点 3---text....

```

#### 1.3Dom节点的Attribute和Property有和区别？

①property只是一个JS对象的属性的修改  width/className
②Attribute是对html标签属性的修改 src/img....

#### 1.4**描述DOM事件捕获的具体流程**

```
第一个接受事件的对象是window;
取得body标签：document.body;
表示html节点：document.documentElement;
    
捕获流程： window-->document-->html标签-->body-->父级元素--子级元素...-->目标元素
冒泡流程：从目标元素一层一层最后到window完成了一次冒泡的流程 ； 
```

### 2.BOM

Brower+Object+Model 浏览器对象模型

- screen  测试浏览器型号 
- location
- history  历史 前进/后退
- navigator

```
{
	var ua=navigator.userAgent  //浏览器型号 一串字符串
    var isChrome=ua.indexOf('Chrome') 
    console.log(ua,isChrome) 
 
    console.log(screen.width) 
    console.log(scr)

    console.log(location.href) //整个url
    console.log(location.protocal) //协议 http/https
    console.log(location.pathname) //路径
    console.log(location.search) //?后面的一串字符串
    console.log(location.hash)//#后面的一串字符串
	//历史纪录
    history.back() 
    histort.forward()
}
```

### 3.事件

#### 通用事件绑定

```
{
    var btn=document.getElementById('btn1'); 

    btn.addEventListener('click',function(event){ 

    	console.log('clicked') 

    })

    function bindEvent(elem,type,fn){ 

   	 	elem.addEventListener(type,fn) 

    }

    var a=document.getElementById('link') 

    bindEvent(a,'click',function(e){ 

    	e.preventDefault() //阻止默认行为 

   		alert('clicked') 

    })

}
```

注：关于IE低版本的兼容性 
①IE低版本使用attachEvent绑定事件，和W3C标准不一样 
②IE低版本使用量以非常少，很多网站都早已不支持 
建议对IE低版本的兼容性：了解即可，无需深究 
如果遇到对IE低版本要求苛刻的面试，果断放弃

#### 事件冒泡

```
{
    <div id="div1">
      <p id="p1">激活</p>
      <p id="p2">取消</p>
      <p id="p3">取消</p>
      <p id="p4">取消</p>
    </div>
    <div id="div2">
      <p id="p5">取消</p>
      <p id="p6">取消</p>
    </div>
 
 
    <script type="text/javascript">
    // 利用阻止冒泡的机制实现：只点击 p1 的时候弹窗激活
      function bindEvent(elem,type,func) {
        elem.addEventListener(type,func)
      }
      var p1 = document.getElementById('p1')
      bindEvent(p1,'click',function(e){
      //如果不阻止 p冒到div，div冒到event
        e.stopPropagation() //阻止冒泡
        alert('激活')
      })
      bindEvent(body,'click',function (e) {
        alert('取消')
      })
    </script>
}
```

#### 事件代理

原理：根据事件冒泡

应用于一些新增的节点

```
{
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
      <p>fjdk</p>
      <h1>jfkd</h1>
      <!-- ...随时会新增更多的 a 标签 -->
    </div>
    <script type="text/javascript">
      // 要求用代理的方式实现 动态事件绑定，绑定到 div1上
      var div = document.getElementById('div1')
      function bindEvent(elem,type,func) {
        elem.addEventListener(type,func)
      }
      bindEvent(div,'click',function(e){
        console.log(e) // MouseEvent
        console.log(e.target) //  完整的 a 标签 对象 <a href="#">a3</a>
        console.log(e.target.nodeName); // 都是大写
        if(e.target.nodeName === 'A'){
          alert(e.target.innerHTML)
        }
  })
    </script>
}
```
ps:比较js和jq的事件委托/代理
```
//jq的写法
//jQueryObject.on( events [, selector ] [, data ], handler )
$('#wrap').on('click', 'a.btn', function (e) {
    console.log(1111);
})

//js 写法
document.querySelector('#wrap').addEventListener("click", function (e) {
    if (e.target.matches('a.btn')) {
        console.log(111);
    }
});
```

####3.1编写一个通用的事件监听函数 bind Event
先判断使用不使用代理，如果要使用再判断有无代理(如果selector有值则elem为父节点)
```
{
   //selector第二个参数
    function bindEvent(elem,type,selector,fn){
        if(fn == null){
        	//如果不使用代理 调换 第三个参数被fn代替，并且selector为null
            fn = selector
            selector = null
        }
        elem.addEventListener(type, function(e){
            var target
            if(selector){
            //selector有值为代理
                target = e.target
                if(target.matches(selector)){
                    //target是否符合传入目标的参数 第二个参数符合就执行
                    fn.call(target,e)
                }
            }else{
                fn(e)
            }
        })
    }
    //使用代理
    var div1 = document.getElementById('div1')
    bindEvent(div1,'click','a',function(e){
    	//e.target---a，e.currentTarget---div
        console.log(e.target,e.currentTarget)
        //false true
        console.log(e.currentTarget===this,e.target===this)
    	//a标签阻止默认行为
        e.preventDefault();
        console.log(this.innerHTNL) 
     })
     //不使用代理
     var a = document.getElementById('a1');
     bindEvent(a, 'click', function(e){
        console.log(a.innerHTNL) 
     })
}
```

####3.2描述事件冒泡流程 
①DOM树形结构 
②事件冒泡 
③阻止冒泡 
④冒泡的应用(代理) 

#### 3.3e.target和*e.currentTarget*区别

e.target是点击事件触发的对象（也就是点击的是谁）

e.currentTarget是事件绑定在哪个元素上（也就是这个事件在哪个组件上）。

currentTarget指的是事件触发后，冒泡到绑定处理程序的元素，就是绑定事件处理程序的元素，target指的是触发事件的元素 

####3.4对于一个无线下拉加载图片的页面，如何给每个图片绑定事件 
①使用代理 
②知道代理的两个优点 
代码简洁 
减少浏览器内存占用

###4.Ajax
####核心API---XMLHttpRequest
```
	var xhr = new XMLHttpRequest()
	xhr.onreadystatechange = function () {
        if(xhr.readyState === 4){
            if(xhr.satus ==== 200) {
                
            }
        }
	}
	xmlHttp.open('GET', url);
    //发送
    xmlHttp.send();
```
##### 注：IE兼容性问题

①IE低版本使用ActiveXObject( new ActiveXObject() )，和W3C标准( new XMLHttpRequest() )不一样
②IE低版本使用量非常少，很多网站都早已不支持
③建议对IE低版本的兼容性了解即可，无需深究
④如果遇到对IE低版本要求苛刻的面试，果断放弃

####状态码说明（ajax）

readyState

0-（未初始化）还没有调用send()方法 
1-（载入）已调用send()方法，正在发送请求 
2-（载入完成）send()方法执行完成，已经接收得到全部响应内容 
3-  (交互)正在解析响应内容 
4-（完成）响应内容解析完成，可以在客户端调用了

status

2xx-表示成功处理请求。如200 
3xx-需要重定向，浏览器直接跳转 
4xx-客户端请求错误，如404 
5xx-服务器端错误，如500

####跨域

- 什么是跨域

  浏览器有同源策略（域名、端口、协议任一不同就算跨域），不允许ajax访问其他域接口

  http协议默认80，https默认端口443

  所有跨域请求都要经过信息提供方允许 

- 跨域限制 

  **Cookie、LocalStorage 和 IndexDB 无法读取**
   --操作不了Cookie、LocalStorage、IndexDB

   **DOM无法获得**
   --无法获取和操作另一个资源的DOM

   **Ajax请求不能发送（同源下的通信方式）**
   --Ajax只适合同源的通信（跨域就不行了）

- 可以跨域的三个标签

  <img src=xxx>

  <link href=xxx>

  <script src=xxx>

- 三个标签场景 

  <img>用于打点统计，统计网站可能是其它域，且img没有兼容性问题
  <link><script>可以使用CDN
      CDN的全称是Content Delivery Network，即内容分发网络。
      CND加速主要是加速静态资源，如网站上面上传的图片、媒体，以及引入的一些Js、css等文件。
      CND加速需要依靠各个网络节点，会从最近的节点返回资源，这是核心。
      CND服务器通过缓存或者主动抓取主服务器的内容来实现资源储备。
  
  <script>可以用于JSONP

#### 4.1手动编写一个ajax，不依赖于第三方库

```
{
    // 指定了请求目标，也明确了如何处理之后，就可以发送请求了
    var request = new XMLHttpRequest();
   
    request.onload = function(){

        if(request.readyState === 4){
            // 请求完成
            if(request.status === 200 || request.status === 304){
                // 请求成功，获得一个成功的响应,此后可以开始请求成功后的处理
                request.responseText//responseText 保存文本字符串格式
                request.responseXML//responseXML 保存 Content-Type 头部中指定为 "text/html" 的数据
            }else{
                // 请求失败，根据响应码判断失败原因
                console.log('error,status:'+request.status)
            }
        }else{
            // 请求还在继续
        }
        request.open('GET',url,true);// 指定请求目标，三个参数，1.GET or POST 2.请求路径 3.是否异步 （默认true，可以不写）
        request.send();//发送请求
    }
}
```

####4.2跨域的几种实现方式

- nginx反向代理

- JSONP

  在出现postMessage、CORS之前一直用JSONP做跨域通信的

  **限制：JSONP只支持`GET`请求** 

  **原理：利用script标签的异步加载来实现的**

  ```
  {
  //网站A要跨域访问网站B的一个接口，B给A一个接口，约定返回内容格式如callback({x:100,y:200})(可动态生成),然后在前台定义好这个callback函数
  <script>
  	//定义
      window.jsonp = function(data){
          console.log(data);
      }
  </script>
  <script src="http://xxxxxxxx(B)?callback=jsonp"></script>//返回的是callback函数，之前已经定义了，这里相当于直接就执行，返回({x:100,y:200})，然后就输出了
  }
  ```

  ```
  {
      function createJs(sUrl){
          var oScript = document.createElement('script');
          oScript.type = "text/javascript";
          oScript.src = sUrl;
          document.getElementsByTagName('head')[0].appendChild(oScript);
      }
          
      createJs('jsonp.js?callback=box');
  }
  ```

- Hash

  **原理：Hash的变动页面不会刷新**
  url中?后面的叫search：search的改变是会刷新页面的；所以search不能做跨域通信

  利用hash，场景是当前页面 A 通过iframe或frame嵌入了跨域的页面 B，跨域给B发消息

  ```
  /*先拿到B这个窗口的地址src，然后通过hash的方式后面发一段字符串，这个字符串可以是通过json数据最后通过json.stringify()把json转成字符串发给B，你是发出去了，B能不能接收到，B在自己的代码中增加一个window.onhashchange,这个事件是用来监听你当前页面的hash有没有改变。之前的页面是src，现在的页面加一个hash，所以对B来说，你的url的hash变化了，就可以拿到了。拿到了以后通过window.location.hash就能拿到hash的具体内容。window.location.hash可不是拿到一个，如果hash后面等于data除了A发送过来的东西还有别的拼接，回来要特殊处理一下*/
  
  // 在A中伪代码如下
  var B = document.getElementsByTagName('iframe');
  B.src = B.src + '#' + 'data';
  
  // 在B中的伪代码如下
  window.onhashchange = function () {
  	var data = window.location.hash;
  };
  ```

- postMessage 

   html5中出现了这个标准postMessage，用这个实现跨域通信

  ```
   // 窗口A(http:A.com)向跨域的窗口B(http:B.com)发送信息
   targetWindow.postMessage('data', 'http://B.com');
   // 在窗口B中监听message事件
   Awindow.addEventListener('message', function (event) {
       console.log(event.origin);//源
       console.log(event.source);
       console.log(event.data);
   }, false);//默认值为 false, 即冒泡传递
  ```

  ```
  <!DOCTYPE HTML>
  <html>
  <head>
      <meta charset="utf-8">
      <title>42度空间-window.postMessage()跨域消息传递</title>
  </head>
  <body>
  <div>
      <input id="text" type="text" value="42度空间" />
      <button id="send" >发送消息</button>
  </div>
  <iframe id="receiver" src="http://res.42du.cn/static/html/receiver.html" width="500" height="60">
      <p>你的浏览器不支持IFrame。</p>
  </iframe>
  <script>
  	//发送程序
      window.onload = function() {
          var receiver = document.getElementById('receiver').contentWindow;
          var btn = document.getElementById('send');
          btn.addEventListener('click', function (e) {
              e.preventDefault();
              var val = document.getElementById('text').value;
              receiver.postMessage("Hello "+val+"！", "http://res.42du.cn");
          });
      }
  </script>
  </body>
  </html>
  
  //在另一个页面接收程序
  window.addEventListener('message', function (event) {
       console.log(event.origin);//源
       console.log(event.source);
       console.log(event.data);
   }, false);
  
  ```

- WebSocket 

  ```
  //1.声明一个webSocket对象，这个地方有两种，ws、wss区别一个加密一个非加密，
  后面指向服务器的一个地址，这样就建立了相当于JS一个对象来管理这个链接。
  var ws = new WebSocket('wss://echo.websocket.org');
  
  //2.请求发送出去
  ws.onopen = function (evt) {
  	console.log('Connection open ...');
  	ws.send('Hello WebSockets!');
  };
  
  //3.对方给消息怎么接收，通过这个参数的data来拿到
  ws.onmessage = function (evt) {
  	console.log('Received Message: ', evt.data);
  	ws.close();
  };
  
  //4.最后这个链接不用了，中断了，监听onclose来确定是不是关闭了
  ws.onclose = function (evt) {
  	console.log('Connection closed.');
  };
  ```

- CORS

  Ajax一个变种，fetch实现CORS通信的--新出的通信标准，可以理解为支持跨域通信的Ajax

  **原理: 浏览器会拦截ajax请求，如果它觉得这个ajax请求是跨域的，它会在http请求中，加一个origin**

##### 补充CORS

CORS是一个W3C标准，全称是跨域资源共享(Cross-origin resource sharing)。它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制 

CORS请求分成两类:简单请求（simple request）和非简单请求（not-so-simple request）  

```
1.简单请求
①请求方法是以下三种方法之一：
HEAD
GET
POST
②HTTP的头信息不超出以下几种字段：
Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

2.非简单请求 
非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT、DELETE或OPTIONS，或者Content-Type字段的类型是application/json
```

**简单请求**

浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。 

**非简单请求** 
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，**称为”预检”请求（preflight），预捡的请求方法是OPTIONS**。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错 

因此我们可以在浏览器的开发者工具中查看头信息，若头信息中有OPTIONS方法，说明此次CORS请求是非简单请求

除了`Origin`字段，"预检"请求的头信息包括两个特殊字段。

**（1）Access-Control-Request-Method**

该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，PUT/DELETE

**（2）Access-Control-Request-Headers**

该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，比如`X-Custom-Header`。

一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个`Origin`头信息字段。服务器的回应，也都会有一个`Access-Control-Allow-Origin`头信息字段 

<http://www.ruanyifeng.com/blog/2016/04/cors.html> 

### 5.存储

- cookie

  - 本身用于客户端和服务端通信

  - 有本地存储功能

  - 使用document.cookie = ..获取和修改
  - 存储量小，只有4kb(需要携带到http中不能太大)
  - 所有http请求都带着，会影响获取资源的效率
  - API简单但是需要封装才好用

- localStorage

- sessionStorage

  - html5特性，最大容量5m

  - API简单：

  		localStorage.setItem(key,value);

  		localStorage.getItem(key);

  ```
  loacationStorage和sessionStorage的区别：
  浏览器关了，sessionstorage是会话层，会清理存储，locationStorage为永久存储不会清理，一般用locationStorage；
  ps：在IOS safari隐藏模式下，localStorage.getItem会报错，最好用try-catch封装
  ```

### 开发环境

#### IDE

- webstorm
- sublime
- vscode
- atom （小清新）
- 插件

#### Git 版本管理

- 正式项目都需要版本管理，可以清晰的看到历史版本
- 多人协作开发
- Git 和 Linux 是一个作者
- Git 服务器
  - github
  - coding.net（国内）
  - 码云

<https://blog.csdn.net/qq_41115965/article/details/81559765> 

origin是你的远程库，master是该远程库的主分支

```
	{
    git add <文件名>//将文件加入暂存区。
  	git add .  //将所有文件加入暂存区。
 	git checkout <文件名>//撤销工作区某文件的修改，使之与暂存区一致。/切换
	git checkout .  //撤销工作区所有文件的修改，使之与暂存区一致。
	git commit -m "提交信息"//将暂存区的内容保存到本地仓库，并填写提交信息。
    git status 用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。
 	git push origin master //提交代码至远程代码库主分支
	git pull origin master //从远程代码拉取代码 同步本地
	
	git clone sshxxx//克隆项目到本地
	git init //自己创建
	
	//多人开发 
	git branch //查看分支
    git checkout -b xxx新创建一个分支
    git chckout xxx 切换到某个分支           
	
	vi a.js //创建一个新文件
	git diff //查看不同
	git commit -m "提交信息"//提交信息
	git push origin xxx分支 //提交到远程xxx分支
	
	git checkout master // 切换到master分支
	git pull origin master//先拉取一遍别人可能更新了
 	git merge origin/xxx分支 // 把xxx分支上新增的内容合并到master分支上
	git push // 把master分支的内容提交到线上 一定还要再提交一遍！！！！！！！！！！
	
	git reset --soft HEAD^ //撤销本次提交，将本地仓库回滚到上一个版本，工作区和暂存区不变。
	HEAD^表示上个提交版本，HEAD^^表示上上个提交版本，HEAD~n代表前面第n个版本
	
}
```

#### 模块化 

- 使用模块化原因 

  如果不使用模块化，用多个js文件引用的方法，可能会造成全局变量污染（覆盖），并且依赖关系复杂也可能导致错误（你只知道你要依赖的js文件，却不知道你依赖的js文件里还有哪些依赖的js文件）。

- AMD模块规范

  异步模块定义 ，被define过的才能被require，define和require内的数组可以有多个元素，define和require内function的参数是他所引用的
  对象的返回。

  主模块，require.js在加载的时候会检查data-main属性，当加载完毕，data-main属性规定的js文件会第一个被require.js加载并执行。由于require.js默认的文件后缀名是js，所以可以把main.js简写成main 

  *data-main*:为HTML5新增的规范,用来嵌入自定义数据 

  ```
  入口文件main.js   <script src="js/require.js" data-main="js/main"></script>
  ```

  按顺序依赖模块自动异步加载js

![](.\img\2.JPG)

![](.\img\3.JPG)

- commonJS模块规范

  是nodeJs的模块规范，不是异步加载js，而是同步一次性加载

  ![](.\img\4.JPG)

#### 上线回滚

将当前服务器的代码打包并记录版本号，备份

将备份的上一个版本号解压，覆盖到线上服务器，并生成新的版本号

##### linux 基础命令

```
1. mkdir a      // 在当前目录中创建一个空文件夹'a'
2. ls           // 查看当前目录下的文件
3. ll           // ls -l 的简写 显示当前目录下每个文件或者子目录的详细信息
4. cd a         // 进入当前目录下的 a 目录
5. pwd          // 查看当前目录的路径
6. cd ..        // 返回上层目录
7. rm -rf a     // 删除文件夹 a
8. vi a.js      // 或者 vim ；编辑 a.js 文件，写入新的内容并保存则会创建；键入 i 进入插入模式，ESC 返回刚刚的模式 :w 保存 :q 退出 :wq 保存并退出
9. cp a.js a1.js  // 拷贝 a.js 存入 a1.js 
10. mv a1.js <new dir>  // 将 a1.js 移动到新的文件夹下
11. rm a.js         // 删除 a.js
12. cat a.js    // 查看 a.js 
13. head a.js   // 查看头部内容
14. tail a.js   // 查看尾部内容
15. head -n 1 a.js   // 查看第一行
16. tail -n 2 a.js   // 查看后两行
16. grep '2' a.js    // 搜索 包含 '2'
```

### 性能优化

#### 加载一个资源的过程 /从输入url到得到html的详细过程

- 浏览器根据DNS服务器得到域名的IP地址（dns解析）
- 向这个IP的机器发送http请求（http/https）
- 服务器收到、处理并返回http请求
- 浏览器得到返回内容

#### 浏览器渲染页面的过程 

- 根据HTML结构生成DOM Tree（dom树）

- 根据CSS生成CSSOM（将css结构化处理）

  - 为什么把css放在head？

  - 会先渲染一遍默认的， 再遇到css的 会再次渲染一遍 性能会差

- 将DOM和CSSOM结合成RenderTree（渲染树）

- 根据RenderTree渲染和展示

- 遇到`<script>`时，会执行并阻塞渲染，所以`<script>`放在`<body>`即将结束的位置前    

  - 因为js有权利改变dom结构，如果同时进行会发生冲突

#### window.onload和document.DOMContentLoaded区别

window.onload页面的全部资源加载完成才会执行 

document.DOMContentLoaded 、document.ready方法在DOM 渲染完成即可执行，此时图片，视频还可能没有加载完 

document.ready缩写：$(function(){});

#### 优化总结

##### 关键

- **资源压缩合并，减少HTTP请求（资源传输的过程变小）** 
- **非核心的代码异步加载—异步加载的方式—异步加载的区别** 
- **利用浏览器缓存—缓存的分类—缓存的原理** 
- **使用 CDN**
- **预解析DNS**

##### 页面资源如何加载更快 

- 静态资源的压缩合并（多个js文件合成一个js文件，减少请求）压缩代码减少体积
- 使用静态资源缓存
  - 通过链接名称控制缓存
- 使用 CDN 让资源加载更快
- 使用 SSR 后端渲染，让数据直接输出到 HTML 中
  - jsp原理上也属于后端渲染

##### 对于页面的渲染以及动态操作如何更快

- CSS 放前面，JS 放后面
- 懒加载（图片懒加载，下载加载）
- 减少 DOM 查询，对 DOM 查询做缓存
- 减少 DOM 操作，多个操作尽量合并在一起执行
- 事件节流（多次频繁触发事件时，减少cpu计算）

```
 
// 1. 懒加载,页面首次渲染先加载一个预览图，再用用DOM操作改变其真实需要加载的图片
 
<img id = 'img1' src="preview.png" data-relsrc="abc.png"/>
<script>
    var img1 = document.getElementById('img1')
    img1.src = img1.getAttribute('data-realsrc')
</script>
 
 
// 2. 缓存 DOM 查询
<script>    
var i
for(i = 0;i<document.getElementsByTagName('p').length;i++){
    // 每次循环都得执行一次 DOM 查询
}
 
// 缓存 DOM 查询是这样的（只做了1次dom查询）
var pList = document.getElementById('p')
var i
for(i = 0;i<pList.length;i++){
    //TODO
}
 
// 3. 合并 DOM 插入
var listNode = document.getElementById('list')
// 要插入 10 个 li 标签
var frag = document.createDocumentFragment()
var i,li
for(i = 0; i < 10; i++){
    li = document.createElement('li')
    li.innerHTML = "list item " + i
    frag.appendChild(li)
} 
listNode.appendChild(frag)// 插入不是本身而是所有子孙节点
 
// 4. 事件节流
var textarea = document.getElementById('text')
var timeoutId
textarea.addEventListener('keyup',function{
	//如果有时间函数
    if(timeoutId){
        clearTimeout(timeoutId)
    }
    timeoutId = setTimeout(function(){
        // 触发 change 事件
    },100)
</script>
 
```

####  预解析DNS

浏览器输入一个url到页面真正渲染完中间发第一步就是DNS解析 

 **怎么使用预解析DNS **

 <link rel=“dns-prefetch” href=“//host_name_to_prefetch.com”> 

**强制打开a标签的DNS预解析**

 <meta http-equiv=“x-dns-prefetch-control” content=“on”>
 页面中所有的a标签，在一些高级浏览器中默认打开了DNS预解析的；(不加这句话，a标签也是可以做到DNS预解析的，这是浏览器的一个功能；)但是如果你的页面是https协议头的，很多浏览器是默认关闭DNS预解析的。这句话是强制打开a标签的DNS预解析。

#### **异步加载** 

**异步加载的方式**
1.动态脚本加载
 js创建了一个标签，把标签最后加到body上去或者head（加到文档当中实现加载）

```
var oScript = document.createElement('script');
oScript.type = "text/javascript";
oScript.src = sUrl;
document.getElementsByTagName('head')[0].appendChild(oScript);
```

2.defer

```
head：
<script src=“./defer1.js” charset=“utf-8” defer></script>
//内容console.log(‘defer1’);
<script src=“./defer2.js” charset=“utf-8” defer></script>
//内容console.log(‘defer2’);
body:
<div class=“div”>
<script type=“text/javascript”>
	document.write('<span>write</span>');
</script>
<script type=“text/javascript”>
for(var i=0; i<2000000; i++){
    if(i%2000000===0){
    	console.log(i);
    }
}
</script>
</div>
执行顺序：先执行write，再执行for循环，最后执行异步加载js，defer1-->defer2
（defer1,defer2异步加载是等到同步加载之后执行的）
```

3.async

```
async1.js console.log(‘async1’);
async2.js console.log(‘async2’);
head：
<script src=“./async1.js” charset=“utf-8” defer></script>
内容console.log(‘async1’);
<script src=“./async2.js” charset=“utf-8” defer></script>
内容console.log(‘async2’);
body:
//内联到DOM中的是做同步执行的
<div class=“div”>
test
<script type=“text/javascript”>
	document.write(‘<span>write</span>’);
</script>
<script type=“text/javascript”>
for(var i=0; i<2000000; i++){
    if(i%2000000===0){
    	console.log(i);
    }
}
</script>
</div>
先执行write，再执行for循环，最后执行异步加载js(因为是本地文件，加载速度比较快，所以执行顺序是按顺序的)
如果把async1.js换成大的js文件，async2.js文件小，加载快，就有可能会在前面执行
```

**异步加载的区别**

1.defer是在HTML解析完之后才会执行，如果是多个，按照加载顺序依次执行； 

2.async是在加载完之后立即执行，如果是多个，执行顺序和加载顺序无关 ，谁快就执行谁

#### 浏览器缓存 

**缓存：**对应文件在浏览器中存在的备份（副本），把请求的东西缓存到本地了（是放在电脑磁盘中的），浏览器下次请求相当于是从这个磁盘直接读了，而不会再去请求这个文件的地址 

**缓存的分类** 

**1.强缓存** 

不管是绝对时间还是相对时间，这个时间过期之前不会和服务器通信了，直接从浏览器副本拿出来给页面用

```
http协议头：
Expires       Expires:Thu,21 Jan 2017 23:39:02 GMT//服务器的绝对时间
Cache-Control      Cahe-Control : Max-age=3600 //浏览器的相对时间 单位s 以相对时间为准
```

**2.协商缓存**

和服务器协商这个文件能不能用，过期没有

```
Last-Modified If-Modified-Since   Last-Modified:Wed,26 Jan 2017 00:35:11 GMT
Etag   If-None-Match  
/*
Last-Modified 请求资源的最后修改时间 (服务器下发的上次修改的时间)
If-Modified-Since浏览器端缓存页面的最后修改时间一起发到服务器去，服务器会把这个时间与服务器上实际文件的最后修改时间进行比较 与Last-Modified同一个value值
如果时间一致，那么返回HTTP状态码304（不返回文件内容），客户端接到之后，就直接把本地缓存文件显示到浏览器中。
如果时间不一致，就返回HTTP状态码200和新的文件内容，客户端接到之后，会丢弃旧文件，把新文件缓存起来，并显示到浏览器
Etag虽然hash值变了，内容并没有变化，完全可以从副本拿，Etag就是解决这个问题的
当你第一次发起HTTP请求时，服务器会返回一个Etag
并在你第二次发起同一个请求时，客户端会同时发送一个If-None-Match，而它的值就是Etag的值
*/
```

**3.和缓存相关的http头** 

```
Expires
Cache-Control
Last-Modified
Etag
If-None-Match
```

### 错误监控

如何保证产品质量 ----做好错误监控

#### 前端错误的分类及捕获方式

**1.即时运行错误：代码错误** 

```
* try…catch(需要把try...catch布到代码中)
* window.onerror(dom0)只能捕获即时运行错误
```

**2.资源加载错误 (不会冒泡)**

```
* object.onerror   //通过节点绑onerror事件，捕获加载错误
    <img src="/images/image.gif" onerror="myFunction()">
    <script>
        function myFunction() {
            alert('无法加载图像。');
        }
    </script>
* performance.getEntries();//间接获取加载失败资源
    //获取已加载资源时长，通过这个方式可以间接的拿到没有加载资源的错误；
    //返回的是一个数组，有forEach方法。  可以得到已成功加载的资源。 
    performance.getEntries().forEach(item=>{
        console.log(item.name)
    });
    document.getElementsByTagName('img');//能拿到所有img的一个集合
    //所需要加载的所有图片的一个集合减去上面已成功加载的集合，剩下就是没有成功加载的。
* Error事件捕获 //window上通过事件捕获一样可以拦截到资源加载错误。可以在捕获阶段拿到这个
    //不会冒泡，script标签发生了错误，触发本身onerror事件已经可以了，不会向上冒泡到window
    <script src=“//baidu.com/test.js” charset="utf-8"></script>//不存在
    <script>
        window.addEventListener('error',function(e){
            conosle.log('捕获',e); 
            /*捕获Event{isTrusted:true,'type':'error',target:script,currentTarget:window,eventphase:1…}*/
        },true);//true才是捕获
    </script>
   // GETxxx ERROR    
```

#### **延伸：跨域JS运行错误可以捕获吗，错误提示是什么，应该怎么处理？** 

因为跨域没有权限，只能捕获到这个错误，但是没拿到相应的具体信息
![](.\img\9.JPG)

 **需要这样处理之后就可以拿到详细信息：**
 1.在script标签增加crossorigin属性（在客户端做的）

 2.设置JS资源响应头Access-control-Allow-origin；*（在服务端做的）
 在响应JS资源的http头上加一个Access-control-Allow-origin，也可以是指定域名

#### 上报错误的基本原理

1.**采用Ajax通信的方式上报** （所有的错误监控都不是通过这种方式来做的；） 

2.**利用Image对象上报** （所有的监控体系都是这样做的） 

```
<script>
    //利用这种方式发送一个请求非常简单，比Ajax简单，不需要借助任何第三方库；
    (new Image()).src="http://baidu.com/tesjk?r=tksjk";
    //一行代码实现一个资源向上报；/tesjk?这个是上报路径；r=tksjk加信息
</script>
```

### 补充面试题/知识点

#### delete、vue.delete区别

delte会删除数组的值，但是它依然会在内存中占位置 而vue.delete会删除数组在内存中的占位 

一段代码

```
let a = {a:10};
let b = {b:10};
let obj = {
	a:10
};
obj[b]=20;
console.log(obj,obj[a])//{a: 10, [object Object]: 20} 20
```

#### 小test:es6求阶乘箭头函数递归

```
{

	let one = n => {

		return n<1 ? 1 : n * one(n-1)

	}

	console.log(one(3))

}

```

#### 冒泡排序

```
function bSort(arr) {
  var len = arr.length;
  for (var i = 0; i < len-1; i++) {
    for (var j = 0; j < len - 1 - i; j++) {
         // 相邻元素两两对比，元素交换，大的元素交换到后面
        if (arr[j] > arr[j + 1]) {
            var temp = arr[j];
            arr[j] = arr[j+1];
            arr[j+1] = temp;
        }
    }
  }
  return arr;
}
```

```
{
    //es6写法使用解构赋值
    var arr = [8, 2, 4, 1, 0, 6, 5];
    var bubbleSort = () => {
        for (i = 1; i < arr.length; i++) {
            for (j = 0; j < arr.length - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                }
            }
        }
    	return arr;
    }
    bubbleSort(arr);
    console.log(arr);
}
```

也可用s.sort(function(a,b){return a-b;});  ---return a-b即为升序

#### 页面布局

**假设高度已知，请写出三栏布局，其中左栏、右栏宽度各为300px，中间自适应** 

补充：height:100vh == height:100%;
但是当元素没有内容时候，设置height:100%，该元素不会被撑开，此时高度为0，
但是设置height:100vh，该元素会被撑开屏幕高度一致

```
方法一：display:flex
#container{
    display: flex;
    height: 100vh;
}
.left{
    width: 300px;
    background: red;
}
.content{
    flex: 1;
    background: darkcyan;
}
.right{
    width: 300px;
    background: red;
}
<article id="container">
    <div class="left"></div>
    <div class="content"></div>
    <div class="right"></div>
</article>
方法二：浮动
#container div{
	min-height: 100px;
}
.left{
    float: left;
    width: 300px;
    background: red;
}
.content{
    background: darkcyan;
}
.right{
    float: right;
    width: 300px;
    background: red;
}
<article id="container">
    <div class="left"></div>
    <div class="right"></div>
    //浮动法 content放最后
    <div class="content"></div>
</article>
方法三：定位
*{
    margin: 0;
    padding: 0;
}
.left{
    position: absolute;
    top: 0;
    left: 0;
    width: 300px;
    height: 100vh;
    background: red;
}
.content{
    margin: 0 300px;
    height: 100vh;
    background: darkcyan;
}
.right{
    position: absolute;
    top: 0;
    right: 0;
    width: 300px;
    height: 100vh;
    background: red;
}
<article id="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="content"></div>
</article>
方法四：表格布局
*{
    margin: 0;
    padding: 0;
}
#contenter{
    display: table;
    width: 100%;
    min-height: 100px;
}
.left,.center,.right{
    display: table-cell;
}
.left{
    width: 300px;
    background: red;
}
.right{
    width: 300px;
    background: cornflowerblue;
}
<article id="contenter">
    <div class="left"></div>
    <div class="center">表格布局</div>
    <div class="right"></div>
</article>
方法五：网格布局
*{
    margin: 0;
    padding: 0;
}
#contenter{
    display: grid;
    width: 100%;
    grid-template-rows: 100px;//其实就相当于行的高度
    grid-template-columns: 300px auto 300px;
}
.left{
    background: red;
}
.center{
    background: rebeccapurple;
}
.right{
    background: cornflowerblue;
}
<article id="contenter">
    <div class="left"></div>
    <div class="center">表格布局</div>
    <div class="right"></div>
</article>
```

- 每个解决方案优缺点

> float，比较好的兼容性需要清除浮动，处理好与周边元素的关系
>
> absolute，方便快捷，布局脱离文档流，导致方案的可使用性较差
>
> flex布局，解决上述两个的缺点，移动端使用flex更多，ie8不支持
>
> table-ceil布局，兼容性好，对SEO不够友好，每一个部分可以理解为单元格，当其中的一个表格高度变高，其他的两个表格也会变高
>
> grid布局，代码量简洁，新技术

- 如果中间高度不固定，哪个还可以用

> flex可以使用
>
> table-ceil可以使用
>
> float不能用，中间超出的内容 向左溢出
> 解决方案：创建BFC
>
> absolute不能用  中间无法适应
>
> grid不能用  文字会超出

- 总结

>语义化总结到位 --------dom结构一致，用section区分，用article表明容器，不要通篇div
>
>页面布局理解要深刻
>
>CSS基础知识扎实
>
>思维灵活且积极上进



####  CSS盒模型 

标准模型/默认模式 

- box-sizing:content-box

IE模型

- box-sizing:border-box

##### **区别：宽高的计算方式不同**

>属性高（height）和属性宽（width）这两个值不包括（padding）和边框（border）
>
>IE盒子模型——属性高（height）和属性宽（width）这两个值包含填充（padding）和边框（border）。 
##### **JS如何设置获取盒模型对应的宽和高** 

- dom.style.width/height 只能取出内联样式的宽高

- dom.currentStyle.width/height 只有IE支持

- dom.getBoundingClientRect().widht/height 计算元素的绝对位置，根据viewport/视窗左上角确定，会得到left top width height 4个元素  

  widht/height表示元素的宽和高（= text 的高度 + border + padding） 

- window.getComputedStyle(dom).width/height 所有浏览器支持 、兼容性好（渲染之后的宽高 ）

##### BFC（边距重叠解决方案）

BFC即块级格式化上下文

- 父子元素边距重叠 

  ```
  eg:子元素高度为100px，距父元素顶部10px，计算父元素的高度？
  1.因为父元素和子元素存在边距重叠，所以父元素高度为100px；
  2.父元素设置了overflow：hidden；之后父元素高度为110px；
  ```

- 兄弟元素/空元素边距重叠 

  ```
  eg:一个元素下边距为30px，下面那个元素上边距为5px，会合并成30px两者取最大值
  设置了overflow：hidden；之后margin会相加 30+5
  ```

- 补充**IFC**:内联格式化上下文 

- BFC的原理（BFC的渲染规则）

  ①BFC元素垂直方向的边距发生重叠 
  ②BFC的区域不会与浮动元素的box重叠 
  ③BFC是页面上一个独立的容器，内外的元素不会相互影响 
  ④计算BFC的高度时，浮动元素也参与计算。

- 如何创建BFC

  ①float值不为none 
  ②position为absolute或fixed 
  ③display为inline-block，table-cell，table-caption，flex； 
  ④overflow不为visible

- BFC使用场景 

  ①BFC垂直方向边距重叠

      /*垂直方向三个p元素，p{margin:5px auto 25px},之后第二个第三个的上边距变成25px；发生了边距重叠，取得的是最大值；如果不想重叠，就需要再那个元素在加一个父级div，对父级div创建BFC（overflow：hidden）*/
        <style>
            article{
                background: blue;
                overflow: auto;//这里overflow: auto后，article的高会算上第一个p元素的margin-top和最后一个p元素的margin-bottom
            }
            p{
                margin: 5px 0 25px;
                background: red;
            }
            div{
                overflow: auto;
            }
        </style>
        <section>
            <article>
                <p>1</p>
                <div>
                    <p>2</p>
                </div>
                <p>3</p>
            </article>
        </section>

  ②BFC不与float重叠

      /*两栏布局，左边宽度固定左浮动height：100px，右边自适应height：120px;右边的高度增高的时候，右边的背景色已经侵染到了左侧，解决方法：给右侧元素创建一个BFC（overflow：atuo）*/
      <style>
          article{
              background: blue;
          }
          .left{
              width: 100px;
              height: 100px;
              float: left;
              background: red;
          }
          .right{
              height: 110px;
              background: #ccc;
              overflow: hidden;
          }
      </style>
      <section>
          <article>
              <div class="left"></div>
              <div class="right"></div>
          </article>
      </section>

  ③清除浮动；（BFC子元素即使是float元素也会参与计算） 

  ```
  /*一个普通的元素当子元素是float元素的时候，它的高度计算没有包含进来--->给父级设置BFC之后，父级有了高度；---->当把外层元素设为BFC的时候，子元素的浮动元素也会参与父级的高度计算*/
  <style>
      section{
          overflow: auto;
          background: red;
      }
      .float{
          float: left;
      }
  </style>
  <section>
      <div class="float">我是浮动元素</div>
  </section>
  ```

#### HTTP协议类 

#####  **HTTP协议的主要特点** 

1. 简单快速：URI(统一资源符)是固定的
2. 灵活：HTTP协议头里有一个数据类型，通过HTTP协议可以完成不同数据类型的传输
3. 无连接：连接一次就会断开，不会保持连接
4. 无状态：客户端和服务端是两种，连接断开后，下次客户端再过来服务端是没法区分上一次连接和这一次连接是不是同一个身份。因为服务端是没有记住你的状态的。单从http协议上是不能区分两次连接者的身份的。

##### **HTTP报文的组成部分**  **(有请求有回应)** 

- 请求报文 

①请求行（http方法 页面地址 http协议及版本） 
②请求头（key、value值来告诉服务端我要那些内容，注意哪些类型） 
③空行（当遇到空行就知道不在是请求头部分了，就该当请求体来解析了）
④请求体

- 响应报文 

①状态行 （HTTP/1.1 200OK--http状态码）
②响应头 
③空行 
④响应体（比如：给的html文档就是响应体部分 ）

#####  **HTTP方法** 

```
GET-------------获取资源
POST------------传输资源
PUT-------------更新资源
DELETE----------删除资源(一般做业务是不删除资源的)
HEAD------------获取报文首部
```

##### get/post区别

```
   **1、GET在浏览器回退时是无害的，而POST会再次提交请求
     2、 GET产生的URL地址可以被收藏，而POST不可以
   **3、 GET请求会被浏览器主动缓存，而POST不会，除非手动设置
     4、 GET请求只能进行url编码，而POST支持多种编码方式
   **5、 GET请求参数会被完整保留在浏览器历史记录里，而POSt中的参数不会被保留
     --想把参数留在浏览器历史记录里用GET，做业务开发的时候为了防止crsf攻击，公司里都把GET请求
改成POST，如果业务需要把参数留在浏览器历史记录中记得把POST改成GET;
   **6、 GET请求在URL中传送的参数是有长度限制的(2kb)，而POST没有限制
     --如果用的是GET请求，拼接的url不要太长，否则会被浏览器截断，因为http协议对这个长度是
有限制的，发不出去，导致截断这种情况，一旦被截断，你的服务器拿不到正确的地址。
     7、对参数的数据类型，GET只接受ASCII字符，而POST没有限制
   **8、GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息
   **9、GET参数通过URL传递，POST放在Request body中
```

**什么时候用get？什么时候用post？**

当请求无副作用时（如进行搜索），便可使用GET方法；当请求有副作用时（如添加数据行），则用POST方法。

##### **HTTP状态码** （详细）

```
1xx: 提示信息--表示请求已接收，继续处理
2xx：成功--表示请求已被成功接收
200 OK：客户端请求成功
206 Partial Content：客户端发送了一个带有Range（范围）头的GET请求，服务器完成了它
（客户端请求一部分内容，在http head头中有一个range范围，请求0-10000字节，服务器就
返回一个206，服务器的文件是完整的，这个时候看到range头，按照range头从整个文件中截取一部分响应
给你，你响应体中只有你range头中指定的内容）
**使用video播放视频地址，audio播放音频地址，当视频文件、音频文件很大的时候基本都是返回的206;

3xx：重定向--要完成请求必须进行更进一步的操作
301 Moved Permanently：（永久重定向）所请求的页面已经转移至新的url
302 Found：            （临时重定向）所请求的页面已经临时转移至新的url
304 Not Modified：     客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户,原来缓冲的文档还可以继续使用。--服务器告诉浏览器已经有缓存了，可以直接从缓存中去取文档用，不用去服务器取了

4xx：客户端错误--请求有语法错误或请求无法实现
400 Bad Request：客户端请求有语法错误，不能被服务器所理解；
401 Unauthorized：请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用;
403 Forbidden：对被请求页面的访问被禁止；（资源禁止被访问）
--看到一个页面中一个地址，这个地址真的访问又不允许直接访问，只能通过服务器去访问;
404 Not Found：请求资源不存在；

5xx：服务器错误--服务器未能实现合法的请求
500 Internal Server Error：服务器发生不可预期的错误原来缓冲的文档还可以继续使用；
503 Server Unavailable：请求未完成，服务器临时过载或当机，一段时间后可能恢复正常；
```

##### **HTTP持久连接** 

- HTTP是支持持久连接的，1.1版本才支持，HTTP 1.0版本不支持
- HTTP协议采用“请求--应答”模式，当使用普通模式，即非Keep-Alive（持久连接）模式时，每个请求/应答客户端和服务器都要新建一个连接，完成之后立即断开连接（HTTP协议为无连接的协议）
- 当使用Keep-Alive模式（持久连接、连接重用）时，Keep-Alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive 功能避免了建立或者重新建立连接 

##### **管线化**（HTTP/1.1）

工作原理:将请求/响应打包回来；

```
在使用持久连接的情况下，某个连接上消息的传递类似于：
--请求1-->响应1-->请求2-->响应2-->请求3-->响应3（表示整个连接没有中断过）

某个连接上的消息变成了类似这样：（持久连接情况下完成的管线化）
请求1-->请求2-->请求3-->响应1-->响应2-->响应3

*1.管线化机制通过持久连接完成，仅HTTP/1.1支持此技术;
*2.只有GET和HEAD请求可以进行管线化，而POST则有所限制;
*3.初次创建连接时不应启动管线机制，因为对方（服务器）不一定支持HTTP/1.1版本的协议;
 4.管线化不会影响响应到来的顺序，如上面的例子所示，响应返回的顺序并未改变;
 5.HTTP/1.1要求服务器端支持管线化，但并不要求服务器端也对响应进行管线化处理，只是要
求对于管线化的请求不失败即可;
 6.由于上面提到的服务器端问题，开启管线化很可能并不会带来大幅度的性能提升，而且很多服务器端
和代理程序对管线化的支持并不好，因此现代浏览器如 Chrome 和 Firefox 默认并未开启管线化支持；
```

#### 继承

- 继承的本质（原理）就是原型链

##### **借助构造函数实现继承** 

缺点：只实现了部分继承，如果父类的属性都在构造函数上，那没有问题；如果父类的原型对象上还有方法（原型链的方法），子类是拿不到这些方法的

```
function Parent1(){
	this.name = "parent1";
}

function Child(){
    // 同时修改了父级构造函数this的指向，从而导致了父类执行的时候属性都会挂在Child类实例上去。
    Parent1.call(this);
    this.type="child1";
}

console.log(new Child());//Child {name: "parent1", type: "child1"}
```

##### **借助原型链实现继承（弥补构造函数实现继承不足）** ----可忽略 无意义

缺点：在一个类上实例了两个对象，改第一个对象的属性，第二个对象也跟着改变；
引起这个问题的原因：原型链上的原型对象是共用的；

```
function Parent2(){
    this.name ="parent2";
    this.play=[1,2,3];
}

function Child2(){
	this.type="child2";
}

Child2.prototype = new Parent2();
console.log(new Child2());

var s1= new Child2();
var s2= new Child2();

console.log(s1.play,s2.play);//[1,2,3];[1,2,3]

s1.play.push(4);
console.log(s1.play,s2.play);//[1,2,3,4];[1,2,3,4]

 s1.__proto__===s2.__proto__;//true
```

##### **组合方式(实现继承的通用方式)** 

缺点：父类的构造函数执行了2次，并且子类的constructor不是自己的，而是父类的

```
function Parent3(){
	this.name ="Parent3";
}

function Child3(){
    Parent3.call(this);//原型链上执行了1次
    this.type="child3";
}

//父类的构造函数执行了2次
Child3.prototype = new Parent3(); //new的时候执行了一次

var s3 = new Parent3();
var s4 = new Child3();

console.log(s4.constructor);//Parent3
----因为prototype直接拿的父类的实例，它没有自己的constructor，它的constructor是从这个实例中继承的，也就是原型链上一级拿过来的，它拿过来的肯定是父类的constructor
```

##### **组合继承的优化** (常用)

缺点：子类的constructor不是自己的，而是父类的

```
function Parent4(){
    this.name ="Parent4";
    this.play=[1,2,3];
}

function Child4(){
    Parent4.call(this);//父类只执行了一次
    this.type="child4";
}

//对象引用类型，引用同一个对象，说白是一个对象，缺点：它俩的构造函数指向的是一个
Child4.prototype = Parent4.prototype;

var parent = new Parent4();
var  = new Child4();

parent.play.push(4);

console.log(parent.play,child.play);//(4) [1, 2, 3, 4]     (3) [1, 2, 3]
console.log(p.constructor);//还是父类的构造器
/* Parent4(){
    this.name ="Parent4";
    this.play=[1,2,3];
} 
子类原型对象和父类的原型对象是一个对象，这个对象的constructor
就是父类里的constructor，这个父类的constructor指的是Parent4
*/
```

##### **组合继承优化2(完美版)** 

```
function Parent5(){
    this.name ="name";
    this.play=[1,2,3];
}

function Child5(){
    Parent5.call(this);
    this.type="Child5";
}
    
//Object.create(Parent5.prototype);创建中间对象方法，就把两个原型对象区分开，这个中间对象还具备一个特性就是它的原型对象是父类的原型对象，这样在原型链上又开始连起来了。

Child5.prototype = Object.create(Parent5.prototype);
//child = Object.create(Parent5)
Child5.prototype.constructor = Child5;//构造器指向自己的构造函数

var parent = new Parent5();
var child = new Child5();

parent.play.push(4);

console.log(parent,child); //Parent5 {name: "name", play: Array(4)}   //Child5 {name: "name", play: Array(3), type: "Child5"}
console.log(child.constructor); //Child5
```



#### hybrid

##### hybrid是什么，为何用hybrid

```
hybrid是客户端与前端的混合开发
hybrid存在的核心意义在意快速迭代，无需审核
hybrid实现流程，了解webview和file协议(本地文件传输协议)
```

##### hybrid更新和上线的流程

```
静态文件--file协议---webview
要点一： 服务端的版本和zip包维护
要点二： 更新zip包之前，先对比版本号
要点三： zip下载解压和覆盖
```

##### hybrid和h5的主要区别

```
优点： 体验好，可快速迭代
缺点： 开发成本高，运维成本高
适合的场景： hybrid适合产品型，H5适合运营型
```

##### 前端js和客户端通讯

- webview(html+js+css) ---->参数+回调函数 ---->客户端能力
- 通讯基本形式：调用能力，传递参数，监听回调

##### schema协议简介和使用

前端和客户端通讯的约定

可以通过ifream使用

```
//发起scheme协议
function require(schema){
    const iframe = document.createElement('iframe');
    ifram.style.display = 'none';//先隐藏不要占用空间
    iframe.src = 'URL scheme'; //iframe 访问 scheme 
    var body = document.body
    document.body.appendChild(iframe);
    //避免重复iframe
    setTimeout(function(){
        body.removeChild(iframe)；//销毁iframe
        iframe = null；
    })
}
```

##### scheme使用的封装

```
//闭包 防止全局污染
(function (window, undefined) {
    //分享 
    function invokeShare(data, callback){
        _invoke('share',data,callback)
    }
    //登录 
    function invokeLogin(data, callback){
        _invoke('login',data,callback)
    }
    //扫一扫
    function invokeScan(data, callback){
        _invoke('scan',data,callback)
    }
     // 暴露到全局变量
     window.invoke = {
         share: invokeShare,
         login: invokeLogin,
         scan: invokeScan
     }
     // 调用 schema 的封装
     function _invoke(action, data, callback) {
         // 拼装 schema 协议
         var schema = 'myapp://utils/' + action
         // 拼接参数
         schema += '?a=a'
         var key
         for (key in data) {
             if (data.hasOwnProperty(key)) {
                schema += '&' + key + data[key]
             }
         }
         // 处理 callback
         var callbackName = ''
         if (typeof callback === 'string') {
            callbackName = callback
         } else {
            //callback为函数
            callbackName = action + Date.now()
            window[callbackName] = callback
         }
         schema += '&callback=' + callbackName'

         // 调用 schema
         require(schema);
 	}
 })(window)
 
 
 
 //使用
 document.getElementById('btn2').addEventListener('click', function () {
            window.invoke.share({
            	//参数
                title: 'xxx',
                content: 'yyy'
                
                //callback
            }, function (result) {
                if (result.errno === 0) {
                    alert('分享成功')
                } else {
                    alert(result.message)
                }
            })
 })

```

以上js代码打包，叫做invoke.js，内置到客户端，内置上线，更快（免去请求）更安全

这样客户端每次启动webview，都默认执行invoke.js

#### **CSRF**/**XSS**

##### **CSRF**

**跨站请求伪造**: Cross-Site Request Forgery 缩写CSRF

```
1.用户是网站A的注册用户，通过身份验证登录网站A，登录之后网站A核查身份是不是正确的，
如果正确就下发cookie，这个cookie保存到用户浏览器当中，这就是完成了一次身份认证的过程。
2.用户又访问了一个网站B，网站B会给用户下发用户页面的时候，会存在引诱的一个点击，这个点击
往往是一个链接，就是指向网站A的一个API接口，接口是GET类型，比如：www.xxx.com/hack，
指向存在漏洞的接口，当用户经不住引诱点击了这个东西，这个点击就访问了A网站，访问A网站
这个链接的时候，浏览器会自动上传cookie，上传之后网站A觉得是A这个Cookie拿过来之后对身
份重新认证，发现是合法用户，就执行了这个接口的动作。 
```

**CSRF能够造成攻击的原理：**(两个因素)

网站中某一个接口存在漏洞
这个用户在注册网站确实登录过

**防御措施** 

```
 1.加Token验证
 访问接口的时候，浏览器自动上传cookie，但是没有手动上传一个Token，这个Token是你注册成功
 以后，或者没有注册，只要你访问了这个网站，服务器会自动的向你本地存储一个Token，在你访问
 各种接口的时候，如果没带Token,,就不能帮你通过验证，如果只是点击了引诱链接，这个链接只会
 自动携带cookie，不会自动携带Token，所以就避免了那个攻击。

2.Referer验证
Referer指的是页面来源，如果服务器判断页面来的是不是我的这个站点下面的页面，如果是就执行
你这个动作，如果不是就拦截。

3.隐藏令牌
和Token有点像，做法：隐藏在http的head头中，不会放在链接上，这样就做的比较隐蔽。本质上没
有太大区别。只是使用方式有一点差别。

4.验证码
```

##### **XSS** 

**跨域脚本攻击**：Cross-Site Scripting 缩写XSS

**攻击原理**：向你页面注入js脚本

**防御**：转义、字符串模板、后端处理

##### 区别

- **XSS是向你页面注入js运行，然后js函数体里面做它想做的事情。**
- **CSRF利用你本身的漏洞，去帮你自动执行那些接口。**
- **这两种攻击方式是不一样的，CSRF要依赖用户要登录网站**

#### 前端设计模式

 https://www.cnblogs.com/smlp/p/9776789.html 

