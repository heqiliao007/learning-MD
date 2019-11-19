## 一.js基础知识

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

#### 构造函数

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

#### 构造函数扩展

- var a={}其实是var a=new Object()的语法糖
- var a=[]其实是var a=new Array()的语法糖   
- function Foo(){...}其实是var Foo=new Function(...)

平常开发中，都是前面的写法

#### 原型规则和实例(是学习原型链的基础)

- 所有的引用类型(数组，对象，函数)，都具有对像特性，即可自由扩展属性（除了null）
- 所有的引用类型（数组，函数，对象），都有一个 ____proto____属性，属性值是一个普通对象
- 所有的**函数**，都有一个 prototype 属性，属性值也是一个普通的对象
- 函数的 **prototype** 称**显式原型**，引用类型的**____proto____** 成为**隐式原型**
- 所有的引用类型（数组，函数，对象），其____proto____  属性值都指向(等于)其构造函数的 prototype 属性值
- 当试图获取一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的____proto____ (即它的构造函数的prototype)中寻找

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
         console.log(obj.__proto__===Object.prototype)
 
 
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

构造函数.prototype 包含 constructor 和 _ proto_ 两个属性 

构造函数.prototype.constructor = 构造函数 (prototype属性中的constructor又指向构造函数本身 )

所有普通对象的原型链最终都会指向内置的 Object.prototype，而Object.prototype还会去 _ proto_ 里找，直到null

```
//接上一部分代码
f.toString()
//要去f.__proto__.__proto__中查找
//先去f对象的隐式原型__proto__上去找，因为f.__proto__指向f的构造函数Foo的显式原型prototype，prototype属性值也是一个对象，对象上没有toString方法，所有又向它的隐式原型__proto__上去找，执行的就是这个普通对象的构造函数Object
```

![](F:\gongsi\img\1.JPG)

#### instanceof

用于判断**引用类型**属于哪个**构造函数**的方法

f instanceof Foo的判断逻辑是：

- f的____proto____ 一层一层往上，能否对应到Foo.prototype
- 再试着判断 f instanceof Object

#### 2.1如何准确判断一个变量是数组类型

```
var arr = []
arr instanceof Array    // true
typeof无法判断 数组 返回为object
```

#### 2.2写一个原型链继承的例子

```
理解简单例子：
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
    Dog.prototype = new Animal()//使dog有animal的eat属性，//Dog的原型为Animal
    var hashiqi = new Dog()
}


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

养成阅读源码的好习惯

<http://www.imooc.com/learn/745> 

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

1、**内部**函数应**访问其外部**函数**中**声明的**私有变量**、参数或其他函数内部函数。

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

- 闭包实际应用中主要用于封装变量，收敛权限 
- 个人理解就是缓存数据(变量)

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

#### 单线程

js 是单线程的语言，所以需要异步 

```
{
    console.log(100) 
    setTimeout(function(){ 
    console.log(200) 
    }) 
    console.log(300) //100 300 200 
}
```

1. 执行第一行打印100
2. setTimeout 异步执行，先暂存起来
3. 打印 300
4. 待所有程序执行完，处于空闲状态时，拿到暂存的函数在指定的时间后执行

#### 4.1同步和异步的区别是什么？分别举一个同步和异步的例子 

①同步会阻塞代码执行，而异步不会 

②alert是同步，setTimeout是异步

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



#### 1.1dom是哪种基本的数据结构 

树 

#### 1.2Dom操作的常用API有哪些？ 

1、获取DOM节点/属性：.getElementById(还有class和tag)、.querySelectorAll、.getAttribute

2、获取父元素/子节点：.parentElements、.childNodes

3、新增/删除节点：.appendChild()、.removeChild()

PS

nodeType代表dom节点类型
1---元素节点  2---属性节点 3---text....

#### 1.3Dom节点的Attribute和Property有和区别？ 

①property只是一个JS对象的属性的修改  width/className
②Attribute是对html标签属性的修改 src img

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
        elem.addEventListenr(type, function(e){
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
####3.3对于一个无线下拉加载图片的页面，如何给每个图片绑定事件 
①使用代理 
②知道代理的两个优点 
代码简洁 
减少浏览器内存占用

###4.Ajax
#####核心API---XMLHttpRequest
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
###### 注：IE兼容性问题

①IE低版本使用ActiveXObject( new ActiveXObject() )，和W3C标准( new XMLHttpRequest() )不一样
②IE低版本使用量非常少，很多网站都早已不支持
③建议对IE低版本的兼容性了解即可，无需深究
④如果遇到对IE低版本要求苛刻的面试，果断放弃

#####状态码说明

readyState

0-（未初始化）还没有调用send()方法 
1-（载入）已调用send()方法，正在发送请求 
2-（载入完成）send()方法执行完成，已经接收得到全部响应内容 
3-(交互)正在解析响应内容 
4-（完成）响应内容解析完成，可以在客户端调用了

status

2xx-表示成功处理请求。如200 
3xx-需要重定向，浏览器直接跳转 
4xx-客户端请求错误，如404 
5xx-服务器端错误，如500

#####跨域

- 什么是跨域

  浏览器有同源策略（域名、端口、协议任一不同就算跨域），不允许ajax访问其他域接口

  http协议默认80，https默认端口443

  所有跨域请求都要经过信息提供方允许 

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

- JSONP

  **限制：JSONP只支持`GET`请求** 

  ```
  {
      从服务器加载一个文件，不一定服务器一定有这个文件，服务器可以根据请求动态生成这个文件
  网站A要跨域访问网站B的一个接口，B给A一个接口，约定返回内容格式如callback({x:100,y:200})(可动态生成),然后在前台定义好这个callback函数
  <script>
  	//定义
      window.callback = function(data){
          console.log(data);
      }
  </script>
  <script src="http://xxxxxxxx(B)"></script>//返回的是callback函数，之前已经定义了，这里相当于直接就执行，返回({x:100,y:200})，然后就输出了
  }
  ```

- 服务器端设置http header，CORS

  另一种解决跨域的方法，需要服务器做，前端要了解 

  未来跨域解决的趋势

  ```
  {
  	//第二个参数填写允许跨域的与名称，不建议写*
  	response.setHeader('Access-control-Allow-Origin)
  }
  
  ```

####手动编写一个ajax，不依赖于第三方库

```
{
    // 指定了请求目标，也明确了如何处理之后，就可以发送请求了
    var request = new XMLHttpRequest();
   
    request.onreadystatechange() = function(){

        if(request.readyState === 4){
            // 请求完成
            if(request.status === 200){
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

####跨域的几种实现方式

- jsonp
- nginx反向代理
- 服务器后端设置

#### 补充CORS

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
	git push // 把master分支的内容提交到线上
	
	git reset --soft HEAD^ //撤销本次提交，将本地仓库回滚到上一个版本，工作区和暂存区不变。
	HEAD^表示上个提交版本，HEAD^^表示上上个提交版本，HEAD~n代表前面第n个版本
	
}
```

#### 模块化 

- 使用模块化原因 

  如果不使用模块化，用多个js文件引用的方法，可能会造成全局变量污染（覆盖），并且依赖关系复杂也可能导致错误（你只知道你要依赖的js文件，却不知道你依赖的js文件里还有哪些依赖的js文件）。

  ![](F:\gongsi\img\2.JPG)

-   AMD

  异步模块定义 ，被define过的才能被require，define和require内的数组可以有多个元素，define和require内function的参数是他所引用的
  对象的返回。

### 性能优化





### get/post区别

**1.** get是从服务器上获取数据，post是向服务器传送数据。

**2**. GET请求把参数包含在URL中，将请求信息放在URL后面，POST请求通过request body传递参数，将请求信息放置在报文体中。

**3.** get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，IIS4中最大量为80KB，IIS5中为100KB。

**4.** get安全性非常低，get设计成传输数据，一般都在地址栏里面可以看到，post安全性较高，post传递数据比较隐私，所以在地址栏看不到， 如果没有加密，他们安全级别都是一样的，随便一个[监听器](https://www.baidu.com/s?wd=%E7%9B%91%E5%90%AC%E5%99%A8&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)都可以把所有的数据监听到。

**5.** GET请求能够被缓存，GET请求会保存在浏览器的浏览记录中，以GET请求的URL能够保存为浏览器书签，post请求不具有这些功能。

当请求无副作用时（如进行搜索），便可使用GET方法；当请求有副作用时（如添加数据行），则用POST方法。

若符合下列任一情况，则用POST方法：  * 请求的结果有持续性的副作用，例如，数据库内添加新的数据行。 * 若使用GET方法，则表单上收集的数据可能让URL过长。 * 要传送的数据不是采用7位的ASCII编码。  

若符合下列任一情况，则用GET方法：  * 请求是为了查找资源，HTML表单数据仅用来帮助搜索。 * 请求结果无持续性的副作用。 * 收集的数据及HTML表单内的输入字段名称的总长不超过1024个字符。 最早的搜索为了防止无限循环是这么做的，但是限制采用其他办法了，自然也没这个限制了  

### delete、vue.delete区别

delte会删除数组的值，但是它依然会在内存中占位置 而vue.delete会删除数组在内存中的占位 

### 小test:es6求阶乘箭头函数递归

```
{

	let one = n => {

		return n<1 ? 1 : n * one(n-1)

	}

	console.log(one(3))

}

```

currentTarget、target区别

currentTarget指的是事件触发后，冒泡到绑定处理程序的元素，就是绑定事件处理程序的元素，target指的是触发事件的元素 