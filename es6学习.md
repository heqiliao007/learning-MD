es6学习

[http://es6.ruanyifeng.com/](http://es6.ruanyifeng.com/)

### 一.项目构建

任务自动化：gulp

编译工具：

babel：es6=》es5或者更低（针对ie）

webpack：模块化，比如引入模块依赖

webpack-stream：webpack对gulp的支持



基础架构

业务逻辑：页面+交互

自动构建：编译+辅助(自动刷新、文件合并、资源压缩)

服务接口：数据+接口



几个知识点：

mkdir：创建文件夹

ls：查看当前文件夹

touch 初始化文件

clear：清屏

关于ejs：它是node.js使用的模板引擎，因此这里创建的是ejs文件而不是html

关于目录：

app：前端代码

server：服务器端

tasks：脚本构建化工具

express框架：

1. 安装nodejs后

2. 在项目server下执行express -e .  

   -e表示使用ejs模板引擎  .表示在当前文件夹执行

3. 再执行npm i

4. 回到构建目录，tasks目录补充，在tasks下创建util文件夹，在util文件夹下创建args.js文件。

5.在根目录下初始化package.json

npm init 一路回车

6.创建设置babal编译的文件 .babelrc

7.创建gulp构建工具gulpfile.babel.js

8.在task下创建scripts.js文件 引入相关包

9.安装8的那些包

```
npm install gulp gulp-if gulp-concat webpack webpack-stream vinyl-named gulp-livereload gulp-plumber gulp-rename gulp-uglify gulp-util yargs --save-dev
```

```
补充--save-dev和--save区别
模块在我们的项目部署后是不需要的，所以我们可以使用 -save-dev 的形式安装，并且会将模块依赖写入devDependencies 节点。有些模块是项目运行必备的，应该安装在 dependencies 节点下，所以我们应该使用 -save 的形式安装，会将模块依赖写入dependencies 节点。。
```

10.在scripts.js文件中去创建task任务（脚本服务）

11.创建模板脚本pages.js

12.创建css脚本css.js

13.服务器脚本server.js

14.当源文件（app文件夹下这一块）发生改变时让脚本文件自动去完成（编译）----创建browser.js

15.为了安全起见，重新覆盖拷贝的时候在tasks文件夹下创建清空制定文件clean.js去清空指定目录文件

16.gulp-live-server包等核心内容都写完后再安装  cnpm i gulp-live-server del --save-dev

17.新建一个脚本串起关联所有任务 build.js，并安装cnpm i gulp-sequence --save-dev

18.当命令行没有指定gulp xx脚本任务名时，就默认去找default.js，创建default.js

19.在srcipts.js中的webpack部分使用了babel-loader包，需要安装一下 （https://babeljs.io/setup#installation ），还有babel-loader 是依赖于babel-core的，

babel-preset-env ES语法分析包 

cnpm i babel-loader babel-core babel-preset-env

babel-core已升级

```
cnpm install @babel/core babel-loader --save-dev 
	
cnpm install @babel/preset-env --save-dev
```

20.配置gulpfile.babel.js，在js文件中去引入require-dir包 把task任务都引入，并安装require-dir包cnpm install require-dir --save-dev

21.配置.babelrc文件，用于配置babel，转成es2015

```
https://juejin.im/entry/59c4f9dd6fb9a00a4c271167
"presets": ["es2015"] 已被官方废弃

使用以下
"presets": ["@babel/preset-env"] 
```

ps：gulp4.x版本和3.x版本不一样

22.gulp --watch监听，如果执行gulp则程序执行一次

以上方式太复杂 改用webpack4.x版本打包

<https://dbafu.github.io/2018/09/02/%E4%BD%BF%E7%94%A8webpack%E6%90%AD%E5%BB%BAes6%E7%8E%AF%E5%A2%83-20180902/> 

安装@babel-polyfill补丁用于对低版本浏览器实现es6+

```
补充：
babel-polyfill 的原理是当运行环境中并没有实现的一些方法，babel-polyfill会做兼容。
babel-runtime 它是将es6编译成es5去执行。我们使用es6的语法来编写，最终会通过babel-runtime编译成es5.也就是说，不管浏览器是否支持ES6，只要是ES6的语法，它都会进行转码成ES5.所以就有很多冗余的代码。

babel-polyfill 它是通过向全局对象和内置对象的prototype上添加方法来实现的。比如运行环境中不支持Array.prototype.find 方法，引入polyfill, 我们就可以使用es6方法来编写了，但是缺点就是会造成全局空间污染。

babel-runtime: 它不会污染全局对象和内置对象的原型，比如说我们需要Promise，我们只需要import Promise from 'babel-runtime/core-js/promise'即可，这样不仅避免污染全局对象，而且可以减少不必要的代码。
```

babel7.X学习

<https://segmentfault.com/a/1190000017162255?utm_source=tag-newest> 

附

package.json

```
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "scripts": {
    "dev": "webpack-dev-server --config ./webpack.config.js --mode development"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.5.5",
    "@babel/plugin-proposal-decorators": "^7.6.0",
    "@babel/polyfill": "^7.4.4",
    "@babel/preset-env": "^7.5.5",
    "babel-loader": "^8.0.6",
    "html-webpack-plugin": "^3.2.0",
    "webpack": "^4.39.3",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  },
  "dependencies": {
    "css-loader": "^3.2.0",
    "style-loader": "^1.0.0"
  }
}
```

webpake.config.js

```
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: './src/index.js',
  output: {
    path: __dirname, //当前目录
    filename: './dist/bundle.js'  //自动创建
  },
  module: {
    rules: [
	    {
	      test: /\.js?$/,
	      exclude: /(node_modules)/,
	      use: {
	        loader: 'babel-loader'
	      }
	    },
	    {
		    test: /\.css$/, // 针对CSS结尾的文件设置LOADER
		    use: ['style-loader', 'css-loader']
		  }
    ],
  },
  plugins: [
  	new webpack.optimize.UglifyJsPlugin(),//压缩js插件
    new HtmlWebpackPlugin({
      template: './index.html',   // 项目模板
    })
  ],
  devServer: {
    contentBase: path.join(__dirname, './dist'),  // 根目录
    publicPath:"/",
    open: true,  // 自动打开浏览器
    port: 9000
  }
}
```

新版环境 6-10，免去配置环境

```
$ npm i es10-cli -g 
$ es10-cli create projectName 
$ cd projectName 
$ npm start
```

### 二.let和const

#### 作用域概念

es5两个作用域：全局和函数

**es5用var定义，没有用var定义的属于window的属性（不管是否在函数内部），不是真正意义的全局变量，但是因为window是全局项，拥有全局作用域，换句话说，它是可以被 delete 的，而全局变量不可以。**

-  在函数内部声明的变量，都会被提升到该函数开头，而在全局声明的变量，就会提升到全局作用域的顶部 
-  即使if语句的条件是false（程序未进入if判断，但在if判断里声明了变量），也一样不影响a变量提升 
-  在函数嵌套函数的场景下，变量只会提升到最近的一个函数顶部，而不会提升到外部函数 
-  **没有定义/声明和声明后没有赋值是不一样的** ，未声明报错“xx is not defined”，未赋值打印undefined

es6增加：块作用域（大括号包围）

临时死区的意思是在当前作用域的块内，在声明变量前的区域叫做**临时死区** 

```
function test(){
	var a = 1;
	for(let i=1;i<3;i++){
		console.log(i) 
	}
	console.log(i)//i is not defined 因为i是let在块级作用域声明的，外部访问不到，如果是var声明，会做变量提					//升，跳出循环体还可以打印3
}
test();
//第一个i输出1 2  第二个i报错

{
	console.log(name)//直接报错未定义 TDZ，俗称临时死区，用来描述变量不提升的现象
    let name = 'bbb';
}

{
 	function test() {
        if(true) {
          let a = 1
        }
        console.log(a)
    }    
    test() // a is not defined
    //程序先访问a 再声明 报错
}
```

```
{
	let ps = document.getElementsByTagName('p');
	for(let i = 0; i<ps.length; i++){
		var one = ps[i];
//		one.onclick = function () {
//			console.log(i)
//			//始终打印3，因为点击事件相当于回调函数，要被放到事件队列里，等主线程执行完才执行
//		}

		//解决方案1：闭包---立即执行函数
		//立即函数有自己私有的作用域
//		(function(i){
//			//传进来这里是形参，结尾的i是实参
//			one.onclick = function () {
//				console.log(i)
//			}
//		})(i)
		//解决方案2 let--有块级作用域
		one.onclick = function () {
			console.log(i)
		}
	}
	
}
```

##### 作用域总结

1.常见的作用域主要分为几个类型：全局作用域(global/window)、函数作用域(function)、块状作用域({}}、动态作用域(this)

2.作用域链
如果一个 变量 或者其他表达式不在 “当前的作用域”，那么JavaScript机制会继续沿着作用域链上（向上）查找直到全局作用域（global或浏览器中的 window）如果找不到将不可被使用。 作用域也可以根据代码层次分层，以便子作用域可以访问父作用域，通常是指沿着链式的作用域链查找 ，而不能从父作用域引用子作用域中的变量和引用 

两种方法外部拿到内部 屏蔽的变量，一是内部return 这个值，二是闭包

3.块作用域起效必须搭配let或者const，否则在块作用域里声明{ var a = 4 }此时a会因为js机制做变量提升到外部var a，形不成隔离块

4.作用域this是动态改变的，不等于window

```
window.a = 3;
function test(){
	console.log(this.a)
}
test(); //3
test.bind({a:100})(); //100
```

5.词法作用域（你不知道的JavaScript）

词法作用域：就是定义在词法阶段的作用域。在写代码时，将变量和块作用域写在哪里决定的。 
https://www.cnblogs.com/lwl0812/p/9792162.html

#### let总结

1.let声明的变量是有块作用域的， let 声明的全局变量不是全局对象的属性，也就是说不可以通过 window.变量名 的方式访问这些变量

2.es6强制使用严格模式，因此第二个i不是undefined而是报错not defined（变量未引用报错而不是undefined），l临时死区

3.使用let不可重复定义变量，若重复浏览器会报错 
Uncaught SyntaxError: Identifier 'a' has already been declared 

4.let不会预处理，不存在变量提升

5.循环遍历加监听，使用let取代var是趋势

```
function test1(){
	//声明一个常量
	const PI = 3.1415926;
	const k = {
		a:1
	}
	//增加一个value值
	k.b = 3;
	console.log(PI,k)//3.1415926  Object{a: 1,b: 3}
}
test1();
```

#### const总结

```
if(true) {
    const a = function(){

    }
} 
//请注意 以上是块级函数，也会被命名提升，除非在严格模式下
```

1.const只能定义常量，常量const不能修改/不能重复赋值（不严谨），报错Assignment to constant variable.
但是对象可以修改（因为对象是引用类型，返回的是存储内存的指针（地址），因此，是指针不可以改变const，但是指向这个指针的对象是可以变化的）

2.const也有作用域

3.const初始化声明的同时必须赋值！！报错Missing initializer in const declaration

### 三.解构赋值

#### 1.概念

从对象或数组中提取数组，并赋值给(多个)变量

左边一种结构，右边一种结构，一一对应进行赋值

#### 2.分类

数组、对象、字符串.....**凡是可遍历的对象都能解构赋值**(右边)

#### 2.规则

- 如果左右一致则是什么结构就赋值什么结构
- 如果左边是数组，右边是字符串=》字符串解构赋值
- 布尔值、数值解构赋值都是对象解构赋值的一种
- 函数参数解构赋值是数组解构赋值的一种应用
- **如果结构赋值没有成功配对(  比如没有对应的同名属性 )，变量会是undefined（默认值）**
- 对象的解构赋值是按照key，value值来的，左右都是一个对象

```
简单应用
{
	//块作用域内
	let a,b,rest;
	[a,b]=[1,2];
	console.log(a,b)//1 2
}
{
	let a,b,rest;
	[a,b,...rest]=[1,2,3,4,5,6];
	console.log(a,b,rest)
	//1 2 [3, 4, 5, 6]
}
{
	let a,b,c,rest;
	[a,b,c=3]=[1,2]
	//解构的数组不够时，会返回undefined，但是你可以给一个默认值比如c=3避免这种情况
	console.log(a,b,c)
	//1 2 3
}
{
	let a,b,c,rest;
	[a,b,c]=[1,2]
	console.log(a,b,c)
	//1 2 undefined
}
{
	let o = {p:42,q:true};
	let {p,q}=o;
	console.log(p,q)
	//42 true
}
{
	let {a=10，b=5}={a:3};
	console.log(a,b)
	//3 5 a被重新覆盖了 b仍为默认值
}
```

#### 4.使用场景

**(1).变量交互**

```
{
	let a = 1;
	let b = 2;
	[a,b]=[b,a]
	console.log(a,b)
	//2 1
}
```

(2).替代函数中的key值

```
{
	function f(){
		return [1,2]
	}
	let a,b;
	[a,b]=f()
	console.log(a,b)
	//1 2
}
```

(3).当返回了多个值，只取部分值
或者后台对象返回多种数据，只取其中一种
或者想要略过某个值（用逗号）

```
{
	function f(){
		return [1,2,3,4,5]
	}
	let a,b,c;
	[a, , , b]=f()
	console.log(a,b)
	//1 4 第12个逗号之间是2  第23个逗号之间是3
}
{
    //当后台返回多种数据，只取其中一种
    let obj = {name: 'heqilao', age: 18}；
    let {age} = obj;
    console.log(age)//18
}
```

(4).有未知或者已知多个值，只取第一个，剩下的赋值给一个数组
结合rest参数

```
{
	function f(){
		return [1,2,3,4,5]
	}
	let a,b,c;
	[a,...b]=f()
	console.log(a,b)
	//1 [2, 3, 4, 5]
}
```

(3)(4)混用

```
{
	function f(){
		return [1,2,3,4,5]
	}
	let a,b,c;
	[a, ,...b]=f()
	console.log(a,b)
	//1 [3, 4, 5]
}
```

**(5)嵌套对象取value值**

```
{
	let metaData = {
		title:'abc',
		test:[{
			title:'test',
			desc:'description'
		}]
	}
	let {title:esTitle,test:[{title:cnTitle}]}=metaData
	console.log(esTitle,cnTitle)
	//abc test
}
```

**(5) 数值与布尔值解构**

```
解构赋值时，等号右边是数值和布尔值，会先转为对象，数值和布尔值的包装对象都有toString属性，所以变量可以取到值。
let {toString:s} = 123;
//1.先将123转为对象 new Number(123)
//2.Nummber对象有toString方法，解构成功
let {toAbc:h} = 123;
//1.先将123转为对象 new Number(123)
//2.Nummber对象没有toAbc方法，解构失败ndefined

let { toString: s } = 123;
console.log(s === Number.prototype.toString); // true
let { toString: x } = true;
console.log(x === Boolean.prototype.toString);// true
s 是 toString 方法得到的结果 ，而 new Number(123) 有个 toString 方法，
执行这个方法得到的结果就是 s (f toString()) 了。
同理，true 转为对象是 new Boolean() ，有 toString的方法 ，执行这方法得到的结果就是 x 了。

let { toString: x } = undefined; // 报错
let { toString: y } = null; // 报错
undefined 和 null 无法转对象
```

**(7)应用于其他可遍历的对象**

```
//比如set map
let [a, ,b] = new set([1,2,3,4])
//set结构无法直接取某一个值，因为没有索引的概念，只能通过遍历，但是通过解构赋值可以实现取某个值
console.log(a,b); //1 3

//循环体的赋值
//[i,k]是一种隐式赋值
for (let [index, value] of ["1", "c", "ks"].entries()) {
    console.log(index, value);
    //0 '1'
    //1 'c'
    //2 'ks'
}
```



### 四.正则扩展

```
在es5中new一个正则表达式对象两种方式，并且第一种的第一个参数必须为字符串
{
	//es5
	let regex = new RegExp('xyz','i');
	let regex2 = new RegExp(/xyz/i);
	console.log(regex.test('xyz123'),regex2.test('xyz123'))//true true

	//es6 允许两个参数情况下第一个是正则表达式  第二个参数是修饰符  i覆盖ig
	let regex3 = new RegExp(/xyz/ig,'i');//i
	
	console.log(regex3)//xyz/i
	
	//flags是es6新增的用于获取修饰符
	console.log(regex3.flags)//i
	//i
}
```

#### g和y修饰符

都是全局匹配，

不同点：g 修饰符是从上次匹配的位置继续寻找，不一定是紧跟着的字符，y 修饰符必须是从下一个紧跟着的字符开始寻找。 

```
{
	let s = 'bbb_bb_b';
	let a1 = /b+/g;
	let a2 = /b+/y;
	console.log('one',a1.exec(s),a2.exec(s))//bbb bbb
	console.log('two',a1.exec(s),a2.exec(s))//bb undefined
	//RegExp.prototype.sticky属性判断是否开启y修饰符
	console.log(a1.sticky,a2.sticky)//false true
}
```

#### u修饰符

u实际就是unicode

3点：1.点修饰符并不是匹配所有字符  

​	  2.点不能识别：换行符 回车符  行分隔符 段分隔符

​	  3.大于两个字节(0xffff)的一定要加 u 才能正确识别

```
//ps：以下案例 console.log中逗号前面引号中的内容只是为了区分打印出来的东西而做的标识，没具体含义
// 没加 u 的会当成两个字符, 加 u 的会当成一个字符(字母)
{
	//u即代表(unicode) 
	console.log("u-1", /^\uD83D/.test("\uD83D\uDC2A"));// true  把D83D当成2个字符 匹配不上
	console.log("u-2", /^\uD83D/u.test("\uD83D\uDC2A"));// false  把D83D当成1个字符 刚好可以匹配上
}
{
  // 如果大括号内(即61)是 unicode 编码一定要加 u 修饰符才能被识别为unicode
  console.log(/\u{61}/.test("a")); // false  a的16进制unicode为61
  console.log(/\u{61}/u.test("a")); // true
}
{
//	大于两个字节(0xffff)即是4个字符 下面这个例子是5个字符 明显大于两个字节了
   console.log(`\u{20BB7}`); //𠮷
   let s = "𠮷";
   // .字符实际是并不是代表所有字符，只能识别小于等于两个字节的unicode编码
   console.log("u", /^.$/.test(s)); //false
   //加上u后就为true  说明如果字符串大于两个字节需要加上u修饰符
   console.log("u-1", /^.$/u.test(s)); //true
  	
   console.log("test-1", /𠮷{2}/.test("𠮷𠮷")); // false
   console.log("test-2", /𠮷{2}/u.test("𠮷𠮷")); // true
}

```

#### s修饰符

点不能识别：换行符 回车符  行分隔符 段分隔符

用s修饰 但是es6中尚未实现只是提案  es8支持

```
const re = /foo.bar/s;
// 另一种写法
// const re = new RegExp('foo.bar', 's');
//若不加s \n被当作换行符 返回false
re.test('foo\nbar') // true
re.dotAll // true
re.flags // 's'
```



### 五.字符串扩展

#### unicode表示法：大括号

```
{
	//u0061是Unicode码代表小写字母a
	console.log('a',`\u0061`);//a a
	//当字节大于两个0xffff(4个字符) 会变成2个字符 前面u20BB为一个字符
	//(在这里没有对应的 所以无法正常显示) 7单独拿出来做一个字符显示的7
	console.log('s',`\u20BB7`);//s ₻7
	
	//在es6中 大于2个字节的字符串需要用大括号包起来才会当作unicode编码
	console.log('s',`\u{20BB7}`);//𠮷
}
```

#### es6中对unicode的改良处理

```
{
  let s = "𠮷";
  // es5中 对unicode处理不到位
  console.log('length',s.length); // 2 码值大于两个字节按四个字节处理 2个字节是一个长度
  //es5  charAt 取字符编码
  //0取第一个字符编码
  console.log('0',s.charAt(0)); // 乱码
  //1取第二个字符编码
  console.log('1',s.charAt(1)); // 乱码
  //es5中取码值 charCodeAt 只取2个字节的码值
  console.log('ma0',s.charCodeAt(0)); // 55362
  console.log('ma1',s.charCodeAt(1)); // 57271
}
{
  let s1 = "𠮷a";
  console.log(s1.length);//3
  //ES6 中 codePointAt取码值(10进制) 自动取4个字节的码值
  console.log(s1.codePointAt(0)); //134071
  //转换为16进制
  console.log(s1.codePointAt(0).toString(16)); //20bb7
  console.log(s1.codePointAt(1)); //57271 和s.charCodeAt(1)一样 取后两个字节
  console.log(s1.codePointAt(1).toString(16)); //dfb7
  console.log(s1.codePointAt(2)); //97---a
  console.log(s1.codePointAt(2).toString(16));//61
}
{
	//codePointAt还可以判断一个字符由两个字节还是由四个字节组成
	function is32Bit(c) {
		return c.codePointAt(0) > 0xFFFF;
	}
	is32Bit("𠮷") // true
	is32Bit("a") // false
}
{
  //处理大于2个字节的unicode编码
  //es5  根据码值取对应字符
  console.log(String.fromCharCode("0x20bb7")); //ஷ 乱码了
  //es6
  console.log(String.fromCodePoint("0x20bb7")); //𠮷
}

```

#### 遍历器for...of

```
{
	//遍历器接口for...of 这个遍历器最大的优点是可以识别大于0xFFFF的码点
	let str = "\u{20bb7}abc"; //'𠮷abc'
    //es5
    for (let a = 0; a < str.length; a++) {
  		console.log("es5", str[a]); // � � a b c
  	}
  	//es6 const/let都可以
  	for (const code of str) {
    	console.log("es6", code); // 𠮷 a b c
  	}
}
```

#### 新增方法

##### includes

判断字符串中是否包含某些字符

**如果你只是需要匹配字符串中是否包含某子字符串，那么推荐使用xx.includes()，如果需要找到匹配字符串的位置，使用indexOf()** 

##### startsWith/endsWith

判断字符串是否以某些字符串为起始或者截至

##### repeat

 字符串复制功能

```
{
  let str = "string";
  // 判断字符串是否被包含
  console.log("includes", str.includes("r")); //true
  // 判断字符串起始 与 结束
  console.log("start", str.startsWith("str")); //true
  console.log("end", str.endsWith("ng")); //true
  // 字符串复制功能 指定重复的方法
  let str = "abc";
  console.log(str.repeat(2)); //abcabc
}
```



#### 模板字符串

把数据和模板拼成一个带结果的字符串

```
{
  //模板字符串
  let name = "list";
  let info = "hello world";
  let m = `i am ${name},${info}`;
  console.log(m); //i am list,hello world
}
{
	//函数式应用
	Price(strings, type){
	    let s1 = string[0]
	    var retailPrice = 20 
        var wholesalePrice = 16 
        var showTxt = '' 
        if (type === 'retail') { 
            showTxt += '购买单价是：' + retailPrice 
        } else { 
            showTxt += '批发价是：' + wholesalePrice 
        }
        return ${s1}${showTxt}
	}
	let showTxt = Price`你此次的${'retail'}`
	
}
{
	//换行 
	let a = `123
    456
    `
    console.log(a)
    // 123
    // 456
}
```

#### 两个重要的API

ES7语法，需要安装 babel-polyfill 兼容包  

参数：长度，补充内容

###### padStart()  (ES8)

 向前补白

###### padEnd()   (ES8) 

向后补白

```
{
  //补白  参数：第一个参数用来指定字符串的最小长度，补充内容  在日期、数字字符串处理方面常常用
  console.log("1".padStart(2, "0")); // 01
  console.log("1".padEnd(2, "0")); // 10
}
```

#### String.raw() 

String.raw() 返回模板字符串的原始字符串形式 

相当于对/n再进行转义又加了一个/即为//n

```
{
  // raw() 返回原始未加工的值，自带转义
  console.log(String.raw`H1\n${1 + 2}`); // H1\n3
  console.log(`H1\n${1 + 2}`);//H1
}
```



#### 标签模板

1.过滤html字符串时，防止XSS攻击

2.处理多语言转换时，不同的返回不同的结果

```
{
    let user = {
    name: "list",
    info: "hello world"
  };
  // 打印得到i am ,,,listhello world实际就是abc函数return的结果(拼接)
  console.log(abc`i am ${user.name},${user.info}`); 
  
  function abc(s, v1, v2) {
  	// ["i am " , "," , "" , raw: Array(3)] "list" "hello world"
    console.log(s, v1, v2);
	return s + v1 + v2
  }
}
```

### 六.数值扩展

#### 数字扩展

es6 中，二进制的表示方法都是以 0b 开头 (b大小写都可以)

es6 中，八进制的表示方法都是以 0o 开头 

```
{
	//二进制
	console.log(0b111110111);//503
	console.log(0B111110111);//503
	//八进制
	console.log(0o767); //503
	console.log(0O767); //503
}
```

#### 数值判断

它们与传统的全局方法`isFinite()`和`isNaN()`等的区别在于，传统方法先调用`Number()`将非数值的值转为数值，再进行判断，而这几个新方法只对数值有效！！！必须是数字 ，非数字一律false

##### Number.isFinite

判断一个数是否是有尽的

```
{
	//判断一个数是否是有尽的Number.isFinite
	console.log("15", Number.isFinite(15)); //true
	//NaN可以理解为非数字  非数字 --- 如果参数类型不是数值，Number.isFinite一律返回false
	console.log("NaN", Number.isFinite(NaN)); //false
	console.log("1/0", Number.isFinite(1 / 0)); // false
}
```

##### Number.isNaN

判断是否为NaN

```
{
	//判断是不是NaN----Number.isNaN
	//如果参数类型不是NaN，Number.isNaN一律返回false
	console.log("NaN", Number.isNaN(NaN)); //NaN true
  	console.log("0", Number.isNaN(0)); //0 false
}
```

##### Number.isInteger

判断是否整数

```
{
  //判断是否整数
  console.log(Number.isInteger(25)); //true
  console.log(Number.isInteger(25.0)); //true 25.0 等于 25
  console.log(Number.isInteger(25.1)); //false 25.1 不等于 25
  console.log(Number.isInteger("25")); //false  参数必须是数
}
```

##### 数的边界

```
{
	console.log(Number.MAX_SAFE_INTEGER); //9007199254740991 数的最大上限
    console.log(Number.MIN_SAFE_INTEGER); //-9007199254740991 数的最小下限
}
```

##### Number.isSafeInteger

判断安全范围内的整数 

```
{
	//	判断安全范围内的整数
   console.log(Number.isSafeInteger(10)); //true
   console.log(Number.isSafeInteger('a')); //false
   console.log(Number.isSafeInteger(10.4)); //false
}
```

##### Math.trunc

返回一个小数的整数部分 

在es5中可以向上取整Math.ceil    向下取整Math.floor

```
{
	//返回一个小数的整数部分
	console.log(Math.trunc(4.11)); //4
  	console.log(Math.trunc(4.9)); //4
}
```

##### Math.sign

判断一个数 正数\负数\零\NaN

```
{
	//判断一个数 正数 负数 零 NaN
      console.log(Math.sign(-5)); //-1
      console.log(Math.sign(0)); //0
      console.log(Math.sign(5)); //1
      console.log(Math.sign("50")); //1
      console.log(Math.sign("ffd")); //NaN
}
```

##### Math.cbrt

```
{
	//计算立方根因子
	console.log(Math.cbrt(-1)); //-1
	console.log(Math.cbrt(8)); //2
	console.log(Math.cbrt("gf")); //NaN
}
```

##### Math.hypot

三角函数：`Math.hypot`方法返回所有参数的平方和的平方根 

如果参数不是数值，`Math.hypot`方法会将其转为数值。只要有一个参数无法转为数值，就会返回 NaN 

```
Math.hypot(3, 4);        // 5
Math.hypot(3, 4, 5);     // 7.0710678118654755
```

##### 补充了解

Math.expm1() 

Math.log1p() 

Math.log10() 

Math.log2() 

以上几个去看阮一峰文档

### 七.数组扩展

#### 思考：es5有多少种数组遍历的方法？

for/ forEach/every/for....in
异同
1.forEach与for相比，写法更简洁，但不支持break，continue(因此也不支持return，因为forEach()无法在所有元素都传递给调用的函数之前终止遍历 )，其次，注意foreach内赋值问题，临时变量并不能 改变原对象的值 ，比如item = 1；要写成 arr[index] = 1;思考：而引用对象则不同，对象属性的改变会改变原对象（浅拷贝）
2.every能不能向下遍历取决去你rerurn 的值，当return true的时候，才能向下遍历，可以用return false去代替break，用return true代替continue
3.for...in是为object准备的，虽然能遍历数组（数组也是对象的一种），但是有问题，添加对象也会被循环出来(数组会被篡改)
for...in支持continue和break，但注意 它的索引index是字符串！！！

```
let array = [1,2,3,4]
arry.a = 8;//数组也是对象，可添加属性
for(let index in array){
	console.log(index,array[index])
	// 0 1 2 3 a
	// 1 2 3 4 8
	//index 为字符串类型，全等不了2
	if(index === 2){
		continue;
		console.log(index,array[index])
	}
	//把字符串变成数值 *1
	if(index*1 === 2){
		continue;
		console.log(index,array[index])
	}
}
//ps 浏览器控制台 灰色字符串 值是蓝色
```

#### 前置：es6增加遍历的方法？

for...of（除了数组和对象的遍历方法，es6允许自定义数据结构）

```
const Price = {
	A:[1, 2, 3, 4, 5],
	B:[1.5, 2.5, 3.5]
}
for(let key in Price){
	console.log(key)//无法遍历出AB数组的每一个值
}
// 要使Price可遍历，要使其具有iterator接口，对象不具有iterator接口
```

#### 数组转换

##### Array.from（如何将伪数组转换成数组）

将类数组（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）转真数组 

**伪数组就是具备数组的特性（长度、遍历），没有真数组的一般方法（无法直接调用数组的方法）**

语法Array.from(arryLike,mapFunction,thisArg) 
实质也是 ` [].slice.call(obj)`的实现

```
{
	//es5中如何实现
	let args = [].slice.call(obj);
	let imgs = [].slice.call(document.querySelectorAll("img"));
}
{
	//Array.from初始化并填充默认值
	//具有map也就是遍历的功能
	let array = Array.from({length:5},function(){
		return 1
	})
	
}
{
	// 把元素集合转义成数组
  	let p = document.querySelectorAll("p");
  	//es6如何实现
  	let pArr = Array.from(p);
  	//转换后可以用数组的forEach方法
  	pArr.forEach(item => {
    	console.log(item.textContent);
  	});
  	
  	//映射关系map
  	//[2, 6, 10]  函数起了一个map的映射作用
  	console.log(Array.from([1, 3, 5], function(item){return item * 2}));
  	//或者
	console.log(Array.from([1, 3, 5], item => item * 2));
  	// [2,6,10]
}
```

#### 如何生成新数组

##### Array.of

也可以用于将一组值，转换为数组 

`Array.of`总是返回参数值组成的数组。如果没有参数，就返回一个空数组。Array.of()这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异 

```
//es5 生成数组方法
let array = Array(5) 
//字面量方法 但是无法指定长度
let array = ['','']
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]

//es6生成方法
//Array.prototype.of  =》  Array.of
let arr = Array.of(1, 2, 7, 9, 11);

//Array.prototype.fill =》 Array.fill
let array = Array(5).fill(1); //[1, 1, 1, 1, 1]
```

原理分析`Array.of`方法可以用下面的代码模拟实现。

```
//将类数组转化为真正的数组
function ArrayOf(){
  //call作用是调用函数并改变函数内部this指向（使这个对象把这个函数或者说方法据为己有）
  //slice方法返回一个从开始到结束（不包括结束）浅拷贝到一个新数组对象，且原始数组不会被修改
  return [].slice.call(arguments);
}
```

例子

```
{
	//把一组数据变量转换成数组类型
    let arr = Array.of(3, "sd", 7, 9, 11);
  	console.log( arr); //[3, "sd", 7, 9, 11]
  	let arr = Array.of();
  	console.log("arr=", arr);//[]返回空数组
}

{
	//当new一个字符串的时候，生成的是该字符串为元素的数组
    const a = new Array(2) //长度、undefined
    const b = new Array("2") //值 但是是字符串
    console.log(a, b) //[undefined, undefined] ["2"]
    
    const c = Array.of(2) //值为数字，不会有歧义
    const d = Array.of("2")
    console.log(c, d) // [2] ["2"] 
}
```

##### fill

使用给定值，填充一个数组 
语法：Array.fill(value,start,end) start,end有默认值，默认所有

```
{
  	console.log([1, "a", undefined].fill(7)); // [7,7,7]
  	// 填充数值 后两个参数 起始位置（包含）---终止位置（不包含）
  	// 可以用于替换数组的值
  	console.log(["a", "b", "c", "d", "e"].fill(7, 1, 3)); //['a',7,7,'d','e']
}
```

#### 遍历相关

##### keys

返回数组索引

```
{
    for (let index of ["1", "c", "ks"].keys()) {
	    console.log(index);
	    //0
	    //1
	    //2
  	}
}
```

##### values

返回数组value值，values如果在没有poly-fill补丁下会有兼容问题

```
{
    for (let value of ["1", "c", "ks"].values()) {
	    console.log(value);
	    //1
	    //c
	    //ks
  	}
}
```

##### entries

返回索引与值

```
{
	//entries [i,k]是一种隐式赋值
	for (let [index, value] of ["1", "c", "ks"].entries()) {
	    console.log(index, value);
	    //0 '1'
	    //1 'c'
	    //2 'ks'
  	}
}
```

#### copyWithin

将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。使用这个方法，会改变原数组~~~

```
通俗来讲，就是将后面的复制给前面，当参数只有2个的时候，意为剩下的全部直到数组结束
{
	//参数target从该位置开始替换数据,start从该位置开始读取数据,end到该位置前停止读取数据
	//将3号位复制到0号位
	console.log([1, 2, 3, 4, 5].copyWithin(0, 3, 4)); //[4, 2, 3, 4, 5]
	
	//将-2即倒数第2位复制到0号位
	console.log([1, 2, 3, 4, 5].copyWithin(0, -2, -1));//[4,2,3,4,5]
	
	// 将2号位到数组结束，复制到0号位
	let i32a = new Int32Array([1, 2, 3, 4, 5]);
	i32a.copyWithin(0, 2);
	// Int32Array [3, 4, 5, 4, 5]
	
	console.log({length: 5, 3: 1})
	//	{length:5,3:1}
	// 将3号位复制到0号位
	console.log([].copyWithin.call({length: 5, 3: 1}, 0, 3))
	//	{0: 1, 3: 1, length: 5}
}


```

#### 如何查找数组

```
// es5中如何实现----filter
let array = [1,2,3,4,5];
let find = array.filter(function(){
	return item % 2 == 0 //筛选出所有符合条件的
})

```

##### find

用于找出**第一个**符合条件的数组成员 ，没有则返回undefined

```
{
	console.log([1, 2, 3, 4, 5, 6].find(item => item > 3)); //4
	console.log([1, 2, 3, 4, 5, 6].find(function(item){ return item > 3})); //4
}
```

##### findIndex

返回第一个符合条件的数组成员的**位置**，如果所有成员都不符合条件，则返回`-1` 

```
{
	console.log([1, 2, 3, 4, 5, 6].findIndex(item => item > 3)); //3
}
```

####  <a name="#includes">includes（es7）</a>

`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否**包含**给定的值 ，替代es5的indexOf

```
{
	console.log([1, 2, NaN].includes(1)); //true
  	console.log([1, 2, NaN].includes(NaN)); //true
}
```

#### flat（es10）

数组的成员有时还是数组，`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。 flat() 方法会按照一个可指定的深度递归遍历数组。 可指定的深度默认为1。


```
{
    [1, 2, [3, 4]].flat()
	//  [1, 2, 3, 4]
}
{
    //参数表示要拉开的层数 不写参数默认为1
    [1, 2, [3, [4, 5]]].flat(2)
	// [1, 2, 3, 4, 5]
	//如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数
	[1, [2, [3]]].flat(Infinity)
	// [1, 2, 3]
	//如果原数组有空位，flat()方法会跳过空位。
    [1, 2, , 4, 5].flat()
    // [1, 2, 4, 5]
}
```

#### flatMap（es10）

flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()方法 ，不改变原数组 

```
// `flatMap()`只能展开一层数组 
// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
[1, 2, 3, 4].flatMap(x => [[x * 2]])
// [[2], [4], [6], [8]]
```

#### 扩展运算符

不是很了解

替代函数的 apply 方法

```
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

将一个数组添加到另一个数组的尾部 

```
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```

#### 补充es5高阶函数

##### sort

s.sort(function(a,b){return a-b;});

作用就是比较两个数的大小用的,然后返回结果的正负作为排序的依据.
 这个函数是升序排序,如果想逆序排序改成return b-a;就行了

##### filter/map

```
var spread = [12, 5, 8, 8, 130, 44,130]
//三个参数：当前成员，当前位置和整个数组
var filtered = spread.filter((item, idx, arr) => {
  //indexOf找字符串首次出现的位置
  return arr.indexOf(item) === idx;
})
// 筛选符合条件找到的第一个索引值等于当前索引值的数组
console.log('数组去重结果', filtered)
```

filter方法是对原数组进行过滤筛选，产生一个新的数组对象

map方法对元素中的元素进行加工处理，产生一个新的数组对象。

ps：map遇见空数组位，会跳过空位，但会保留这个值

```
{
	function pow(x) {
        return x * x;
    }
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
    console.log(results);
}
{
	//数字转为字符串：
	var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
	arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
}
```

##### reduce

累计函数， callback  接受四个参数：初始值（上一次回调的返回值），当前元素值，当前索引，原数组 

 initialValue（可选的初始值。作为第一次调用回调函数时传给previousValue的值） 

 nitialValue 会多递归一次，而initialValue的值的作用大家应该也明了了：为累加等操作传入起始值（额外的加值） 

```
//数组 arr = [1,2,3,4] 求数组的和
//forEach 实现
var arr = [1,2,3,4],
sum = 0;
arr.forEach(function(e){sum += e;}); // sum = 10  just for demo
//map 实现
var arr = [1,2,3,4],
sum = 0;
arr.map(function(obj){sum += obj});//return undefined array. sum = 10  just for demo
//reduce实现
//不用定义sum值，直接累加初始值和当前元素值
var arr = [1,2,3,4];
arr.reduce(function(pre,cur){return pre + cur}); // return 10
```

##### 函数式编程：函数组合compose

简单需求复杂写法

```
  var testArr = ['start','middle','end'];
  // 函数1：定义header 输入一个数组 返回数组的第一个元素
  var header = arr => arr[0];
  
  //函数2: 定义reverse 输入一个数组 将数组中的元素倒转（顺便熟悉一下数组的归并方法reduce和reduceRight）
  var reverse = function(arr){
     return arr.reduceRight(function(prev,cur){
         return prev.concat([cur])
     },[])
  }
  //或以下写法
  var reverse = (arr)=>arr.reduceRight((prev,cur)=>prev.concat([cur]),[])
  console.log(reverse(testArr)); //  ["end", "middle", "start"]
  
  /*compose函数是一个封装 返回值也是一个函数 这个新函数传的参将被return的这个匿名函数接受
    所以这里需要用到reduce(跟reduceRight一样 只不过是从左往右) 函数的第二个参数 将这个需要处理的参数传进来作为	初始值 交给...fns中的函数去处理*/
  //相当于把arr交给reverse函数处理 然后返回值再交给header函数处理
    var compose = function(...fns){
      return function(arr){
          return fns.reduce(function(val,fn){
              return fn(val)
          },arr)
      }
  }
  
  或以下写法
  var compose = (...fns)=>(arr)=>fns.reduce((val,fn)=>fn(val),arr)
  var last = compose(reverse,header)
  console.log(last(testArr)); // 'end'
```



#### 空位的处理

es5对空位的处理

<http://es6.ruanyifeng.com/#docs/array#%E6%95%B0%E7%BB%84%E7%9A%84%E7%A9%BA%E4%BD%8D> 

```

forEach(), filter(), reduce(), every() 和some()都会跳过空位。
map()会跳过空位，但会保留这个值
join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串
```

ES6 则是明确将空位转为`undefined` 

比如`for...of`循环也会遍历空位 

`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined` 

`copyWithin()`会连空位一起拷贝。

```
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
```

### 八.函数扩展

#### 参数默认值

- 参数变量`x`是默认声明的，在函数体中，不能用`let`或`const`再次声明，否则会报错 
- 先取作用域内的默认值，如果作用域内没有再向外取值
-  **在ES6环境下，默认值对arguments的影响和ES5严格模式是同样的标准** 
-  参数不仅可以设置默认值为字符串，数字，数组或者对象，还可以是一个函数 (表达式)
-  没有参数值的往前写，有默认值的往后靠

```
{
	//es5怎么写参数默认值
	function test(x, y, x) {
    	if(y === "undefined"){
    		y = 7;
    	}
    	if(z === "undefined"){
    		z = 42;
    	}
    	return x + y + z;
    }
    test(1) //50
    
    //es6 
    function test(x, y = 7, z = 42) {
    	
    	return x + y + z;
    }
    test(1, undefined, 43); //传undefined后es6会跳过这个值
}
{
	let x = "test";
	// 默认参数可以是运算表达式
    function test2(x, y = x) {
    	console.log("作用域", x, y);
    }
    test2("kaka"); // kaka kaka
}
{
    let x = "test";
    function test2(c, y = x) {
    	console.log("作用域", c, y);
    }
    test2("kaka"); // kaka test
    //不赋值的话是undefined
}
{
	//在ES5的严格模式下，arguments（函数参数的个数）就不能在函数内修改默认值后跟随着跟新了
	//如非严格模式下，则arguments的值会跟随默认值的改变而改变
    "use strict"; //严格模式   
    function a(num) {
      console.log(num === arguments[0]); // true
      num = 2;
      console.log(num === arguments[0]); // false
    }
    a(1);
}
{
	//es5中如果你想知道参数的个数
	console.log(arguments.length);
	//可以想知道各参数的值
    console.log(Array.from(arguments));
    
    //es6中
    fn.length //但是只知道定义时函数没有默认值参数的个数
}
{
	//默认参数的临时死区
    //这是个默认参数临时死区的例子，当初始化a时，b还没有声明，所以第一个参数对b来说就是临时死区。
    function add(a = b, b){
      console.log(a + b)
    }
    add(undefined, 2) // b is not define
}
```

#### 怎么处理不确定参数：rest参数(...变量名 )  =》收

- 利用 rest 参数，可以向该函数传入任意数目的参数 ，就是**取代arguments**

- 参数不确定，收敛到函数中

- 注意，**rest 参数指的是最后一个参数**，所以之后不能再有其他参数（即只能是最后一个参数），否则会报错！！！ 并且 不能用于对象字面量setter方法中 


```
{
	//es5写一个求和的方法
	function sum(){
		let num = 0;
		Array.prototype.forEach.call(arguments,function(item){
			num += item * 1;
		})
		//方式2
		Array.from(arguments).forEach(function(item){
			num += item * 1;
		})
		return num;
	}
	//es6的方式
	//改动，让第一个参数*2的基础上再求和
	function sum(base, ...nums){
		let num = 0;
		nums.forEach(function(item){
			num += item * 1;
		})
		return base * 2 + num;
	}
}
{ 
	function test3(...arg) { //使用...（展开运算符）的参数就是不定参数
		console.log(arguments)//伪数组，不是真正的数组，没有数组的方法，但是有length属性
		console.log(Array.from(arguments))//可以查看各参数的值
		console.log(arg)//是真正的数组,可以用let...of方法去遍历
	    for (let v of arg) {
	      console.log("rest", v);
	      // 1
	      // 5
	      // a
	    }
  	}
   test3(1, 2, 3, 4, "a");
   /*rest 1
   rest 2
   rest 3
   rest 4
   rest a*/
}
```

##### 补充es5：arguments.callee（）

​	返回函数本身

#### 扩展运算符/展开运算符：rest参数的逆运算  =》 打散

- rest参数和扩展操作符拥有相同的语法，不同的是，rest参数是把所有的参数收集起来转换成数组，而扩展操作符是把数组扩展成单独的参数 (离散)，可以理解为互为逆运用
- 函数参数是确定的，打散进去
- 注意，扩展运算符后还可以有其他值，不要和rest参数混淆!!!!!
- 拓展运算符是按顺序展开的


```
{
	//es5的做法 
	fn.apply(this, data);
	//es6做法
	fn(...data);
}
{
     // 把数组转成离散的值
    let data = [1, 2, 4];
  	console.log("a", ...data); //a 1 2 4
  	
    //ES6的写法
    let arr = [10, 20, 50, 40, 30]
    let a = Math.max(...arr)
    console.log(a) // 50
}
```

#### 箭头函数

三部分组成：函数名+参数+返回值

```
//如下，这里定义的arrow实际是一个函数名，v是参数，箭头指向参数返回值
let arrow = v => v*2;
console.log(arrow(3)); //6
//如果参数为空，就用空括号
let arrow2 = () => 5;
console.log(arrow2()); //5
```

使用：

形参的情况（箭头左边）

- 没有形参的时候，空括号不能省略
- 只有一个形参的时候，括号可以省略，只写参数
- 2个及以上形参，小括号不能省略

函数体的情况（箭头右边）

- 函数体只有一条语句或者表达式的时候可省略{}，会默认返回结果
- 函数体多条语句或者表达式的时候不可省略{}，需要手动返回（return）

##### 补充：this绑定

es5: 函数被调用时的对象（可变的）
es6: 定义时候的对象（不可变的）

（1）函数体内的`this`对象，就是定义时所在的对象，而不是调用时所在的对象。

   所谓定义时所在的对象，要看箭头函数外层是否有函数

- **如果有，外层函数的this就是内部箭头函数的this**

- **如果没有，它所处的对象即是window，则this指向window**

  ```
  //比较es6与es5的this指向的区别
  {
  	//es5
      function fn() {
          console.log(this) //{a:100}
          
          var arr = [1,2,3]
          arr.map(function(item){
          	console.log(this) //window
          })
          //在这里写setTimeout函数，结果和map函数相同，this也是一样的道理
      }
      
      fn.call({a:100})
  }
  
  {
  	//es6
      function fn() {
          console.log(this) //{a:100}
          
          var arr = [1,2,3]
          arr.map(item => {
          	console.log(this) //{a:100} 箭头函数外层有函数,外层函数的this就是内部箭头函数的this
          })
      }
      
      fn.call({a:100})
  }
  ```

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

（5）不可以使用`super`、`new.target` 

（6）没有原型

（7）不支持重复的命名参数。

**箭头函数可以与变量解构结合使用** 

```
{
    const full = ({ first, last }) => first + ' ' + last;

    // 等同于
    function full(person) {
      return person.first + ' ' + person.last;
	}
}
```

 **rest 参数与箭头函数结合** 

```
{
    const headAndTail = (head, ...tail) => [head, tail];

    headAndTail(1, 2, 3, 4, 5)
    // [1,[2,3,4,5]]
}
```

#####  箭头函数写立即执行函数表达式 

```
{
    const test = ((id) => {
      return {
        getId() {
          console.log(id)
        }
      }
    })(18)
    test.getId() // 18
}
```

#### 尾调用

函数的最后一句话是否是调用函数

用于性能提升、优化

```
"use strict";   
function a() {
	return b();
}


function f(x){
  g(x);
}
//这种情况不属于尾调用，因为最后相当于是有一步操作：return undefined;
```

如果尾调用自身，就称为尾递归 

```
//新型尾优化写法
"use strict";  
function a(n, p = 1) {
    if(n <= 1) {
    	return 1 * p
    }
    let s = n * p
    	return a(n - 1, s)
    }
//求 1 x 2 x 3的阶乘
let sum = a(3)
console.log(sum) // 6
```



### 九.对象扩展

#### 简介表示法

如果名和值相同，可以省略只写一个

```
{
    let o = 1;
    let k = 2;
    let es5 = { o: o, k: k };
    let es6 = { o, k };
    console.log(es5);
    console.log(es6);
}
```

#### 属性表示法（中括号）

可以是变量或者是表达式

```
{
	  let a = "b";
	  let es5_obj = {
	    a: "c"
	  };
	  // 中括号包起变量，a 是变量，变量的值是b
	  let es6_obj = {
	  	//这种写法相当于在a的基础上再去加一个b
	    [a]: "c"
	  };
	  console.log(es5_obj); //{a: "c"}
	  console.log(es6_obj); //{b: "c"}
}

{
	//es5写法
	var obj = { 
		foo: 'bar' 
	}
	obj['baz' + quux()] = 42
	
	//es6写法
	let obj = { 
		foo: 'bar', 
		['baz'+ quux()]: 42 
	}
}

```

方法简写

```
//包括常规函数和异步函数
let obj = { 
    foo (a, b) {
    },
    bar (x, y) { 
    },
    //表示generator函数
    * quux (x, y) { 
    } 
}
```



#### super关键字

ES6 又新增了另一个类似的关键字`super`，指向当前对象的原型对象。 

<http://es6.ruanyifeng.com/#docs/object#super-%E5%85%B3%E9%94%AE%E5%AD%97> 

#### 解构赋值

左边解构的变量要和右边对象的属性名一致

```
{
	//嵌套的
    let obj = { a: { b: 1 } };
    let { ...x } = obj;
    obj.a.b = 2;
    x.a.b // 2
}
{
	let option = {
		title: 'menu',
		width: 100
	}
	let { title:title2, width = 130 } = option;
	console.log(title2,width) //menu 130
}
```

解构赋值的拷贝是***浅拷贝***！！！！！！！！！！！！！，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。 

扩展运算符的解构赋值，不能复制继承自原型对象的属性 

```
{
    let o1 = { a: 1 };
    let o2 = { b: 2 };
    o2.__proto__ = o1;
    let { ...o3 } = o2;
    o3 // { b: 2 }
    //对象o3复制了o2，但是只复制了o2自身的属性，没有复制它的原型对象o1的属性
    o3.a // undefined
}
```

```
{
	//Object.create()方法创建一个新对象,使用现有的对象来提供新创建的对象的__proto__
	//Object.setPrototypeOf()（写操作）、Object.getPrototypeOf()（读操作）、Object.create()（生成操作）代替__proto__
	const o = Object.create({ x: 1, y: 2 });
	console.log(o)//{}
	
	o.z = 3;
	console.log(o)//{z: 3}
	
	let { x, ...newObj } = o;
	//let { x, ...{ y, z } } = o;不能写成这种形式
	//因为如果使用解构赋值，扩展运算符后面必须是一个变量名，而不能是一个解构赋值表达式
	console.log(x,newObj)//1 {z: 3}  y没有解构出来
	
	let { y, z } = newObj;
     x // 1
     y // undefined y没有解构出来
     z // 3
}
```

 **嵌套对象解构** 

一般解构赋值应用于
1.函数传入参数很多的时候，比如，postHttp({...filter})或者delHttp([...filter])
2.对请求的接口返回的数据进行解构

```
{
  //解构的结构要和源对象结构保持一致
   let obj = {
      a: {
        b: {
          c: 5
        }
      },
      d: [1,5]
    }
    const {a: {b},d: [d1]} = obj
    console.log(b.c,d1) // 5 1
    const {a: {b},d: [,d2]} = obj
    console.log(b.c,d1) // 5 5
}

{
    const { data: { code, data } } = await api.post('shop/getShopBaseInfo', config)
    //把data下的code和data单独解构出来
}
```



#### 扩展运算符(es7)

```
 let { a, b, ...c } = { a: "test", b: "kill", c: "ddd", d: "ccc" };
 console.log(c)
 // c= {
 //   c:ddd,
 //   d:ddd
 // }
```

如果扩展运算符后面是一个空对象，则没有任何效果 

```
{...{}, a: 1}
// { a: 1 }
```

如果扩展运算符后面不是对象，则会自动将其转为对象 

```
// 等同于 {...Object(1)}
//扩展运算符后面是整数1，会自动转为数值的包装对象Number{1}。由于该对象没有自身属性，所以返回一个空对象
{...1} // {}

// 等同于 {...Object(true)}
{...true} // {}

// 等同于 {...Object(undefined)}
{...undefined} // {}

// 等同于 {...Object(null)}
{...null} // {}
```

如果扩展运算符后面是字符串，它会自动转成一个类似数组的对象 

```
{...'hello'}
// {0: "h", 1: "e", 2: "l", 3: "l", 4: "o"}

```

**ps：对象的扩展运算符等同于使用`Object.assign()`方法 （浅拷贝）**

```
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);

let aClone = { ...a, ...b};
// 等同于
let aClone = Object.assign({}, a, b);

```

ps:对象的深拷贝见文档！

扩展运算符可以用于合并两个对象 

```
let ab = { ...a, ...b };
// 等同于
let ab = Object.assign({}, a, b);
```



#### Object新增方法

##### Object.is

比较两个值是否相等，等同 ===

```
{
	console.log(Object.is("abc", "abc")); //true  
	// 引用类型(数组\对象) 引用两个不同地址
  	console.log(Object.is([], [])); //false
}
```

不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。

```
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

##### Object.assign(浅拷贝)

参数：要拷贝到的对象上  要拷贝的值/对象

 Object.assign(target, ...sources) 实际上是实现了一个mixin

```
//mixin不只有这一种实现方法。
function mixin(receiver, supplier) {
    Object.keys(supplier).forEach((key) => {
    	receiver[key] = supplier[key]
    })
	return receiver
}

let a = {name: 'sb'};
let b = {
    c: {
    	d: 5
    }
}
console.log(mixin(a, b)) // {"name":"sb","c":{"d":5}}
```

最后合并成一个对象，只会拷贝自身对象的属性，不会拷贝继承属性，不会拷贝不可枚举的属性， 如果`undefined`和`null`不在首参数，就不会报错 ，首参数表示target，表示target不能为undefined或null，而sources可以

限制：浅拷贝

```
let target = {
    a: {
    	b: {
    		c: {
    			aa: 1
    		}
    	}
    },
    d: 6,
    e: 7,
    f: 8
}
let source = {
    a: {
    	b: {
    		c: {
    			aa: 1
    		}
    	}
    },
    d: 9,
    e: 10
}
Object.assign(target,...source);
console.log("拷贝",target)
```

 **一个实现Component的例子** 

```
//声明一个对象Component
let Component = {}
//给对象添加原型方法
Component.prototype = {
    componentWillMount() {},
    componentDidMount() {},
    render() {console.log('render')}
}
//定义一个新的对象
let MyComponent = {}
//拷贝Component的方法和属性。
Object.assign(MyComponent, Component.prototype)

console.log(MyComponent.render()) // render
```

##### 补充es5：不可枚举

枚举是指对象中的属性是否可以遍历出来，再简单点说就是属性是否可以列举出来 

属性的枚举性会影响以下三个函数的结果：

for…in

Object.keys()

JSON.stringify

枚举方法的区别：<https://blog.csdn.net/it_hlm/article/details/79555118> 

##### Object.keys()、Object.values()

<http://es6.ruanyifeng.com/#docs/object-methods#Object-keys%EF%BC%8CObject-values%EF%BC%8CObject-entries> 

##### Object.entries()

```
{
	  let test = { k: 123, o: 456 };
	  //Object.entries作用：把不支持for...of的对象变成可遍历对象
	  //[key,value]是一种解构赋值的写法
	  for (let [key, value] of Object.entries(test)) {
	    console.log([key, value]);
	    // ["k", 123]
	    // ["o", 456]
      }
}

{
	//es5
	for (let item in test) {
	    if(item === 'k'){
	    	
	    }
     }
	//es6更优雅的写法
	Object.keys(test).filter(item = > item === 'k')
}
```

##### 直接操作   `__proto__ `对象

在js中，`__proto__ `指向原型对象，这条线叫做隐式原型，本身是不可操作的，可操作的是构造函数指向的原型对象prototype

```
{
    let obj = {};
    let obj2 = {money: 666};
    obj.__proto__ = obj2;
    console.log(obj.money)//666
}
```

##### 增强对象原型

允许改变对象原型

改变对象原型，是指在对象实例化之后，可以改变对象原型。我们使用 Object.setPrototypeOf() 来改变实例化后的对象原型。

```
let a = {
    name() {
    	return 'eryue'
    }
}
let b = Object.create(a)
console.log(b.name()) // eryue
      
//使用setPrototypeOf改变b的原型
let c = {
   name() {
     return "sb"
   }
}    
Object.setPrototypeOf(b, c)    
console.log(b.name()) //sb
```



#### 自有属性枚举顺序

 规律，就是数字提前，按顺序排序，接着是字母排序。**Object.keys(), for in** 等方法也支持枚举自动排序 

```
const state = {
    id: 1,
    5: 5,
    name: "eryue",
    3: 3
}
    
Object.getOwnPropertyNames(state) 
//["3","5","id","name"] 枚举key

Object.assign(state, null)
//{"3":3,"5":5,"id":1,"name":"eryue"} 
```

#### 重复的对象字面量属性

ES5的严格模式下，如果你的对象中出现了key相同的情况，那么就会抛出错误。而在ES6的严格模式下，不会报错，后面的key会覆盖掉前面相同的key。

```
    const state = {
      id: 1,
      id: 2
    }
    console.log(state.id) // 2
```

### 十.Symbol数据类型

#### 1.Symbol概念

它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object） 

- Symbol提供一个独一无二的值 ，即是不重复不相等
- Symbol值不能与其他数据类型进行计算，包括字符串拼接
- for...in ,for ....of遍历时不会遍历symbol属性

```
{
 注意！！！Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象
  //声明
  let a1 = Symbol();
  let a2 = Symbol();
  console.log(a1 === a2); //false
}
```

```
另一种声明方式，便于取值Symbol.for
{
    // 设定key值，同key的值是相等的
      let a3 = Symbol.for("a3");
      let a4 = Symbol.for("a3");
      console.log(a3,a4); //Symbol(a3) Symbol(a3)
      console.log(a3 === a4); //true
}
```



#### 2.Symbol作用(使用场景)

##### 同键值不会造成冲突（传参标识）

```
{
  // 同键值不会造成冲突
  let a1 = Symbol("abc");
  let obj = {
    [a1]: "123",
    abc: 345,
    c: 456
  };
  console.log(obj); //{abc: 345, c: 456, Symbol(abc): "123"}
}
{
  // 同键值不会造成冲突
  let a1 = Symbol.for("abc");
  let symbol = Symbol();
  let obj = {
    [a1]: "123",
    abc: 345,
    c: 456
  };
  obj[symbol] = 'hello';
  console.log(obj); //{abc: 345, c: 456, Symbol(abc): "123", Symbol(): "hello"}
}
```

##### **symbol可以定义常量**

##### **symbol可以内置symbol值**

对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法 (具体看iterator部分)

#### 3.symbol的遍历

- ps:对象中有用到symbol作key值时，for in 和 for of 是拿不到属性的

```
{
     let a1 = Symbol.for("abc");
     let obj = {
        [a1]: "123",
        abc: 345,
        c: 456
     };
     for (let [key, value] of Object.entries(obj)) {
        console.log(key, value);
        // abc 345
        // c 456
      }
    //for in 遍历对象与数组
    for (let key in obj) {
        console.log(key, obj[key]);
        // abc 345
        // c 456
      }
}
```

##### Object.getOwnPropertySymbols

一个api，取Symbol的属性，得到的是一个数组，并且只取symbol

```
{
    Object.getOwnPropertySymbols(obj).forEach(item => {
    	console.log(obj[item]);
    	// 123 (只取到symbol)
  });
}
```

##### Reflect.ownKeys

使用Reflect, 返回所有key、valueh值，包含Symbol变量作为key值的属性，也包含了非Symbol的属性，返回的也是一个数组

```
 //得到的数组遍历一下
 Reflect.ownKeys(obj).forEach(item => {
    console.log(item, obj[item]);
    // abc 345
    // c 456
    // Symbol(abc) '123'
  });
```

### 十一.Set 和 Map 数据结构

#### set 

- 类似数组，特性：集合中的元素不能重复 + 可遍历对象（可遍历的不一定是数组）
- 无序不可重复的多个value的集合体（重复的可自动过滤）
- 本质还是object对象，只是key和value值一样


```
//两种形式，加参数/不加参数
{
  let list = new Set();
  //存储数据
  //增加元素的方法
  list.add(5);
  list.add(7);
  //或者链式结构
  list.add(5).add(7);

  //获取长度
  console.log(list.size); //2
}
{
  // 将数组转换为 Set 集合
  let arr = [1, 2, 3, 4, 5];
  let list = new Set(arr);
  console.log(list.size); //5
}
{
  let list = new Set();
  list.add(1);
  list.add(2);
  // 添加重复元素不报错，但是不生效
  list.add(1);
  console.log(list); //Set(2) {1, 2}
}
```

##### 数组去重

**ps:转换类型的时候不做数据类型的转换，比如2和‘2’去不了重**

```
{
  //数组去重, 但不能过滤数据类型
  let arr = [1, 3, 3, 4, 5, 7, 7, 7];
  let list2 = new Set(arr);
  console.log(list2); //Set(5) {1, 3, 4, 5, 7}
  //把set类型转换成数组
  console.log([...list2]); //[1, 3, 4, 5, 7]
}
//面试经常遇到的去重
{
    const arr = [1, 1, 'haha', 'haha', null, null]
    let set = new Set(arr);
    console.log(Array.from(set)) // [1, 'haha', null]
    console.log([...set]) // [1, 'haha', null]
}
```

##### api方法

```
{
  //常用API
  let arr = ["add", "delete", "clear", "has"];
  let list = new Set(arr);
  //长度属性(统计条数)
  list.size
  //增加
  console.log(list.add("add1")); //Set(5) {"add", "delete", "clear", "has", "add1"}
  // 包含（查）
  console.log(list.has("add")); //true
  // 删除
  console.log(list.delete("add"), list); //true , Set(3) {"delete", "clear", "has"}
  // 清空
  list.clear();
  console.log(list); //Set(0) {}
  
  //如果想实现改 建议先删再增加
}
```

##### 遍历（读取数据）

1.for...of

-  Set 的 key 和 value 都是一样的，都是元素的名称 ,返回的都是Iterator对象

```
{
    let arr = ["add", "delete", "clear", "has"];
      let list = new Set(arr);
	  //遍历key 
	  console.log(list.keys()); //SetIterator {"add", "delete", "clear", "has"}
	  //Iterator对象可以用for...of方法
      for (let key of list.keys()) {
        console.log("keys", key);
        // keys add
        // keys delete
        // keys clear
        // keys has
      }
       //遍历value
      for (let value of list.values()) {
        console.log("values", value);
        // values add
        // values delete
        // values clear
        // values has
      }
       //遍历list
      for (let value of list) {
        console.log("list", value);
        // list add
        // list delete
        // list clear
        // list has
      }
      //以上三种结果都一样
 }
 {
 	//entries遍历key、value 键值对
     for (let [key, value] of list.entries()) {
        console.log("entris", key, value);
        // entris add add
        // entris delete delete
        // entris clear clear
        // entris has has
      }
 }
```

2.forEach  

```
{
    list.forEach(item => {
        console.log(item);
        // add
        // delete
        // clear
        // has
    });
}
```

##### Set 实现并集（Union）、交集（Intersect）和差集（Difference） 

```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集（不重复）
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```



#### map 

- 类似对象，但是**key值为任意数据类型**，可遍历对象
- 按照添加的顺序排序，而不是按照key值大小排序
- map字典数据结构的性能优于object
- 无序不可重复的多个key-value的集合体

```
//第一种定义方法
{
  let map = new Map();
  let arr = ["123"];

   //添加元素用set, 获取arr的值用get
  map.set(arr, 456);
  console.log(map, map.get(arr)); 
  ////Map {["123"] => 456}    456 
}

//第二种定义方法
{
  //带参数的，两个数组结构，
  //参数：内方括号代表一个元素，内部为 key value  实际就是一个二维数组
  let map = new Map([["a", 123], ["b", 456]]);
  console.log(map); //Map(2) {"a" => 123, "b" => 456}
}

{
   //key为任意对象，还可为function
   const o = function(){
   		return '11'
   }
   map.set(o,4);
   console.log(map.get(o),map)//4  {"a" => 123, "b" => 456, ƒ => 4}
}
```

##### 常用API，基本同Set(多了可修改)

```
{
  let map = new Map([["a", 123], ["b", 456]]);
  //长度(统计)
  console.log(map.size); //2
  //增加
  console.log(map.set('c', 789));//Map(3) {"a" => 123, "b" => 456, "c" => 789}
  //set还可做修改
  console.log(map.set('c', ccc));//Map(3) {"a" => 123, "b" => 456, "c" => ccc}
  //获取
  map.get();
  //查找
  console.log('map-has',map.has('a'));//true
  console.log('map-get',map.get('a'));//123
  //删除
  console.log(map.delete("a"), map); //true Map(1) {"b" => 456}
  //清空
  console.log(map.clear());
  console.log(map); //Map(0) {}
}
```

##### Map 的遍历和 Set 一样

ps：**map的forEach的value、key刚好和本身的结构相反**

```
// map.keys()、map.values()、map.entries()、forEach()
{
	 let map = new Map([["a", 123], ["b", 456]]);
	 //遍历key
      for (let key of map.keys()) {
        console.log("keys", key);
        // keys a
        // keys b
      }
      
      //map的forEach的value、key刚好和本身的结构相反
      map.forEach((value,key) => {
      	console.log(value,key)
      })
      
      //entries遍历key、value 键值对
     for (let [key, value] of map.entries()) {
        console.log("entris", key, value);
     }

}
```

##### map的实例：利用map解构解决if-else书写不优雅

 https://mp.weixin.qq.com/s/0eX4FgJXMgCmZaKkZb9HVA 

#### weakSet 

阉割版set

- 和 Set 支持的数据类型不一样, 元素只能是对象，不能是数值、布尔值......

- 对象都是弱引用，不是值拷贝，而是地址引用。不会和垃圾回收机制挂钩（不会检测这个对象有没有在其他地方用过)，正因为这样, **WeakSet 对象是无法被枚举的**, 没有办法拿到它包含的所有元素. 

- 没有clear 方法，没有size属性，不能遍历

  ```
  {
    let weakList = new WeakSet();
    let arg = {};
    weakList.add(arg);
    //元素只能是对象
    weakList.add(2); //Invalid value used in weak set
    console.log(weakList); //WeakSet {{…}}
    console.log(weakList.size); //undefined
  }
  ```

#### weakMap 

阉割版map

- key 值 也必须是对象

- 同样的，同weakSet，没有 size 不能使用 clear，不能遍历

  ```
  {
    let weakmap = new WeakMap();
    let o = {};
    weakmap.set(o, 123);//设置kay，value值
    console.log(weakmap, weakmap.get(o)); //WeakMap {{...} => 123} 123
  }
  ```


### 补充：横向对比Map、Set与Array、Object等数据结构

#### 1.Map与Array的对比

```
{
  // 数据结构的横向对比 增、查、改、删
  let map = new Map();
  let array = [];

  //增
  map.set("t", 1);
  array.push({ t: 1 });//或者 array.unshift({ t: 1 });
  console.info("map-array-add", map, array);
  //Map(1) {"t" => 1}
  //[{t:1}]

  //查
  let map_exist = map.has("t"); //返回布尔值
  let array_exist = array.find(item => item.t); //返回当前对象，如果存在就返回这个值
  console.info("map-array", map_exist, array_exist);
  // true  {t:1}

  //改
  map.set("t", 2);
  //数组需要先遍历，去判断这个值是否存在，如果存在修改
  array.forEach(item => (item.t ? (item.t = 2) : ""));
  console.info("map-array-modify", map, array);
  //Map(1) {"t" => 2}
  //[{t:2}]

  //删
  map.delete("t");
  let index = array.findIndex(item => item.t);
  array.splice(index, 1);
  console.info("map-array-empty", map, array);
  //Map(0) {}
  //[]
}
```



#### 2.Set与Array的对比

```
{
  let set = new Set();
  let array = [];
  //增
  set.add({ t: 1 });
  array.push({ t: 1 });
  console.info("set-array", set, array);
  // Set(1) {{t:1}}
  // [{t:1}]

  //查  
  let set_exist = set.has({ t: 1 }); 
  //对象不是值拷贝，而是地址引用,
  //这里的has和前面的set.add({ t: 1 })指向的不是同一个地址 所以set_exist为false
  let test = { t1: 1 };
  set.add(test);
  console.info("set-find",set, set.has(test));//这里为true 用变量保存对象后,add和set指向同一个地址
  let array_exist = array.find(item => item.t); //返回这个值
  console.info("set-array-find", set_exist, array_exist);
  //false {t:1}

  //改
	set.forEach(item => (item.t ? (item.t = 2) : ""));
	array.forEach(item => (item.t ? (item.t = 2) : ""));
	console.log("set-array-modify", set, array);
  // Set(1) {{t:2}}
  // [{t:2}]

    //删
	set.forEach(item => (item.t ? set.delete(item) : ""));
	let index = array.findIndex(item => item.t);
	array.splice(index, 1);
    console.info("set-array-empty", set, array);
  //Set(0) {}
  //[]
}
```

#### 3.Set、Map与Object的对比

```
{
  //map set 与 object对比
  let item = { t: 1 };
  let map = new Map();
  let set = new Set();
  let obj = {};

  //增
  map.set("t", 1);
  set.add(item);
  obj["t"] = 1;
  console.log("map-set-obj", map, set, obj);
  // Map(1) {"t" => 1}   Set(1) {{t:1}}   {t: 1}

  //查 set和map语义化更好一些，obj语义化不如map和set
  console.info({
    map_exist: map.has("t"),
    set_exist: set.has(item),
    obj_exist: "t" in obj
  });
  //{map_exist: true, set_exist: true, obj_exist: true}

  //改
	map.set("t", 2);
	item.t = 2; //如果存储了数据，所以修改数据本身，如果没有存储需要遍历
	obj["t"] = 2;
	console.log("map-set-obj-modify", map, set, obj);
  //Map(1) {"t" => 2}   Set(1) {{t: 2}}  {t: 2}

  //删
	map.delete("t");
	set.delete(item);//如果没有存储仍然要遍历数据再删除
	delete obj["t"];
	console.log("map-set-obj-empty", map, set, obj);	
  //Map(0) {}   Set(0) {}  Object {}   
}

```

#### 4.总结

能使用map尽量不要使用数组===》优先使用map，放弃数组

有数据结果存储唯一性应考虑 Set ===》对数据要求比较高，要求唯一性，放弃object



### 十二.Proxy(代理)与Reflect(反射)

- 连接用户和原始对象的层
- 为操作Object提供新的Api
- 二者方法是一样的

#### Proxy代理对象

proxy作用:**屏蔽原始数据，进行代理**，有点像中介
new Proxy(target(可为空)，handler(类似json对象))
get 方法 访问数据，三个参数，需要return 
set、has、deleteProperty、apply（调用函数处理）

##### Proxy API

```
{
	//1.创建类似供应商的对象--提供原始数据
	 let obj = {
	    time: "2018-07-08",
	    name: "net",
	    _r: 123
	  };
	  
	//2.通过代理商 Proxy新创建对象 花括号里是代理的东西
	let monitor = new Proxy(obj,{})//代表什么也没操作
	
	//以下是代理操作，比原始如对象里有个价格属性，中介也就是proxy要把价格增加，就是一种代理操作
	let monitor = new Proxy(obj,{
		//拦截 或者说代理对象属性的读取 target是代理的真实对象
		get(target,key){
			//get方法用于拦截某个属性的读取操作，可以接受三个参数，
			//依次为目标对象、属性名和 proxy 实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选
			return target[key].replace("2018", "2019");
		},
		
		//拦截对象属性的设置
		set(target,key,value){
			//set方法用来拦截某个属性的赋值操作，可以接受四个参数
			//依次为目标对象obj、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选
			if(key === 'name'){
				return target[key] = value;
			}else if(key === 'price'){
				//涨价
				return target[key] + 20;
			}
		},
		
		//has方法用来拦截HasProperty操作，即判断对象是否具有某个属性
		//has方法可以接受两个参数，分别是目标对象、需查询的属性名
		has(target,key){
			if(key === 'name'){
				return target[key];
			}else{
				return false;
			}
		},
		
		 //拦截对象 删除属性
	    deleteProperty(target, key) {
	      //以下划线开头的就删除
	      if (key.indexOf("_") > -1) {
	        delete target[key];
	        return true;
	      } else {
	      	//如果不是直接返回当前值
	        return target[key];
	      }
	    },
		
		//关于遍历
		//拦截Object.keys, Object.getOwnPropertySymbols, Object.getOwnPropertyNames, for...in循环
		ownKeys(target) {
	      //保护 time 属性
	      return Object.keys(target).filter(item => item != "time");
	    }
	});
	
	//3.对于用户来说，就是直接操作monitor对象
	console.log("get", monitor.time);//get 2019-07-08
	monitor.time = '2017';
	console.log("set", monitor.time);//set 2019-07-08
	//修改name属性值才有改变
	monitor.name = 'proxysetlearn';
	console.log("set", monitor);//set Proxy {time: "2018-07-08", name: "proxysetlearn", _r: 123}
	//通过代理欺骗了用户 time不在属性中
	//ps！！！has拦截只对in运算符生效，对for...in循环不生效
	console.log("has", 'name' in monitor,'time' in monitor);//has true false
	
	delete monitor.time;
	console.log("delete", monitor);//delete Proxy {time: "2018-07-08", name: "proxysetlearn", _r: 123}
	//delete monitor._r;
	//console.log("delete", monitor);//delete Proxy {time: "2018-07-08", name: "proxysetlearn"}
	//过滤掉time属性
	console.log("ownKeys", Object.keys(monitor));// ["name", "_r"]
}
```

##### ownKeys()补充

有三类属性会被ownKeys方法自动过滤，不会返回

- 目标对象上不存在的属性
- 属性名为 Symbol 值
- 不可遍历（enumerable）的属性

`ownKeys`方法返回的数组成员，只能是字符串或 Symbol 值。如果有其他类型的值，或者返回的根本不是数组，就会报错 

##### proxy使用场景

1.数据拦截，只可读，用户不可修改原数据（保护原始数据）

```
//从后端拿到的数据对象还原 比如排序后要还原
let o = {
	name: 'xxx',
	age: 18
}
let monitor = new Proxy(o,{
	get(target,key){
		return target[key];
    },
    set(target,key,value){
    	//拦截赋值操作 不让修改
		return false;
    },
})
monitor.age = 20; //set中的value变为20，但是并未赋值成功，o对象并未修改
console.log(monitor.name,monitor.age) //xxx 18

//es5 实现
for(let [key] of Object.entries(o)){
	Object.defineProperty(o,key,{
		//属性描述符
		writable: false //被锁死，用户无法操作，但是同时自己也无法操作
		//推荐代理：自己可操作，拦截用户
	}
}
```

2.数据校验，降低耦合，更加优雅

```
{
const n = 1.0000
let a = typeof(n.toFixed(2))
	//验证数据：比如格式等
	//拦截无效数据
	//数据结构不被破坏
	let o = {
        name: 'xxx',
        price: 190
    }
    let monitor = new Proxy(o,{
        get(target,key){
            return target[key] || '';//避免undefined
        },
        set(target,key,value){
            //判断target上有无此key
            if(Reflect.has(target,key)){
            	if(key === 'price'){
            		if(value>300){
            		   return false;
            		}else{
            		   value = target[key];
            		}
            	}else{
            		value = target[key];
            	}
            }else{
            	return false;
            }
        },
    })
    monitor.price = 280;
    console.log(monitor.price)//280
    monitor.price = 301;
    console.log(monitor.price)//190  数据不改变
    monitor.age = 18; //空 赋值失败，新值（数据结构不被破坏）
}

//解耦写法
let validater = (target,key,value) = > {
	if(Reflect.has(target,key)){
        if(key === 'price'){
            if(value>300){
                return false;
            }else{
                value = target[key];
            }
        }else{
            value = target[key];
        }
     }else{
        return false;
     }
}

 let monitor = new Proxy(o,{
        get(target,key){
            return target[key] || '';//避免undefined
        },
        set: validater
 })
```

3.验证结合监控上报

```
//更加解耦完成上报机制
window.addEventListener('error',(e) => {
	console.log(e.message);
	report(./)//上报逻辑
//捕获错误true
},true)

let validater = (target,key,value) = > {
	if(Reflect.has(target,key)){
        if(key === 'price'){
            if(value>300){
            	//不满足规则触发错误
                throw new TypeError('price exceed 300');
            }else{
                value = target[key];
            }
        }else{
            value = target[key];
        }
     }else{
        return false;
     }
}
```

4.组件id只读且唯一

```
//识别组件，id且唯一
class Component(){
	//构造函数
	constructor(){
		//随机id  换成字符串 36进制，截取后8位
		this.id = Math.random().toString(36).slice(-8);
	}
	//只读属性
	get id(){
		//随机id  换成字符串 36进制，截取后8位
		return Math.random().toString(36).slice(-8);
	}
	//修改id时发现上面两种写法 又重新生成新的随机id，换成下面这个写法
	constructor(){
		this.proxy = new Proxy({
			id: Math.random().toString(36).slice(-8);
		},{})
	}
	get id(){
		return this.proxy.id
	}
	
}
let com = new Component()
let com2 = new Component()
for(let i=0; i<10; i++){
	console.log(com.id,com2.id)
}
com.id = 'abc';
console.log(com.id,com2.id)
```

#####  Proxy.*revocable* 

可以用来创建一个可撤销的代理对象，临时代理场景！！！
可撤销代理相当于把房子租给中介后，撤销合同，代理失效

```
{
    let o = {
        name: 'xxx',
        age: 18
    }
    //可撤销 两部分内容，包含代理数据和撤销操作：proxy revoke
    let monitor = Proxy.revocable(o,{
        get(target,key){
            return target[key];
        },
        set(target,key,value){
            return false;
        },
    })
    console.log(monitor.proxy.name,monitor) //两部分proxy revoke
    //场景有点像阅后即焚的既视感
	setTimeout(() => {
		monitor.revoke();
		console.log(monitor.proxy.name)//无法读取
	},1000)
}
```


##### es5补充：Object.defineProperty() 

数据劫持-----知道数据的变化

<http://es6.ruanyifeng.com/#docs/proxy> 

```
//创建对象
/*let vm = {
	name: 'zhangsan'
}*/
以上声明用defineProperty实现，实现可控数据
let vm = Object.defineProperty({},"name",{
    get(){
        console.log(get);
        return 'zhangsan'
    },
    set(newValue){
        console.log('新的值'，newValue);
    }
})
//修改对象
let vm = {
    name: 'zhangsan',
    age: 18
};
//获取key 数组
let keys = Object.keys(vm);
keys.forEach(key => {
    let value = vm[key];
    Object.defineProperty(vm,key,{
    	configurable：true，//可配置 比如你要delete vm.xx
    	enumerable: true,//比如你要用for..in Object.keys()等等
        get(){
            console.log(get);
            return value;
        },
        set(newValue){
            console.log('新的值'，newValue);
            if(newValue!==value)
            value = newValue;
        }
    })
})

//以上defineProperty改写成proxy
let newVm = new Proxy(vm,{
       get(target,key){
            console.log(get);
            return target[key];
        },
        set(target,key,newValue){
            console.log('新的值'，newValue);
            if(target[key]!==newValue)
            target[key] = newValue;
        }
})

//问题1，vue2.X里数组改变不能触发视图更新 因为defineProperty劫持不到，可以用$set
//问题2 vue2.X当属性的值为一对象时，直接添加对象中属性的值时也无法触发set，defineProperty同样劫持不到，可以用$set
https://www.cnblogs.com/mengff/p/8482867.html
https://www.jianshu.com/p/7b138b71e6fd
```

#### Reflect反射对象

Reflect在编译阶段不知道被哪个类加载，在运行的时候加载、执行，才知道被哪个类加载，不用new，直接用


```
{
	  let obj = {
	    time: "2018-07-08",
	    name: "net",
	    _r: 123
	  };
	
	  console.log("Reflect get", Reflect.get(obj, "time")); //2018-07-08
	  Reflect.set(obj, "name", "ReflectsetLearn");
	  console.log(obj); //{time: "2018-07-08", name: "ReflectsetLearn", _r: 123}
	  console.log(Reflect.has(obj, "name")); //true
}
```

##### Reflect.apply

`Reflect.apply`(func, thisArg, args) 方法等同于`Function.prototype.apply.call(func, thisArg, args)`，用于绑定`this`对象后执行给定函数。

一般来说，如果要绑定一个函数的`this`对象，可以这样写`fn.apply(obj, args)`，但是如果函数定义了自己的`apply`方法，就只能写成`Function.prototype.apply.call(fn, obj, args)`，采用`Reflect`对象可以简化这种操作

```
//js中 使用apply方法时必须知道是被哪个函数所调用
//反射机制中，先调用apply，再去指定哪个函数

//实例 向下取整 null代表作用域 这里暂时没有
Math.floor.apply(null,[3.72])
Reflect.apply(Math.floor,null,[3.72]) //4

//实例 反射 3元表达式
Reflect.apply(price>100 : Math.floor, Math.cell, null, [3.72])
```

##### Reflect.construct

之前的做法如果实例化一个类要用new，new不同的类，反射机制中construct(target, argumentsList[, newTarget])是构造函数的意思

```
let d = new Date();
console.log(d.getTime());

//Reflect.construct可以做到不用new,
let d = Reflect.construct(Date,[]);
console.log(d instanceof Date);//与new没有什么区别
```

##### Reflect.defineProperty

定义对象(新增一个属性)

```
//es5
const student = {}
let test1 = Object.defineProperty(student,'name',{ value:'mike' })
//es6
let test2 = Reflect.defineProperty(student,'name',{ value:'mike' })
//区别：返回值不同
console.log(student,test2)//{name: "mike"} true 而test1返回的是一个对象
```

##### Reflect.deleteProperty

删除一个属性

```
//同样是obj.deleteProperty()的迁移,区别同样是返回值不同
const obj = {x:1, y:2}
Reflect.deleteProperty(obj,'x');
console.log(obj)//{y:2}
```

##### Reflect.get

读取数据

```
const obj = {x:1, y:2};
console.log(Reflect.get(obj,'x'))//1
//读取数组更方便
console.log(Reflect.get([3,4],1))//3  读取第一个
```

##### Reflect.ownKeys

返回所有keys

#### 开发中的实例

##### es5补充：`hasOwnProperty()`

`hasOwnProperty()` 返回一个布尔值,判断对象是否包含特定的自身(非继承)属性 

```
{
	/*1.实现 访问一个对象身上属性，默认不存在的时候给undefined
	 希望不存在错误信息*/
	let obj = {
		name: 'aaa'
	}
	let newObj = new Proxy(obj,{
		get(target,key){
			if(key in target){
				return target[key];
			}else{
				console.warn(`${key}属性不在对象上`)
			}
		}
	})
	console.log(newObj.age)
}


{	
	//2.实现数据类型校验模块，和业务解耦
	fuction validator(target,validator){
		let proxy1 = {
			_validator:validator,
			set(target,key,value,proxy){
				//key in target
				if(target.hasOwnProperty(key)){
					console.log(this._validator)//构造函数
					//把条件拿出
					let va = this._validator[key];
					/*va打印是这种形式
					 * name(val) {
				      return typeof val === 'string';
				    }*/
				   console.log(va(value))//false
					if(!!va(value)){
						//如果value不存在
						return Reflect.set(target,key,value,proxy)
					}else{
						throw Error(`不能设置${key}到${value}`)
					}
				}else{
					//如果没有就返回错误
					throw Error(`${key}不存在`)
				}
			}
		};
		return new Proxy(target,proxy1)
	};
	//设置过滤条件
	const personValidator = {
		name(val){
			//名字是否是字符串类型
			return typeof val === 'string'
		},
		age(val){
			//返回布尔值
			return typeof val === 'number' && val > 18
		}
	}
	//console.log(personValidator['age'](19))//true
	//构造函数
	class Person{
		constructor(name,age){
			this.name = name;
			this.age = age;
			console.log(this)//Person {name: "lilei", age: 30}
			//代理/拦截Person对象 this==》target
			return validator(this,personValidator)
		}
	}
	//实例化
	const person = new Person('lilei',30);
	console.log(person)//Proxy {name: "lilei", age: 30}
	
	person.name = 'tanh';
	console.log(person);//Proxy {name: "tanh", age: 30}
	
	//对属性修改，被拦截，对属性的修改有了限制
	person.name = 48;//Uncaught Error: 不能设置name到48
}
```

##### 总结

通过代理把条件和对象本身（业务逻辑）完全隔离开，比如你还要加mobile、id等条件，后续代码的复壮性、维护会方便很多

### 十三.类与对象

```
// es5如何实现类的定义/声明---用函数去模拟
// 构造函数2个作用 1.传参数 2.初始化
let Animal = function (type) { 
    this.type = type 
    this.walk = function () {
    	console.log(`I am walking`) 
    }
}
let dog = new Animal('dog') 
let monkey = new Animal('monkey')

monkey.eat = function(){
	console.log(`error`) //只修改了monkey的 没有修改到dog的eat方法
}
// 弊端：每个实例对象都有type属性和walk方法，如果想修改eat的方法，违背了继承的原则，而且生成的对象很大


// 改进
let Animal = function (type) { 
	this.type = type 
}
//把walk写到原型链上（类继承的原理）
Animal.prototype.walk = function () { 
	console.log(`I am walking`) 
}
//修改原型链的eat方法
monkey.constructor.prototype.eat = function(){
	Animal.walk();
	this.walk(); //会报错 this指的是实例对象，静态方法不是挂在实例对象上
	console.log(`error`) //只修改了monkey的 没有修改到dog的eat方法
}
let dog = new Animal('dog') 
let monkey = new Animal('monkey')

//es5的静态方法（挂载到类而不是实例对象）
//定义
Animal.walk = function(){
	console.log(`I am walking`) 
}
//通过调用实例对象的eat方法去调用静态方法walk
dog.eat();
dog.walk(); //is not a function 没找到 实例对象上没有walk方法

//es6的方法
class Animal { 
    constructor (type) { 
        this.type = type 
    }
    walk () { 
    	console.log(`I am walking`)
    } 
}
let dog = new Animal('dog') 
let monkey = new Animal('monkey')
console.log(typeof Animal); //function 说明class实际就是构造函数的语法糖
```

#### 基本定义及生成实例

```
{
	class Parent{
		//定义构造函数
		constructor(name = "muke") {
	      this.name = name;
	    }
		tell() {
	      console.log("tell");
	    }
	}
	
	//生成实例
	let v_parnet = new Parent("v");
  	console.log(v_parnet);//Parent {name: "v"}
  	v_parnet.tell();//tell
}
```

Parent.prototype.constructor === Parent //true

v_parnet._________proto_________ === Parent.prototype

typeof Parent //"function"

由此可见，es6的类实际是function的语法糖

#### 如何读写属性

#####  getter和setter

```
// es6如何做到属性的保护和只读
// getter和setter + functuon就变成了类的属性

{
	let _age = 4;
	
	class Parent{
		//定义构造函数
		constructor(name = "muke") {
	      this.name = name;
	    }
		//注意！get是属性并不是方法！！！
		// get 只读
		get longName(){
			return 'mk'+this.name;
		}
		get age(){
			//age 是属性，是暴露出的这个类的属性，通过它操作私有属性_age,也就是闭包的方法
			return _age;
		}
		set age(val){
			//set/get可做拦截
			if( val > 2 ){
				_age = val;
			}
		}
		
		set longName(value){
			this.name = value;
			
		}
	}
	let v = new Parent();
    console.log("getter", v.longName); //mkmuke
    
    // 赋值就等于 set 操作
	v.longName = "hello";
	console.log("setter", v.longName); //mkhello
	
	//私有属性_age
	console.log("getter 私有", v._age); //undefined 肯定是直接读取不到_age属性的
}
```

#### 静态方法和属性

静态方法是通过类去调用，而不是通过类的实例去调用

```
{
	// 静态方法 与 静态属性
	class Parent{
		//定义构造函数
		constructor(name = "muke") {
	      this.name = name;
	    }
		//静态方法是通过类去调用，而不是通过类的实例去调用
		//static只是用来定义静态方法，并不能定义静态属性
	    static tell() {
	      console.log("tell");
	    }
	    // 非静态方法
	    talk() {
	      console.log("talk");
	    }
	}
	Parent.tell(); //tell
	// 在类上直接定义静态属性
  	Parent.type = "test";
  	console.log("静态属性", Parent.type); //静态属性 test
}
```

##### 总结

1.es6 用static识别静态方法

2.区分何时用静态方法：方法内部要引用实例对象的内容时就要使用实例对象的方法，否则就要用类的静态方法，因为类的静态方法拿不到当前类的实例对象

#### 如何继承一个类

```
//es5如何做继承  详细见js基础深入.md
function Parent1(){
	this.name = "parent1";
}

function Child(){
    // 同时修改了父级构造函数this的指向，从而导致了父类执行的时候属性都会挂在Child类实例上去。
    Parent1.call(this);
    this.type="child1";
}
//why加这步骤：只实现了部分继承，如果父类的属性都在构造函数上，那没有问题；如果父类的原型对象上还有方法（原型链
//的方法），子类是拿不到这些方法的
Child.prototype = Parent.prototype;//引用类型 共享一块内存 ---》改变原型链指向
console.log(new Child());//Child {name: "parent1", type: "child1"}
```

##### super

super 指向父类，相当于 A.prototype.constructor.call(this)

constructor可以忽略不写，因为子类extends后有默认值，但是如果你想添加子类自己的属性或方法，必须显示调用constructor，且super方法必须位于构造器内第一行

```
{
	//继承传递参数，也就是子类覆盖父类
	class Parent{
		//定义构造函数
		constructor(name = "muke") {
	      this.name = name;
	    };
	    a(){
            console.log('aa');
	    }
	}
	
	class Child extends Parent{
		//constructor可以忽略不写，因为子类extends后有默认值，但是如果你想添加子类自己的属性或方法，必须显示调		// 用constructor，且super方法必须位于构造器内第一行
		constructor(name = "child") {
	      //如果super参数为空，则会使用所有父类的默认值此外，super必须放在子类的第一行
	      //调用super方法，得到子类的this对象
	      super(name);
	      
	      //如果子类还要添加自己的属性，则this必须放在super后面
	      this.type = 'child';
	    }
	    //父类的方法重写
	    a(){
            console.log('bb');
	    }
	}
	let p1 = new Child('111')
	console.log(p1)//_Child {name: "111", type: "child"}
	p1.a();//bb
	console.log('继承',new Child()); //继承 _Child {name: "child", type: "child"}
	console.log('继承',new Child('hello')); //继承 _Child {name: "hello", type: "child"} 参数覆盖
}
```

#### 模拟vue视图更新渲染

```
class Vue extends EventTarget{
	constructor(options){
	    super();
        this.options = options;
        this.compile();
        this.observe(this,options.data);
	}
	//自定义事件 ---类似 创建发布者订阅
	//自定义事件需要继承EventTarget
	//劫持数据
	observer(data){
        let keys = Object.keys(data);
        keys.forEach(key => {
            this.defineRect(data,key,data[key])
        })
	}
	defineRect(data,key,value){
	    let _this = this;
        Object.defineProperty(vm,key,{
            configurable：true，//可配置 比如你要delete vm.xx
            enumerable: true,//比如你要用for..in Object.keys()等等
            get(){
                console.log(get);
                return value;
            },
            set(newValue){
                console.log('新的值'，newValue);
                //创建事件 CustomEvent方便传参
                let event = new CustomEvent(key，{
                    detail: newValue
                });
                //触发事件
                this.dispatchEvent(event)
                value = newValue;
            }
        })
	}
	//把数据渲染到页面上
	compile(){
		//找到根节点
        let ele = document.querySelector(this.options.el);
        //范围内所有的节点
        let childNodes = ele.childNodes;
        this.compileNode(childNodes);
	}
	//编译
	compileNode(childNodes){
        [...childNodes].forEach(node => {
            if(node.nodeType == 1){
            	if(node.childNodes.Length>0){
            		//如果节点下有子节点 递归
                    this.compileNode((node.childNodes);
            	}
                //元素节点
            }else if(node.nodeType == 3){
            	//文本节点
            	let nodeText = node.textContent;
            	let reg = /\{\{\s*{\S+)\s*\}\}/g;
            	let test = reg.test(nodeText);
            	if(test){
                    let $1 = RegExp.$1;
                    console.log(this.options.data[$1]);
                    
                    //多层
                    let arr = $1.split('.');
                    let str = this.options.data;
                    arr.forEach(v => {
                        str = str[v];
                    })
                    node.textContent = str;
                    //替换
                    //node.textContent = this.options.data[$1];
                    //绑定更新视图事件
                    this.addEventListener($1,e => {
                        console.log('触发更新了')；
                        node.textContent = e.detail;
                    })
            	}
            }
        })
	}
}
```

### 十四.Promise-异步编程的解决方案

#### promise概念

Promise是一个构造函数，用来生成promise实例，有三种状态

- pending：初始化状态
- fullfilled：成功状态
- rejected：失败状态
- 状态变化不可逆

#### 什么是异步

“同步" 就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；

"异步"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数（或者说你想要执行的函数），后一个任务也不必等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。 

异步A执行完去执行B，

传统方式：回调，事件触发 ，promise区别于这两种方式

补充知识

同步或非同步，表明着是否需要将整个流程按顺序地完成

阻塞或非阻塞，意味着你调用的函数会不会**立刻**告诉你结果，继不继续向后执行

关于异步编程的方式，常用的**定时器、ajax、Promise、Generator、async/await(es7/8)**

#### 传统方法与promise对比

缺点：如果回调函数很多，写法复杂，并且影响后期的维护，因为很难看出回调之间的顺序问题，难以阅读

```
{
  //ES5中回调
  let ajax = function(callback) {
    console.log("执行");
    setTimeout(() => {
    	//先判断回调是否存在,call调用函数
      callback && callback.call();
    }, 1000);
  };
  //调用ajax函数
  ajax(function() {
    console.log("timeout1");
    //执行
    //timeout1
  });
}
```

用promise改写，避免回掉地狱，promise有3种状态，new promise函数后，首先会挂起pending，promise状态result并没有值是undefined，如果resolve被执行，promise状态被改变，就是 *full*filled ，如果执行reject，就是 *rejected* ，并且两种状态任意改变后都不逆，只能返回一种状态

```
{
  let ajax = function() {
    console.log("执行2");
    //返回一个promise实例，这个对象才有一个then方法  
    //reject是ajax封装方法的第二个参数
    return new Promise(function(resolve, reject) {
     	setTimeout(() => {
     		//通过resolve传递参数	
	    	resolve('test');
	    }, 1000);
    });
  };
  //用then
  ajax().then(function(data){
  	console.log("timeout2");
    //成功的状态输出	
  	console.log(data);
    //执行2
    //timeout2
  }, ()=> {
     //失败的回调 
  })
}

/*4个顺序
执行
执行2
timeout1
timeout2
*/
```

- 连续调用


```
{
  let ajax = function() {
    console.log("执行3");
    //返回一个promise实例，这个对象有一个then方法
    return new Promise(function(resolve, reject) {
     	setTimeout(() => {
	    	resolve();
	    }, 1000);
    });
  };
  
  ajax().then(function(){
  	console.log("执行33");
    return new Promise(function(resolve, reject) {
     	setTimeout(() => {
	    	resolve();
	    }, 2000);
    });
  })
  	.then(function() {
      console.log("timeout3");
  });
}
 //执行3
 //执行33
 //timeout3
```

##### 小tips

**Promise的函数代码的异步任务会优先于setTimeout的延时为0的任务先执行**

#### 异步操作Then

1.链式调用 只有你return了一个Promise对象才有then，才可以不断的链式调用

2.promise.then(onFullfilled,onReject),then里写函数（then支持两个参数，都是函数）
.then返回正常的promise对象，如果你传的不是函数，.then会忽略onFullfilled和onReject，返回一个空的promise对象

```
test()
    .then(()=>{
      console.log("success1");
      要return 一个promise对象; 不能直接调用test()
    },(err)=>{
      console.log("fail1");
    })
    .then(()=>{
      console.log("success2");
    },(err)=>{
      console.log("fail2");
    })
 //第一次then执行后，又返回一个promise对象，再次执行.then方法
 
 //这种方式如果执行reject后因为状态不可逆，就无法执行then的第一个参数方法，同理，如果执行成功后，就无法执行err方法,但是，执行err后，.then会返回一个空的promise对象，test又进行到下一个.then方法
 
 //如果第一次then执行了onReject函数，onReject里的逻辑调用promise的reject，按道理，第二次then应该直接进入到err方法里执行，但是如果第一次then里没有return返回值（promise对象），第二次会调用then的onFullfilled函数
 
 //如果then无法直接返回一个promise异步操作，可以调用promise的静态方法（用promise类来调用）生成promise实例，Promise.resolve(xx)或者Promise.reject()
 
```

#### 捕获异常错误catch

更优雅的方法处理错误,.catch写到then最后，catch是promise对象的原型方法
语法错误Error和逻辑错误reject都可以通过catch来捕获，所以promise最好只传一个成功的回调就好了，失败的进入catch来捕获

```
{
	let ajax = function(num) {
	    console.log("执行4");
	    //返回一个promise实例
	    return new Promise(function(resolve, reject) {
	      if (num > 5) {
	        resolve();
	      } else {
	        throw new Error("出错");//实际情况写reject来改变promise的状态，不能写throw Error，catch捕获状                                     //态改变时的错误·
	      }
	    });
    };
    //使用
    ajax(6)
    .then(function() {
      console.log("log", 4);
      return ajax(7);
    })
    .then(function() {
      console.log("log", 4);
      return ajax(4);
    })
    .catch(function(err) {
      console.log("catch", err); // catch Error: 出错
    });
}
```

统一捕获异常，放最后

```
{
	function loading(src){
        return new Promise((resolve,reject)=>{
        	//首先创建一个img标签
        	let img = document.createElement("img");
            //监听图片加载完成
            img.onload = () => {
            	resolve(img);
            }
            //onerror 事件在加载外部文件（文档或图像）发生错误时触发
            img.onerror = (err) => {
            	reject(err)
            }
            img.src = src
         })
     }

        var src = 'xxx.jpg';
        var result = loading(src);
        result.then((img) => {
        	console.log(1,img.width);
        	return img //如果return不是一个Promise实例对象那么返回的就是这个本身promise实例，.then()即是在本身实例上继续链式调用
        }.then((img) => {
        	console.log(2,img.height);
        }).catch((error) => {
        	console.log(3,error);
        })
}
```

#### 实例-封装原生ajax(promise串联)

```
比如应用到先获取内容再获取内容下相关的评论
{
	//获取新闻功能函数	
	function getNews(url){
        return new Promise((resolve,reject) => {
        	//1创建xmlHttp实例对象
            let xmlHttp = new XMLHttpRequest();
            //2绑定监听 readyState
            //为什么不写xmlHttp.readyState === 4 && xmlHttp.status === 200，因为readyState4种状态，执行4次，当他初始化第一次等于1的时候，就直接进去else去reject了，显然不符合需求
            if(xmlHttp.readyState === 4){
            	if(xmlHttp.status === 200){
            		//参数在resolve中传递	
                    resolve(xmlHttp.responseText);
            	}else{
                    reject('暂时没有内容');
            	}
            }
            //3open 设置请求的方式以及url open可选第3个参数代表同步还是异步，async默认是true, 即为异步方式,如果写xmlHttp.open('GET', url, false)则是同步
            xmlHttp.open('GET', url);
            //4发送
            xmlHttp.send();
        })
	}
	//调用函数 每条新闻内容都有自己的id关联相关的评论
	getNews('xxxurl?id=？')
		.then((data) => {
			console.log(data)；
			//原生这里的data是string类型，要转换
			//发送请求获取评论内容
            let commentUrl = JSON.parse(data).commenturl;
            //拼接链接
            let url = 'http://xxx' + commentUrl; 
            //再次发送评论的请求
            return getNews(url);
		}, (error) => {
            console.log(error)；
		})
		//链式调用 只有你return了一个new Promise对象才有then，才可以不断的链式调用
		.then((data) => {
			console.log(data)；
		}, (error) => {
            console.log(error)；
		})
}
```

#### promise高级用法

以下的Promise为大写，表示window里的Promise变量

##### Promise.all

Promise.all实际就是一种并行，待全部完成后，统一执行success，成功的时候返回的是一个结果数组，而失败的时候则返回最先被reject失败状态的值 

```
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.resolve(3);

Promise.all([
	p1,p2,p3
]).then((value)=>{
	console.log(value); //1 2 3
})


```

###### **使用场景**

所有图片加载完成再添加到页面，用户看不到图片加载的过程

```
{
	//所有图片加载完成再加载页面
	function loading(src){
		return new Promise((resolve,reject)=>{
			//首先创建一个img标签
			let img = document.createElement("img");
			img.src = src;
			//监听图片加载完成
			img.onload = function(){
				resolve(img);
			}
			//onerror 事件在加载外部文件（文档或图像）发生错误时触发
			img.onerror = function(err){
				reject(err)
			}
		})
	}
	
	// 把多个Promise实例当作一个实例
	Promise.all([
	    loading('http://chuantu.xyz/t6/702/1570366675x992245926.jpg'),
        loading('http://chuantu.xyz/t6/702/1570366675x992245926.jpg'),
        loading('http://chuantu.xyz/t6/702/1570366675x992245926.jpg')
	]).then(showImgs)
}
```

**需要特别注意的是，Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的**，**即p1的结果始终在前，即便p1的结果获取的比p2要晚。这带来了一个绝大的好处：在前端开发请求数据的过程中，偶尔会遇到发送多个请求并根据请求顺序获取和使用数据的场景** 

```
let wake = (time) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(`${time / 1000}秒后醒来`)
    }, time)
  })
}

let p1 = wake(3000)
let p2 = wake(2000)

Promise.all([p1, p2]).then((result) => {
  console.log(result,result[0],result[1])       // [ '3秒后醒来', '2秒后醒来' ]
}).catch((error) => {
  console.log(error)
})
```

##### **Promise.race**

**Promse.race先到先得，就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态** 

###### **使用场景**

有一个图片加载完成就添加到页面

```
{
	//所有图片加载完成再加载页面
	function loading(src){
		return new Promise((resolve,reject)=>{
			//首先创建一个img标签
			let img = document.createElement("img");
			img.src = src;
			//监听图片加载完成
			img.onload = function(){
				resolve(img);
			}
			//onerror 事件在加载外部文件（文档或图像）发生错误时触发
			img.onerror = function(err){
				reject(err)
			}
		})
	}
	//添加到页面
	function showImgs(img){
		let p = document.createElement('p')
		p.appendChild(img)
        document.body.appendChild(p);
	}
	
	// 把多个Promise实例当作一个实例
	Promise.race([
	    loading('http://chuantu.xyz/t6/702/1570367039x992245926.jpg'),
        loading('http://chuantu.xyz/t6/702/1570366675x992245926.jpg'),
        loading('http://chuantu.xyz/t6/702/1570367039x992245926.jpg')
	]).then(showImgs)
}
```



### 十五.iterator(接口) 和 for...of 循环

#### 什么是iterator 接口

不同的数据结构通过 for...of 统一的这种形式来达到读取不同数据结构的目标

但是背后的iterator接口是不一样的

- 也就是说只有部署了iterator接口，才能被 for...of遍历
- 或者说任何数据结构只要部署 Iterator 接口， 就可以完成遍历操作 

#### iterator 遍历过程

Iterator 的遍历过程是这样的。

（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

#### 模拟指针对象/遍历器对象(底层原理)-闭包写法

```
{
	//iterator接口 模拟指针对象
	//这是一个闭包写法，内部函数调用了外部函数的变量index，
	//并且直接调用了内部函数next(通过return给外部函数)
    function myIterator(arr){
        let index = 0;//记录指针的位置
        let len = arr.length;//长度 用来判断done
	    return {
             next(){
                    // 三目表达式 如果index < len说明没有遍历完
                    return index < len ? {value: arr[index++], done: false} : 
                    {value: undefined, done: true}
                  }
            }
    }
    let arr = [1,4,65,'abc'];
    let iteratorObj = myIterator(arr);
    //指针随着next方法可以向下平移了
    console.log(iteratorObj.next())
    console.log(iteratorObj.next())
    console.log(iteratorObj.next())
    console.log(iteratorObj.next())
    console.log(iteratorObj.next())
    /*
     {value: 1, done: false}
     {value: 4, done: false}
     {value: 65, done: false}
     {value: "abc", done: false}
     {value: undefined, done: true}
     */
}
```

#### iterator 基本用法

- 将iterator接口部署到指定的数据类型上：数组、Set 和 Map 容器、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串，可以使用for...of遍历
- arguments可以用for...of却不可以用forEach，伪数组没有数组的方法哦！
- obj is not iterable！！！对象可以用for....in
- `for...of`循环内部调用的是数据结构的`Symbol.iterator`遍历器方法

```
{
  //循环数组  数组有定义好的iterator接口，因为结构都是固定的
  let arr = ["hello", "world"];
  //调用，返回map，map有一个方法next
  //Symbol.iterator属性本身是一个函数，它是一个表达式，返回Symbol对象的iterator属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内
  let map = arr[Symbol.iterator]();
  console.log(map.next()); //{value: "hello", done: false}
  console.log(map.next()); //{value: "world", done: false}
  console.log(map.next()); //{value: undefined, done: true}
  
  for (let i of arr) {
    console.log(i); //hello world
  }
}

{
  //因为obj没有部署iterator接口，所以要自定义iterator 接口，然后使其可以使用for...of循环
  let obj = {
    start: [1, 3, 2],
    end: [7, 9, 8],
    [Symbol.iterator]() {
      //当前对象
      let self = this;
      //当前索引
      let index = 0;
      //合并成一个数组
      let arr = self.start.concat(self.end);
      //长度 用来判断done
      let len = arr.length;
      //返回一个对象
      return {
        next() {
        	//当前索引小于数组长度，返回当前值，值有两部分：value和done
          if (index < len) {
            return {
              value: arr[index++],
              done: false
            };
          } else {
          	//遍历结束
            return {
              value: arr[index++],
              done: true
            };
          }
        }
      };
    }
  };
  //验证接口（如果没有部署成功是obj没法用for..of进行循环的）
  for (let key of obj) {
    console.log(key); //1 3 2 7 9 8
  }
}
```

#### iterator扩展

使用三点运算符和解构赋值时，会默认去调用iterator接口

```
{
    let arr1 = [1,6];
    let arr2 = [2,3,4,5];
    arr1 = [1, ...arr2, 6];
    console.log(arr1) //1 2 3 4 5 6
    let [a,b] = arr1;
    console.log(a,b) //1 2
}
```

#### for...of循环

```
{
    let set = new set([1,2,3,4,5,6]);
    for(let i of set){
        console.log(i); //1 2 3 4 5 6
    }
}
```

#### iterator应用实例

以Symbol.iterator为key：可迭代协议
返回next函数，next返回值done+value：迭代器协议

```
//获取所有作者名单
let authors = {
  allAuthors: {
    fiction: ['Agla', 'Skks', 'LP'],
    scienceFiction: ['Neal', 'Arthru', 'Ribert'],
    fantasy: ['J.R.Tole', 'J.M.R', 'Terry P.K']
  },
  Addres: []
}

//这种方式缺点：当数据结构发生改变时，都要改变循环
 let r = []
 for (let [k, v] of Object.entries(authors.allAuthors)) {
 	//聚合
     r = r.concat(v)
 }
 console.log(r)

//改变方式，部署iterator接口，使其可以直接被for...of循环，不关心内部结构，更优雅
const author = new Proxy(allAuthors,{
	has(target,key){
	  const keys = Reflect.ownKeys(target).slice(0,-2)
	  //console.log(keys) fiction、scienceFiction、fantasy
	  for(const item of keys){
	  	if(target[item].includes(key)){
	  		return true
	  	}
	  }
	}
})	
authors[Symbol.iterator] () {
  //this.allAuthors取到authors对象的allAuthors
  let allAuthors = this.allAuthors
  let keys = Reflect.ownKeys(allAuthors).slice(0,-2)
  let values = []
  return {
    next () {
      if (!values.length) {
        if (keys.length) {
          values = allAuthors[keys[0]]
          //从首部弹出去 
          keys.shift()
        }
      }
      return {
        done: !values.length
        //返回第一个元素的值
        value: values.shift()
      }
    }
  }
}


let r = []
for (let v of authors) {
  r.push(v)
}
console.log(r)

//generator实现 不再手动写迭代器协议
authors[Symbol.iterator] = function * () {
  //this.allAuthors取到authors对象的allAuthors
  let allAuthors = this.allAuthors
  let keys = Reflect.ownKeys(allAuthors).slice(0,2)
  let values = []
  //无限循环 遇到yeild即取出一个值后就暂停
  while(1){
     if (!values.length) {
        if (keys.length) {
          values = allAuthors[keys[0]]
          //从首部弹出去 
          keys.shift()
          yield values.shift()
        }else{
          return false
        }
     }else{
     	 yield values.shift()
     }
  }
}
this.authors = authors

let r = []
for (let v of authors) {
  r.push(v)
}
console.log(r)
```

### 十六.Generator

#### Generator基本概念

- 让遍历‘停下来’，而不是break/return这种退出
- 异步编程的解决方案之一
- Generator函数是一个状态机，内部封装了不同状态的数据
- 用来生成遍历器对象
- 可暂停函数，又叫惰性求值函数，yield可暂停，next方法可启动，每次返回的是yield后的表达式结果

特定：

- function后紧跟*号
- 内部用yield语句定义不同状态
- generator返回的是指针对象，而不会执行函数内部逻辑
- 调用next方法（会执行）内部逻辑开始执行，遇到yield表达式停止，函数返回当前数据和结果{value:yield表达式结果/undefined, done: true/false}
- **Generator 函数的定义不能使用箭头函数，否则会触发 SyntaxError 错误**

```
{
    function * myGenerator(){
        let result = yield 'hello';
        console.log(result)//yield语句默认返回undefined
        yield 'generator';
        return 'over';//指定返回的值
    }
    let MG = myGenerator();//返回指针对象
    console.log(MG); //看__proto__原型对象，原型的原型上有一个属性Symbol(Symbol.toStringTag): 						"Generator" 表示标识当前Generator函数
    console.log(MG.next());//返回的是iterator接口next返回的形式{value: "hello", done: false}
    console.log(MG.next('abc'));//next方法可以传参，表示启动yield语句时候的返回值，result此时的值为								 abc
    console.log(MG.next());////{value: "generator", done: false}
    console.log(MG.next());//{value: "over", done: true}
    
}
```

#### Generator基本用法

yeild没有返回值
yeild后有无 * 的区别
yeild后还可以是可遍历的对象或者generator对象（加不加*号），输出会有所不同
在yield后可以写不同的ajax请求，异步请求，请求成功后，还可以手动请求next，执行后面的语句
在函数内部，next调用后，value代表当前函数的值，done代表循环是否已经结束
t通过next可以解决死循环

```
{
  //基本语法解析
  let tell = function*() {
    yield "a";
    yield "b";
    return "c";
  };
  let k = tell();
  // 调用next()时 会去执行一个yield，保证了函数体内异步操作的过程
  console.log(k.next());
  k.return(); //提前结束循环，就和for循环的return是一样的，return可以传值，传值影响返回的generator对象的value				值
  console.log(k.next());
  console.log(k.next());
  console.log(k.next());
  // {value: "a", done: false}
  // {value: "b", done: false}
  // {value: "c", done: true}
  // {value: undefined, done: true}
}
{
	function * gen () { 
		//yeild后可以是常量 也可以是数组
		let val = yeild [1，2，3]
		console.log(val)
	}
	let g = gen()
	g.next(); //undefined  
}

{
	function * gen () { 
		//yeild后可以是常量 也可以是数组
		let val = yeild [1，2，3]
		console.log(val)
	}
	let g = gen()
	
	console.log(g.next(10)) //{value: [1,2,3], done: false}
	console.log(g.next(20)) //undefined {value: undefined, done: true}
	
	//next方法可以传参
	console.log(g.next(10)) //{value: [1,2,3], done: false}
	console.log(g.next(20)) //20 {value: undefined, done: true}
	//传参实际上是改变yeild返回值的方式，第一次没有涉及到赋值，第二次才赋上值
}
{
	//提前结束除了return还可以用抛出异常
	function * gen () {
		while(true){
			try{
				yeild 1
			}catch(e){
				console.log(e.message)
			}
		}
	}
	let g = gen()
	g.throw(new Error(ss))//捕获错误不会阻止程序的运行
	console.log(g.next())
}
```

#### generator 和 iterator 的关系

generator实质是一个遍历器生成函数，可以把它直接赋值给symbol的iterator，使对象具有iterator接口

```
{
	let obj = {};
	//generator函数用于返回遍历器对象，相当于部署了一个iterator接口
	obj[Symbol.iterator] = function* (){
	    yield 1;
	    yield 2;
	    yield 3;
	}
	//遍历状态
	for (let value of obj) {
	    console.log(value);
	    //1
	    //2
	    //3
	 }
}
```

#### 状态机

Generator 是实现状态机的最佳结构

只有abc3种状态

```
{
	let state = function* (){
		//永远会执行这个状态 通过while函数不断获取当前的状态
		while(1){
			yield 'A';
			yield 'B';
			yield 'C';
		}
	}
	let status = state();
	console.log(status.next())
	console.log(status.next())
	console.log(status.next())
	console.log(status.next())
	console.log(status.next())
    //	{value: "A", done: false}
    //	{value: "B", done: false}
    //	{value: "C", done: false}
    //	{value: "A", done: false}
    //	{value: "B", done: false}
}
```

#### 应用实例（多写几遍）

1. 年会抽奖模拟 一等奖：一名 二等奖：三名 三等奖：五名 

```
{
	// ES5 实现方法
    function draw(first = 1, second = 3, third = 5) {
      let firstPrize = ['1A', '1B', '1C', '1D', '1E']
      let secondPrize = ['2A', '2B', '2C', '2D', '2E', '2F', '2G', '2H', '2I']
      let thirdPrize = ['3A', '3B', , '3C', '3D', '3E', '3F', '3G', '3H', '3I', '3K', '3J']
      let result = []
      let random
      // 抽一等奖
      for (let i = 0; i < first; i++) 
      	//产生一个0到5的随机数
        random = Math.floor(Math.random() * firstPrize.length)
        //在组合的同时，要把抽奖删掉，避免重复 firstPrize.splice(random, 1)通过取索引
        result = result.concat(firstPrize.splice(random, 1))
      }
      // 抽二等奖
      for (let i = 0; i < second; i++) {
        random = Math.floor(Math.random() * secondPrize.length)
        result = result.concat(secondPrize.splice(random, 1))
      }
      // 抽三等奖
      for (let i = 0; i < third; i++) {
        random = Math.floor(Math.random() * thirdPrize.length)
        result = result.concat(thirdPrize.splice(random, 1))
      }
      return result
    }
    let start = draw()
    for (let value of start) {
      console.log(value) //抽奖一次性抽完1、2、3等奖，不合理
    }

	//es6实现方式
    function* draw(first = 1, second = 3, third = 5) {
      let firstPrize = ['1A', '1B', '1C', '1D', '1E']
      let secondPrize = ['2A', '2B', , '2C', '2D', '2E', '2F', '2G', '2H', '2I']
      let thirdPrize = ['3A', '3B', , '3C', '3D', '3E', '3F', '3G', '3H', '3I', '3K', '3J']
      let count = 0
      let random
      while (1) {
        if (count < first) { // 抽一等奖
          random = Math.floor(Math.random() * firstPrize.length)
          yield firstPrize[random]
          count++ // 抽一次计数器+1
          //firstPrize.splice(random, 1)
          //一等奖只有1个不需要删除
        } else if (count < first + second) { // 抽二等奖 判断二等奖有没有抽完
          random = Math.floor(Math.random() * secondPrize.length)
          yield secondPrize[random]
          count++ // 抽一次计数器+1
          secondPrize.splice(random, 1)//同时抽一次删除一次
        } else if (count < first + second + third) {
          random = Math.floor(Math.random() * thirdPrize.length)
          yield thirdPrize[random]
          count++ // 抽一次计数器+1
          thirdPrize.splice(random, 1)
        } else { // 如果三个奖项都抽完
          return false
        }
      }

	//新建一个抽奖元素
	let btn = document.createElement("button");
	btn.id = 'start';
	btn.textContent = "抽奖";
	//按钮添加到页面
  	document.body.appendChild(btn);
  	//注册事件
  	document.getElementById('start').addEventListener('click',function(){
  		//执行
  		start.next();
  		//console.log(start.next().value)
  	},false)
}
```

2.长轮询

```
{
	//状态值服务器端定期变化，前端不断获取
	//因为http是无状态的连接 两种方式，1长轮询 2websocket(有兼容性问题)
	let ajax = function* (){
		//generator和promise的结合
		yield new Promise(function(resolve,reject){
			//实际开发中这段拿来写实际接口
			setTimeout(function () {
				resolve({code:0})
			},200)
		})
	}
	//轮询
	let pull = function (){
		//实例化generator
		let generator = ajax();
		//取得当前generator步骤
		let step = generator.next();
		//value即promise实例（iterator接口两个参数value和done）  d即是code=0的数据
		step.value.then(function(d){
			//如果code拿到的不是最新的值去重新请求
			if(d.code!=0){
				setTimeout(function(){
					console.info('wait');
					pull();
				},1000)
			}else{
				console.info(d);
			}
		})
	}
	pull();
}
```

3.内容、评论接口异步请求（对比promise封装ajax实例）

generator与promise对比，关键是可以暂停

```
function getNews(url){
    $.get(url,function(data){
        console.log(data)
        //拿到url
        let url = 'xxx' + data.commentsUrl;
        //上一次请求成功后再去调用next
        SX.next(url);
    })
}
function* sendXml(){
    let result = yield getNews('xxxurl?id=xx');
    //url通过外部next传参进来
    yield getNews(url);
}
//获取遍历器对象
let SX = sendXml();
SX.next();
```

#### generator语法糖-async/await

详细见十七章

```
{
	let state = async function (){
		//永远会执行这个状态 通过while函数不断获取当前的状态
		while(1){
			await 'A';
			await 'B';
			await 'C';
		}
	}
	let status = state();
}
```

### 十七.Decorator 修饰器

一个用来修改类的行为的**函数**，可以理解为扩展类的功能(只在类的范畴中有用)

需要安装修饰符 插件(babel-plugin-transform-decorators-legacy)

npm install babel-plugin-transform-decorators-legacy --save-dev

修改package.json 里面的 bable 或者 .bablerc 文件 

"plugins": ["@babel/plugin-proposal-decorators", { legacy: true }]

#### 基本用法

```
{
  //限制某个参数为只读的
  //descriptor该属性的描述对象
  let readonly = function(target, name, descriptor) {
    descriptor.writable = false
    console.log(target, name, descriptor)
    
    return descriptor
  };

  class Test {
    //在这里加了修饰器  不允许重新赋值 在花括号里面
    @readonly
    time() {
      return "2019-07-11";
    }
  }

  let test = new Test();
  test.time = function(){
  	console.log('reset time');
  }
  console.log(test.time()); //2019-07-11
}
{
  let typename = function(target, name, descriptor) {
  	//target指类本身而非类实例,增加一个静态属性
    target.myname = "hello";
  };
	//在花括号外
  @typename
  class Test {}
  //类修饰符
  console.log(Test.myname); //hello
}
```

#### 第三方decorator库

core-decorators

有了这个库，就可以直接import@xxx

#### 实例

##### 1.日志系统

```
{
	let log = type => {
		return function(target, name, descriptor){
			//原始函数体
			let src_method = descriptor.value;
			//重新赋值
			descriptor.value = (...arg) =>{
				src_method.apply(target,arg);
				//实际开发中换成自己的埋点
				console.log(`log ${type}`);
			}
		}
	}
	
	//广告类
	class AD{
		@log('show')
		show(){
			console.log('ad is show');
		}
		@log('click')
		click(){
			console.log('ad is click');
		}
	}
	let ad = new AD();
	ad.show();
	ad.click();
	/*
	 ad is show
	 log show
	 ad is click
	 log click
	 */
	//类中函数先执行,埋点后执行,把埋点系统都抽离出来,可复用,可维护性高(修饰器优点)
}
```

##### 2.检查登录

```
{
    class User {
        // 获取已登录用户的用户信息
        @checkLogin
        getUserInfo() {
            /**
             * 之前，我们都会这么写：
             *      if(checkLogin()) {
             *          // 业务代码
             *      }
             *  这段代码会在每一个需要登录的方法中执行
             *  还是上面的问题，执行的前提和业务又混到了一起
             */
            console.log('获取已登录用户的用户信息')
        }
        // 发送消息
        @checkLogin
        sendMsg() {
            console.log('发送消息')
        }
    }

    // 检查用户是否登录，如果没有登录，就跳转到登录页面
    function checkLogin(target, name, descriptor) {
        let method = descriptor.value

        // 模拟判断条件
        let isLogin = true

        descriptor.value = function (...args) {
            if (isLogin) {
                method.apply(this, args)
            } else {
                console.log('没有登录，即将跳转到登录页面...')
            }
        }
    }
    let u = new User()
    u.getUserInfo()
    u.sendMsg()
}
```

### 十八Module模块化

- 模块化编译es6语法，可用webpack和rollup
- 引入import
- 导出export
- 对象的导出 解构赋值，不能在导出的语句同时解构，需要在外面解构
- 被导出的模块在本模块也能使用


方法一:不推荐

```
//不能加区块域	
//导出变量
export let A = 123;
//导出函数
export function test() {
  console.log("test");
}
//导出类
export class Hello {
  test() {
    console.log("class");
  }
}

//引入
import {A,test,Hello} from './19module.js'
console.log(A,test,Hello)
//全部引入
import * as OtherName from "./class/lesson17";
console.log(OtherName.A)
```

方法二：推荐

```
let A = 123;
let test = function() {
  console.log("test");
};

class Hello {
  test() {
    console.log("class");
  }
}
//导出
export default {
  A,
  test,
  Hello
};

//方法二引入
import module from './19module.js'
console.log(module.A)
//或者
import * as module from './19module.js'
console.log(module.A)
//对于有default的导出对象/类/函数/变量
module.default


//默认导出一个类
export default ClassXX
//或者
export default class {
}
import Test/ClassXX from 'xx'
//没有默认值
export class ClassXX{
}
import {ClassXX} from 'xx'
```

#### export default 和 export 区别

- 在一个文件或模块中，export、import可以有多个，export default仅有一个

  (1) 输出单个值，使用export default，并且名字可以自己改变（导出导入不同）
  (2) 输出多个值，使用export 【注意：引入时要加花括号 import { A } from " B" 】解构引入
  (3) export default与普通的export不要同时使用

- 通过export方式导出，在导入时要加{ }，export default则不需要



### es5补充：call、apply、bind

function.prototype.bind(obj)作用：将函数内的this绑定为obj，并将函数返回

**面试题：区分以上三个？**

- 都能指定函数中的this
- call()、apply()是立即调用函数，call与apply传入参数不同
- bind()是将函数返回,通常是用来指定回调函数的this，传参方式和call一样
- bind只能用**函数表达式**，函数声明不可用，会报错


```
{
	let obj = { a: 'aaa'};
	function foo() {
        console.log(this);//此处this指向window
	}
	foo.call(obj)//使用call后，上面的this指向对象obj
	//foo.apply(obj)
}
```

区别：

```
{
    let obj = { a: 'aaa'};
	function foo(data) {
        console.log(this,data);//此处this指向window
	}
	foo.call(obj,333)//使用call，一个一个参数传入
	foo.apply(obj,[333])//第二个参数必须是数组
	var bar = foo.bind(obj，33)//bind特定:绑定完this不会立即调用当前的函数，而是将函数返回
	//console.log(bar);
	///bar();
	
	//把定时器里的this指向obj
	setTimeout(function(){
        console.log(this);//window
	},1000)
	setTimeout(function(){
        console.log(this);//window
	}.bind(obj),1000)
}
```

### 总结：克隆

```
{
	//1.直接赋值
    let obj = {username: 'kobe', age: 39};
    let obj1 = obj;
    console.log(obj1); //{username: 'kobe', age: 39}
    obj1.username = 'wade';
    console.log(obj.username);//wade
}
{
	//2.Object.assign
	let obj = {'name':'111','age':'222'};
	let obj2 = Object.assign(obj);
	console.log(obj2);//'{'name':'111','age':'222'}'
	obj2.name = '222';
	console.log(obj)//{'name':'222','age':'222'}
}
以上两种都是浅拷贝，会改变原来引用类型数据的值
```

- 基本类型(undefined、null、number、boolean、string)，因为数据是保存在栈内，复制是生成一个新的地址，不会影响原数据
- 引用类型Object(对象、数组、函数....)指向原来的对象，值是保存在堆内，复制的只是指向储存对象内存的地址(引用)，没有生成新的数据，改变它的值，会影响复制了它的值的变量 
- 如果有两个变量的值是引用类型值，就算它们的值完全相同，它们也是不相等的，因为它们指向的内存地址不同 

**补充： null和undefined的区别**

null表示"没有对象"，即该处不应该有值。

　　典型用法是：

> （1） 作为函数的参数，表示该函数的参数不是对象。
>
> （2） 作为对象原型链的终点。

undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。

　　典型用法是：

> （1）变量被声明了，但没有赋值时，就等于undefined。
>
> （2)  调用函数时，应该提供的参数没有提供，该参数等于undefined。
>
> （3）对象没有赋值的属性，该属性的值为undefined。
>
> （4）函数没有返回值时，默认返回undefined。

**常用拷贝方法**

- 浅拷贝（对象/数组）--会影响原数据，数据不安全
- 深拷贝/深度克隆--不会影响原数据

1. 直接赋值给一个变量---浅拷贝
2. Object.assign()---浅拷贝
3. Array.prototype.concat()数组浅拷贝  不传参数
4. Array.prototype.slice()数组浅拷贝 不传参数相当于截取整个数组
5. JSON.parse(JSON.stringify(arr/obj))数组或对象深拷贝，但不能处理函数数据

<http://caibaojian.com/javascript-object-clone.html> 

**以上3、4使用，数组里的数据非引用类型时，一个数组变化，另一个数组不受影响** 

```
{
	let arr = [1, 3, {username: 'kobe'}];
    let arr2 = arr.concat();
    console.log(arr2);//[1, 3, {username: 'kobe'}]
    arr2[2].username = 'wade';//引用类型时，原数组会改变
    
    arr2.push(1)
    arr2[1] = '111';//数组里的数据非引用类型，原数组不会改变原有值
    
    console.log(arr);//[1, 3, {username: 'wade'}]
    let arr3 = arr.slice();
    console.log(arr3);//[1, 3, {username: 'wade'}]
    arr3[2].username = 'one';
    console.log(arr);//[1, 3, {username: 'one'}]
    
    let arr4 = JSON.parse(JSON.stringify(arr));
    console.log(arr4);//[1, 3, {username: 'one'}]
    arr4[2].username = 'JSON';
    console.log(arr);//[1, 3, {username: 'one'}]原数据无影响
}
```

**深度克隆**

如何实现深度克隆/深拷贝？

补充：

typeof返回的数据类型：String/Number/Boolean/undefined/Object/Function（null也返回Object）

Object.prototype.toString.call(obj)  ==>立即调用这个call方法，并且使this指向obj

slice包含开始，不包含结束（-1）

for...in循环枚举的时候，对象是枚举的key（属性名），数组枚举的是下标

```
{
    //定义检测数据类型的功能函数
    function checkType(target){
        return Object.prototype.toString.call(target).slice(8,-1);
    }
    //实现深度克隆 ==》对象/数组
    function clone(target){
        //判断拷贝的数据类型
        //初始化变量result，成为最终克隆的数据
        let result, targetType = checkType(target);
        //判断
        if(targetType === 'Object'){
            result = {};
        }else if(targetType === 'Array'){
            result = [];
        }else{
            return target;
        }
        //遍历目标数据
        for(let i in target){
        	//获取数据结构的每一项值
            let value = target[i];
            //result[i] = value;
            //判断目标结构里的每一项是否存在对象/数组（嵌套）
            if(checkType(value) === 'Object' || checkType(value) === 'Array'){
            	//继续遍历获取到的value值---递归
            	//如果进入到这一步，就说明有嵌套数据
            	result[i] = clone(value)
            }else{
            	//进入这一步，说明value值(原数据)是基本数据类型或者是函数
                result[i] = value;
            }
        }
        return result;
    }
    let arr = [1, 3, {username: 'kobe', age: 18}];
    let arr2 = clone(arr);
    arr2[2].username = '19';
    console.log(arr,arr2);//[1, 3, {username: 'kobe', age: 18}] [1, 3, {username: 'kobe', age: 19}]
}
```

### ES7扩展

#### 指/幂数运算符

指数运算符** 

```
{
	//es5
    console.log(Math.pow(3,3));//27
    //es7简写
    console.log(3 ** 3);//27
}
```

#### Array.prototype.includes(value)

[详见数组扩展](#includes)

### es8扩展

####  Async/Await 

真正意义上去解决异步回调的问题，同步流程表达异步操作！！

特点：

-  函数加了async后，返回的对象是promise对象，实例 instanceof Promise
-  有async才能使用await，使用async/await需要用babel-polyfill编译
-  不需要像Generator去调用next，遇到await等待，当前异步操作完成就往下执行
-  返回的是Promise，但不像.then方法进行下一步操作
-  async取代Generator函数的*，await取代Generator的yield
-  await后必须是promise对象，就算是数字，也会转换成promise.resolve对象

##### 基本使用

```
{
	async function foo() {
        return new Promise(resolve => {
         	/*setTimeout(function () {
                resolve();
         	},2000)*/
         	//另一种写法
         	setTimeout(resolve, 5000)
        })
    }
    async function test(){
        console.log('111',new Date().toTimeString());
        await foo();
        console.log('222',new Date().toTimeString());
    }
    test();
    //说明等待后继续执行了后面的程序，如何看出来有等待？时间间隔了5秒
    /*
     * 111 17:53:40
     * 222 17:53:45
     */
}

{
    //await的返回值
	async function asyncPrint(){
		//Promise作为对象，可以直接调用resolve(),表示成功的状态
		let result = await Promise.resolve();
		let result1 = await Promise.resolve('success');
		let result2 = await Promise.reject();
		let result3 = await Promise.reject('fail');
		
		console.log(result)//默认值为undefined
		console.log(result1)//success 如果有传入参数则为传入参数的值
		console.log(result2)//16asyncAndAwait.js:1 Uncaught (in promise) undefined
		console.log(result3)//16asyncAndAwait.js:1 Uncaught (in promise) fail
	}
	asyncPrint();
}
```

```
{
	 function loading(src){
        return new Promise((resolve,reject)=>{
        	//首先创建一个img标签
        	let img = document.createElement("img");
            //监听图片加载完成
            img.onload = function(){
            	resolve(img);
            }
            //onerror 事件在加载外部文件（文档或图像）发生错误时触发
            img.onerror = function(err){
            	reject(err)
            }
            img.src = src
         })
       }

        var src1 = 'xxx.jpg';
        var src2 = 'xxx.jpg';
        
        //new Promise写法那么需要用.then方法继续调后续操作
        var result1 = loading(src1);
        var result2 = loading(src2);
        result1.then((img) => {
        	console.log(1,img.width);
        	return result2 
        }.then((img) => {
        	console.log(2,img.height);
        }).catch((error) => {
        	console.log(3,error);
        })
        
        //async/await方式
        const load = async function (){
            const result1 = await loading(src1)
            console.log(result1)
            const result2 = await loading(src2)
            console.log(result2)
        }
        load();
}
```

##### 实例改写

```
1.不用改写，因为它是点击一次需要暂停的而await是继续执行后续代码
2.长轮询
{
	//状态值服务器端定期变化，前端不断获取
	let ajax = async function (){
		//generator和promise的结合
		return new Promise(function(resolve,reject){
			//实际开发中这段拿来写实际接口
			setTimeout(function () {
//				let code = 500;
				let code = 0;
				resolve(code)
			},200)
		})
	}
	//轮询
	let pull = async function (){
		let result = await ajax();
		if(result!=0){
			setTimeout(function(){
				console.info('wait');
				pull();
			},1000)
		}else{
			console.info(result);
		}
	}
	pull();
}
3.新闻+内容封装ajax
{
    async function getNews(url){
    	return new Promise((resolve,reject) => {
             $.ajax({
                methods: 'GET',
                url,//url:url es6同名属性缩写
                /*success：function(data){
					resolve();
                },*/
                //箭头函数写法 函数体一行代码省略花括号
                //传参后自动作为await的返回值
                success: data => resolve(data),
                //!注意如果需要报错，最好不要直接调用reject，因为一旦调用报错就不执行后续内容
                //error: error => reject()
                error: error => resolve(false)
            })
    	})
    }
    async function sendXml(){
        let result = await getNews('xxxurl?id=xx');
        console.log(result)//data
        //如果请求有问题给出用户提示
        if(!result){alert('暂时没有新闻！！！');return;}
        let url = 'xxx' + data.commentsUrl;
        await getNews(url);
    }
    sendXml();
}
```

#### String padding

详见字符串扩展

####  Object.keys/Object.values/Object.entries

详见对象扩展

#### **Object.getOwnPropertyDescriptors** 

 如何获取Object数据的描述符(descriptor) 
 defineProperty 的第三个参数就是描述符(descriptor)。这个描述符包括几个属性：

- value [属性的值]
- writable [属性的值是否可被改变/只读]
- enumerable [属性的值是否可被枚举]
- configurable [描述符属性是否可被删除]

```
//例如前端做状态报才去操作数据
//名单
const data = {
  Portland: '78/50',
  Dublin: '88/52',
  Lima: '58/40'
}
//需求：上边的代码把所有的 key、value 遍历出来，如果我们不想让这份名单有 Lima 或者说这个属性和值被枚举
Object.defineProperty(data, 'Lima', {
  enumerable: false,
  writable: false
})
Object.entries(data).map(([name, feature]) => {
  console.log(`Name: ${name.padEnd(16)} Feature: ${feature}`)
  // Name: Portland         Feature: 78/50
  // Name: Dublin           Feature: 88/52
})

Object.getOwnPropertyDescriptors(data);//输出所有数据，并得到各个属性值（只读/被枚举/被修改等等）
Object.getOwnPropertyDescriptor(data,'lima');//只拿某一项，查看其深层信息属性值
```

### es9扩展

#### for await of

异步遍历

```
   function gen(time) {
        return new Promise(resolve => {
         	setTimeout(resolve(time), time)
        })
    }
    
    function test () {
    	let arr = [gen(2000),gen(100),gen(3000)]
    	for(let item of arr){
    		//console.log直接输出resolve的值
    		console.log(Date.now(),item.then(console.log))
    	}
    }
    /*
        时间戳 Promise {<pending>}
        时间戳 Promise {<pending>}
        时间戳 Promise {<pending>}
        100
        2000
        3000
    */
    
      async function test () {
    	let arr = [gen(2000),gen(100),gen(3000)]
    	for(let item of arr){
    		console.log(Date.now(),await item.then(console.log))
    	}
      }
      /*
        2000
        时间戳 undefined
        100
        时间戳 undefined
        3000
        时间戳 undefined
      */
      
      //用 for…await…of 真正实现
      async function test () {
    	let arr = [gen(2000),gen(100),gen(3000)]
    	for await(let item of arr){
    		console.log(Date.now(), item)
    	}
      }
      /*
      	时间戳 2000
        时间戳 100
        时间戳 3000
      */
```

自定义数据结构异步遍历

```
//自定义数据结构异步遍历
const obj = {
	count: 0,
	Gen(time){
		 return new Promise((resolve,reject) => {
         	setTimeout({
         		resolve({value: time, done: false})
         	},time)
        })
	},
	[Symbol.asyncIterator] () {
		let self = this;
		 return {
             next () {
             	self.count++;
             	if(self.count < 4){
             		return self.Gen(Math.random() * 1000)
             	}else{
             		return Promise.resolve({
             			done: true,
             			value: ''
             		})
             	}
             }
          }
	}
}
for await(let item of obj){
	console.log(Date.now(), item)
}
```

####  Promise.finally

promise兜底操作： 执行完promise后，无论结果是fulfilled还是rejected都需要执行的代码提供了一种方式，避免同样的语句需要在then()和catch()中各写一次的情况 

```
//数据库最后要关闭
let connection;
db.open()
.then(conn => {
    connection = conn;
    return connection.select({ name: 'Jane' });
})
.then(result => {
    // Process result
    // Use `connection` to make more queries
})
···
.catch(error => {
    // handle errors
})
.finally(() => {
    connection.close();
});
```

#### Object.rest.spread

剩余、扩展运算符在对象的运算，代替浅拷贝、深拷贝

```
const input = {
  a: 1,
  b: 2
}

const output = {
  ...input,
  c: 3
}

console.log(input,output) // {a: 1, b: 2} {a: 1, b: 2, c: 3}  
input.a = 4;
//相当于浅拷贝
console.log(input,output) // {a: 4, b: 2} {a: 1, b: 2, c: 3}
```

#### RegExp-dotAll

/s 匹配任何空白字符,包括空格、制表符、换页符等等

```
//.不能匹配四个字节的utf16字符和行终止符\n,\r
console.log(/foo.bar/.test('foo\nbar')) //false

//dotAll
console.log(/foo.bar/s.test('foo\nbar')) 
//全能，四个字节的utf16字符和行终止符\n都能匹配
console.log(/foo.bar/us.test('foo\nbar')) //true

//如何判断正则是否启用了dotAll模式
const re = /foo.bar/s 
console.log(re.dotAll) //true  加了s修饰符
console.log(re.flags)  //s
```

#### RegExp-命名分组捕获

```
const t =' 2020-01-23'.match(/(\d{4})-(\d{2})-(\d{2})/)
console.log(t) //["2020-01-23", "2020", "01", "23", index: 0, input: "2020-01-23", groups: undefined]
//根据index取值
console.log(t[1]) //2020
console.log(t[2]) //01

//?<命名name>
const t = '2020-01-23'.match(/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/)
console.log(t.groups.year) //2020
console.log(t.groups.month) //01
console.log(t.groups.day) //23
```

#### RegExp-后行断言

```
//先行断言：先遇到一个条件，判断后面的条件是否满足
let test = 'hello world'
console.log(test.match(/hello(?=\sworld)/))

//后行断言
//判断world的前面是不是hello
console.log(test.match(/(?<=hello\s)world/))
//匹配world前面不是hello的
//?<  采用后行断言
console.log(test.match(/(?<!hells\s)world/))

//练习1 把“$foo %foo foo”前面为$的foo替换成bar
let test = "$foo %foo foo";
test = test.replace(/(?<=\$)foo/,'bar')

//练习2 提取“$1 is worth about ￥123”字符中美元的数字
let test1 = "$3 is worth about ￥123"
let res = /(?<=\$)(?<money>\d)/.exec(test1)
console.log(res.groups.money)
```

### es10扩展

#### JSON.stringify

JSON.stringify 在 ES10 修复了对于一些超出范围的 Unicode 展示错误的问题。因为 JSON 都是被编码成 UTF-8，所以遇到 0xD800–0xDFFF 之内的字符会因为无法编码成 UTF-8 进而导致显示错误。在 ES10 它会用转义字符的方式来处理这部分字符而非编码的方式，这样就会正常显示了 

```
/*
    /u 表示按unicode(utf-8)匹配（主要针对多字节比如汉字）
    /i 表示不区分大小写（如果表达式里面有 a， 那么 A 也是匹配对象）
*/
JSON.stringify('\u{D800}') // "\ud800"
```

####  flat和flatmap 

详见数组拓展

#### trimStart/trimEnd

```
//去除首 或者 尾的空白字符串
let str = "  foo   "
console.log(str.trimStart().trimEnd())
//trimLeft = trimStart  trimRight = trimEnd
//trim = trimStart+trimEnd
```

####  **matchAll** 

```
//需求：提取单词 `"foo" and "bar" and "baz"`

//回顾下 ES10 之前一共有多少种正则全部遍历的方法
//1. RegExp.prototype.exec
function collectGroup1 (regExp, str) {
  const matches = []
  while (true) {
    const match = regExp.exec(str)
    if (match === null) break
    // Add capture of group 1 to `matches`
    matches.push(match[1])
  }
  return matches
}
//加g后会从匹配后的下一个位置继续找，正则表达式 []表示捕获 捕获不是双引号
collectGroup1(/"([^"]*)"/g, `"foo" and "bar" and "baz"`)
// [ 'foo', 'bar', 'baz' ]

//2. String.prototype.match
//如果用 .match 方法结合 /g 的正则模式，将会把所有的匹配打包成一个数组返回，换句话说所有的捕获被忽略
let str = `"foo" and "bar" and "baz"`;
str.match(/"([^"]*)"/g)
[ '"foo"', '"bar"', '"baz"' ]
//如果没有使用 /g 的正则模式，.match 的效果和 RegExp.prototype.exec() 是一致的

//3.String.prototype.replace
function collectGroup1 (regExp, str) {
  const matches = []
  function replacementFunc (all, first) {
    matches.push(first)
  }
  //replace第二个参数可传函数
  str.replace(regExp, replacementFunc)
  return matches
}

collectGroup1(/"([^"]*)"/ug, `"foo" and "bar" and "baz"`)
// ["foo", "bar", "baz"]

//4.matchAll 方法
function collectGroup1 (regExp, str) {
  let results = []
  for (const match of str.matchAll(regExp)) {
    results.push(match[1])
  }
  return results
}
collectGroup1(/"([^"]*)"/g, `"foo" and "bar" and "baz"`)
// ["foo", "bar", "baz"]
```

####  fromEntries (把数组转化为对象 )

```
const arr = [["foo", 1], ["bar", 2]]
const obj = Object.fromEntries(arr)
console.log(obj) //{foo: 1, bar: 2}

// 筛选出obj中key的长度为3的项
const obj = {
    "abc": 1,
    "def": 2,
    "ghsjk": 3
}
//最后用fromEntries把数组还原成对象
let res = Object.fromEntries(
	//[key, val]解构赋值
    Object.entries(obj).filter(([key, val]) => key.length == 3)
    /*
    Object.entries(返回一个给定对象自身可枚举属性的键值对数组
    console.log(Object.entries(obj));  // [['abc', '1'], ['def', '2'], ['ghsjk', '3']]
    */
)
console.log(res)
/*{
    "abc": 1,
    "def": 2,
}*/
```

####  try...catch ..的增强

```
//es10允许catch后不带参数e
try {
         
} catch {

}
```

####  BigInt类型 

```
//用于处理2的53次方以上的数据类型---长数据
//加一个n
console.log(typeof 11n)

//bigInt处理后台的超长字符串
let d = BigInt('214119685394665472')
console.log(d) //214119685394665472n
```



### es11扩展

#### 可选链操作符 

Optional chaining operator

 https://blog.csdn.net/Smell_rookie/article/details/103840271 

```
//es5
console.log(this.obj && this.obj.useInfo && this.obj.userInfo.other && this.obj.userInfo.other.name)
//可选链操作符
console.log(this.obj?.userInfo?.other?..name)

//es5
function getLeadingActor(movie) {
  if (movie.actors && movie.actors.length > 0) {
    return movie.actors[0].name;
  }
}
//可选链
function getLeadingActor(movie) {
  return movie.actors?.[0]?.name;
}

const arr=[1,2,3]
//  es5
if(arr.length){
	arr.map(res=>{
		//处理你的逻辑
	})
}
// 可选链
   arr?.map(item=>{
       //处理你的逻辑
   })

```

####   空值合并运算符 

```
//当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数

let c = a ?? b
//等价于 特别适用于值为0的时候
let c = a !== undefined  && a !== null ? a : b

let c = 0 // 真实有效的输入值
let d =c ?? 9000  // 0

```

#### Promise.allSettled()

 Promise.allSettled()不管参数中的promise是fulfilled还是rejected，都会等参数中的实例都返回结果，包装实例才会结束。 

### 实战---彩票项目（选）

#### 项目介绍

11选5---快频彩种

1.每10分钟一期，早9晚10，一天一共78期，需要有每一期的期号和本期的倒计时（剩余时间）

2.玩法提升，es6动态tab切换

3.动态投选，金额、注数计算

#### time倒计时模块

- 使用class 
- 使用const  const self = this
- 模块导出 export default Timer
- 字符串模板 `<em>${d}</em>时`

```
{
    class Timer{
        countdown(end,update,handle){
            const now = new Date().getTime();
            const self = this;
            //判断now和end大小，如果当前时间比结束时间晚 倒计时就要截止了
            if(now-end>0){
                //倒计时结束 调用handle
                handle.call(self);
            }else{
                //剩余时间
                let last_time = end - now;
                //一天时间的毫秒
                const px_d=1000*60*60*24;
                //一小时时间的毫秒
                const px_h=1000*60*60;
                //一分钟的毫秒
                const px_m=1000*60;
                //一秒==1000hs
                const px_s=1000;
                //剩余时间包含多少天 向下取整
                let d=Math.floor(last_time/px_d);
                //剩余小时 要减去天数
                let h=Math.floor((last_time-d*px_d)/px_h);
                //剩余分
                let m=Math.floor((last_time-d*px_d-h*px_h)/px_m);
                //剩余秒
                let s=Math.floor((last_time-d*px_d-h*px_h-m*px_m)/px_s);
                //数组结果
                let r = [];
                if(d>0){
                    //字符串模板拼接
                    r.push(`<em>${d}</em>天`)
                }
                if(r.length || (h>0)){
                    r.push(`<em>${d}</em>时`)
                }
                if(r.length || (m>0)){
                    r.push(`<em>${m}</em>分`)
                }
                if(r.length || (s>0)){
                    r.push(`<em>${s}</em>秒`)
                }
                self.last_time = r.join('');
                //调用
                update.call(self,r.join(''));
                setInterval(function () {
                  self.countdown(end,update,handle);
                },1000)
                }
            }
        }
    }
    export default Timer
}
```

#### interface接口模块

- 导入 import $ from 'jquery'
- 异步promise+箭头函数

```
import $ from 'jquery'

class Interface{
  /**
   * [getOmit 获取遗漏数据]
   * @param  {string} issue [当前期号]
   * @return {[type]}       [description]
  */
  //如果类里面没有属性的声明，可以不用写constructor
  getOmit(issue){
  	//传统方法是传入参数 -- 回调函数callback
  	//箭头函数要特别注意this指向，是定义时候的this指向
    //闭包的形式
    let self=this;
    //之后再调用getOmit的时候直接.then就可以了
    return new Promise((resolve,reject)=>{
      $.ajax({
        url:'/get/omit',
        data:{
          issue:issue
        },
        dataType:'json',
        success:function(res){
          self.setOmit(res.data);
          resolve.call(self,res)
        },
        error:function(err){
          reject.call(err);
        }
      })
    });
  }
  
   /**
   * [getOpenCode 获取开奖号码]
   * @param  {string} issue [期号]
   * @return {[type]}       [description]
   */
  getOpenCode(issue){
    let self=this;
    return new Promise((resolve,rejet)=>{
      $.ajax({
        url:'/get/opencode',
        data:{
          issue:issue
        },
        dataType:'json',
        success:function(res){
          //保存当前中奖号码
          //调用当前对象内部的其他方法 避免回调
          self.setOpenCode(res.data);
          resolve.call(self,res);
        },
        error:function(err){
          reject.call(err);
        }
      })
    });
  }
  
  /**
   * [getState 获取当前状态]
   * @param  {string} issue [当前期号]
   * @return {[type]}       [description]
   */
  getState(issue){
    let self=this;
    return new Promise((resolve,rejet)=>{
      $.ajax({
        url:'/get/state',
        data:{
          issue:issue
        },
        dataType:'json',
        success:function(res){
          resolve.call(self,res);
        },
        error:function(err){
          reject.call(err);
        }
      })
    });
  }
}
export default Interface
```

#### calculate计算模块

初始化数组长度并默认填充0      const arr=new Array(active).fill('0')

charAt 方法返回的是 UTF-16 编码的第一个字节，实际上是无法显示的。使用字符串实例的 at 方法，可以识别 Unicode 编号大于 0xFFFF 的字符，返回正确的字符    play_name.at(0)==='r'

立即执行函数 es6不可省略方法名 递归的时候，否则会报错

使用类名直接引用---静态方法  static combine(arr,size){}

map数据结构确认有没有字符串  this.play_list.has(play_name)

map数据结构获取字符串  play_list.get(play_name)

```
class Calculate{
  /**
   * [computeCount 计算注数]
   * @param  {number} active    [当前选中的号码]
   * @param  {string} play_name [当前的玩法标识]  任几
   * @return {number}           [注数]
   */
  computeCount(active,play_name){
  	//当前注数为0
    let count=0;
    //play_list玩法列表 map类型
    const exist=this.play_list.has(play_name);
    //对数组进行组合运算 生成一个长度为active的数组 每一个元素默认为0
    const arr=new Array(active).fill('0');
    if(exist && play_name.at(0)==='r'){
    	//切割截取r几的数字 使用类名直接引用 静态方法
      count=Calculate.combine(arr,play_name.split('')[1]).length;
    }
    return count;
  }
  
   /**
   * [combine 组合运算]
   * @param  {array}  arr  [参与组合运算的数组] 选择的数组成的数组
   * @param  {number} size [组合运算的基数]  就是之前切割的任几的数字
   * @return {number}      [计算注数]
   */
  //static静态方法
  static combine(arr,size){
    let allResult=[];
    //立即执行函数 es6不可省略方法名
    (function f(arr,size,result){
      let arrLen=arr.length;
      if(size>arrLen){
        return;
      }
      //如果size===传入数组的长度
      if(size===arrLen){
        allResult.push([].concat(result,arr))
      }else{
        for(let i=0;i<arrLen;i++){
          let newResult=[].concat(result);
          newResult.push(arr[i]);
          if(size===1){
            allResult.push(newResult)
          }else{
            let newArr=[].concat(arr);
            newArr.splice(0,i+1);
            //递归
            f(newArr,size-1,newResult)
          }
        }
      }
	//括号里传入的参数就是之前传入的参数
    })(arr,size,[])
    return allResult
  }
  
  
    /**
   * [computeBonus 奖金范围预测]
   * @param  {number} active    [当前选中的号码]
   * @param  {string} play_name [当前的玩法标识]
   * @return {array}           [奖金范围]
   */
  computeBonus(active,play_name){
    const play=play_name.split('');
    const self=this;
    let arr=new Array(play[1]*1).fill(0);
    let min,max;
    if(play[0]==='r'){
    	//最小命中数
      let min_active=5-(11-active);
      if(min_active>0){
        if(min_active-play[1]>=0){
          arr=new Array(min_active).fill(0);
          min=Calculate.combine(arr,play[1]).length;
        }else{
          if(play[1]-5>0&&active-play[1]>=0){
            arr=new Array(active-5).fill(0);
            min=Calculate.combine(arr,play[1]-5).length;
          }else{
            min=active-play[1]>-1?1:0
          }
        }
      }else{
        min=active-play[1]>-1?1:0;
      }

      let max_active=Math.min(active,5);
      if(play[1]-5>0){
        if(active-play[1]>=0){
          arr=new Array(active-5).fill(0);
          max=Calculate.combine(arr,play[1]-5).length;
        }else{
          max=0;
        }
      }else if(play[1]-5<0){
        arr=new Array(Math.min(active,5)).fill(0);
        max=Calculate.combine(arr,play[1]).length;
      }else{
        max=1;
      }
    }
    return [min,max].map(item=>item*self.play_list.get(play_name).bonus)
  }
  
}

export default Calculate
```

#### base基本模块

map数据类型添加数据  this.play_list.set(名称',{})　

number set类型add添加，唯一不重复、字符串补白 this.number.add((''+i).padStart(2,'0'))

set/map clear清除 self.omit.clear()

es6遍历  let [index,item] of omit.entries()

把集合转成真正的数组 let number=Array.from(this.number)

```
import $ from 'jquery';
class Base{
  /**
   * [initPlayList 初始化奖金和玩法及说明]
   * @return {[type]} [description]
   */
  initPlayList(){
  	//初始化数据结构
    this.play_list.set('r2',{
      bonus:6,
      tip:'从01～11中任选2个或多个号码，所选号码与开奖号码任意两个号码相同，即中奖<em class="red">6</em>元',
      name:'任二'
    })
    .set('r3',{
      bonus:19,
      tip:'从01～11中任选3个或多个号码，选号与奖号任意三个号相同，即中奖<em class="red">19</em>元',
      name:'任三'
    })
    .set('r4',{
      bonus:78,
      tip:'从01～11中任选4个或多个号码，所选号码与开奖号码任意四个号码相同，即中奖<em class="red">78</em>元',
      name:'任四'
    })
    .set('r5',{
      bonus:540,
      tip:'从01～11中任选5个或多个号码，所选号码与开奖号码相同，即中奖<em class="red">540</em>元',
      name:'任五'
    })
    .set('r6',{
      bonus:90,
      tip:'从01～11中任选6个或多个号码，所选号码与开奖号码五个号码相同，即中奖<em class="red">90</em>元',
      name:'任六'
    })
    .set('r7',{
      bonus:26,
      tip:'从01～11中任选7个或多个号码，选号与奖号五个号相同，即中奖<em class="red">26</em>元',
      name:'任七'
    })
    .set('r8',{
      bonus:9,
      tip:'从01～11中任选8个或多个号码，选号与奖号五个号相同，即中奖<em class="red">9</em>元',
      name:'任八'
    })
  }
  
  /**
   * [initNumber 初始化号码]
   * @return {[type]} [description]
   */
  initNumber(){
    for(let i=1;i<12;i++){
      //number--set对象 不会重复 保持两位，没有两位就补充两位
      this.number.add((''+i).padStart(2,'0'))
    }
  }
  
  /**
   * [setOmit 设置遗漏数据]
   * @param {[type]} omit [description]
   */
  setOmit(omit){
  	//保存当前对象引用
    let self=this;
    //omit--map对象 先清空
    self.omit.clear();
    for(let [index,item] of omit.entries()){
    //重新赋值  因为每10分钟赋值
      self.omit.set(index,item)
    }
    $(self.omit_el).each(function(index,item){
      $(item).text(self.omit.get(index))
    });
  }
  /**
   * [setOpenCode 设置开奖]
   * @param {[type]} code [description]
   */
  setOpenCode(code){
    let self=this;
    self.open_code.clear();
    for(let item of code.values()){
    	//open_code set结构
      self.open_code.add(item);
    }
    //更新获奖接口
    self.updateOpenCode&&self.updateOpenCode.call(self,code);
  }

  /**
   * [toggleCodeActive 号码选中取消]
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  toggleCodeActive(e){
    let self=this;
    //获取选中对象
    let $cur=$(e.currentTarget);
    $cur.toggleClass('btn-boll-active');
    self.getCount();
  }
  
  /**
   * [changePlayNav 切换玩法]
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  changePlayNav(e){
    let self=this;
    let $cur=$(e.currentTarget);
    $cur.addClass('active').siblings().removeClass('active');
    self.cur_play=$cur.attr('desc').toLocaleLowerCase();
    $('#zx_sm span').html(self.play_list.get(self.cur_play).tip);
    $('.boll-list .btn-boll').removeClass('btn-boll-active');
    self.getCount();
  }
  
    /**
   * [assistHandle 操作区] 大小奇偶 全选
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  assistHandle(e){
  	//阻止默认事件
    e.preventDefault();
    let self=this;
    let $cur=$(e.currentTarget);
    let index=$cur.index();
    $('.boll-list .btn-boll').removeClass('btn-boll-active');
    if(index===0){
      $('.boll-list .btn-boll').addClass('btn-boll-active');
    }
    if(index===1){
      $('.boll-list .btn-boll').each(function(i,t){
      	//textContent 属性设置或返回指定节点的文本内容，以及它的所有后代
        if(t.textContent-5>0){
          $(t).addClass('btn-boll-active')
        }
      })
    }
    if(index===2){
      $('.boll-list .btn-boll').each(function(i,t){
        if(t.textContent-6<0){
          $(t).addClass('btn-boll-active')
        }
      })
    }
    if(index===3){
      $('.boll-list .btn-boll').each(function(i,t){
        if(t.textContent%2==1){
          $(t).addClass('btn-boll-active')
        }
      })
    }
    if(index===4){
      $('.boll-list .btn-boll').each(function(i,t){
        if(t.textContent%2==0){
          $(t).addClass('btn-boll-active')
        }
      })
    }
    self.getCount();
  }
  
  
    /**
   * [getName 获取当前彩票名称]
   * @return {[type]} [description]
   */
  getName(){
    return this.name
  }
  
  /**
   * [addCode 添加号码]
   */
  addCode(){
    let self=this;
    //当前号码文本的值
    let $active=$('.boll-list .btn-boll-active').text().match(/\d{2}/g);
    //如果值存在就取它的长度，否则就取0
    let active=$active?$active.length:0;
    let count=self.computeCount(active,self.cur_play);
    if(count){
      self.addCodeItem($active.join(' '),self.cur_play,self.play_list.get(self.cur_play).name,count);
    }
  }
  
  /**
   * [addCodeItem 添加单次号码]
   * @param {[type]} code     [description]
   * @param {[type]} type     [description]
   * @param {[type]} typeName [description]
   * @param {[type]} count    [description]
   */
  addCodeItem(code,type,typeName,count){
    let self=this;
    const tpl=`
    <li codes="${type}|${code}" bonus="${count*2}" count="${count}">
		 <div class="code">
			 <b>${typeName}${count>1?'复式':'单式'}</b>
			 <b class="em">${code}</b>
			 [${count}注,<em class="code-list-money">${count*2}</em>元]
		 </div>
	 </li>
    `;
    $(self.cart_el).append(tpl);
    self.getTotal();
  }

  getCount(){
    let self=this;
    let active=$('.boll-list .btn-boll-active').length;
    let count=self.computeCount(active,self.cur_play);
    let range=self.computeBonus(active,self.cur_play);
    let money=count*2;
    let win1=range[0]-money;
    let win2=range[1]-money;
    let tpl;
    //判断盈利
    let c1=(win1<0&&win2<0)?Math.abs(win1):win1;
    let c2=(win1<0&&win2<0)?Math.abs(win2):win2;
    if(count===0){
      tpl=`您选了 <b class="red">${count}</b> 注，共 <b class="red">${count*2}</b> 元`
    }else if(range[0]===range[1]){
      tpl=`您选了 <b>${count}</b> 注，共 <b>${count*2}</b> 元  <em>若中奖，奖金：
			<strong class="red">${range[0]}</strong> 元，
			您将${win1>=0?'盈利':'亏损'}
			<strong class="${win1>=0?'red':'green'}">${Math.abs(win1)} </strong> 元</em>`
    }else{
      tpl=`您选了 <b>${count}</b> 注，共 <b>${count*2}</b> 元  <em>若中奖，奖金：
			<strong class="red">${range[0]}</strong> 至 <strong class="red">${range[1]}</strong> 元，
			您将${(win1<0&&win2<0)?'亏损':'盈利'}
			<strong class="${win1>=0?'red':'green'}">${c1} </strong>
			至 <strong class="${win2>=0?'red':'green'}"> ${c2} </strong>
			元</em>`
    }
    $('.sel_info').html(tpl);

  }


  /**
   * [getTotal 计算所有金额]
   * @return {[type]} [description]
   */
  getTotal(){
    let count=0;
    $('.codelist li').each(function(index,item){
      count+=$(item).attr('count')*1;
    })
    $('#count').text(count);
    $('#money').text(count*2);
  }

  /**
   * [getRandom 生成随机数]
   * @param  {[type]} num [description]
   * @return {[type]}     [description]
   */
  getRandom(num){
  	//保存临时变量
    let arr=[],index;
    //把类数组转换成真正数组
    let number=Array.from(this.number);
    while(num--){
    	//指定1-11号码内
      index=Number.parseInt(Math.random()*number.length);
      arr.push(number[index]);
      number.splice(index,1);
    }
    return arr.join(' ')
  }

  /**
   * [getRandomCode 添加随机号码]
   * @param  {[type]} e [description]
   * @return {[type]}   [description]
   */
  getRandomCode(e){
    e.preventDefault();
    let num=e.currentTarget.getAttribute('count');
    let play=this.cur_play.match(/\d+/g)[0];
    let self=this;
    if(num==='0'){
      $(self.cart_el).html('')
    }else{
      for(let i=0;i<num;i++){
        self.addCodeItem(self.getRandom(play),self.cur_play,self.play_list.get(self.cur_play).name,1);
      }
    }
  }
}

export default Base
```

#### lottery整合模块

```
import 'babel-polyfill';
import Base from './lottery/base.js';
import Timer from './lottery/timer.js';
import Calculate from './lottery/calculate.js';
import Interface from './lottery/interface.js';
import $ from 'jquery';

//深度拷贝
const copyProperties=function(target,source){
	//Reflect.ownKeys拿源对象
  for(let key of Reflect.ownKeys(source)){
    if(key!=='constructor'&&key!=='prototype'&&key!=='name'){
      let desc=Object.getOwnPropertyDescriptor(source,key);
      //复制
      Object.defineProperty(target,key,desc);
    }
  }
}

//多重继承
const mix=function(...mixins){
  class Mix{}
  for(let mixin of mixins){
    copyProperties(Mix,mixin);
    copyProperties(Mix.prototype,mixin.prototype);
  }
  return Mix
}

//多重继承
class Lottery extends mix(Base,Calculate,Interface,Timer){
	//构造函数
  constructor(name='syy',cname='11选5',issue='**',state='**'){
    super();
    this.name=name;
    this.cname=cname;
    this.issue=issue;
    this.state=state;
    this.el='';
    this.omit=new Map();
    this.open_code=new Set();
    this.open_code_list=new Set();
    this.play_list=new Map();
    this.number=new Set();
    this.issue_el='#curr_issue';
    this.countdown_el='#countdown';
    this.state_el='.state_el';
    this.cart_el='.codelist';
    this.omit_el='';
    this.cur_play='r5';
    this.initPlayList();
    this.initNumber();
    this.updateState();
    this.initEvent();
  }
  

  /**
   * [updateState 状态更新]
   * @return {[type]} [description]
   */
  updateState(){
    let self=this;
    //异步操作
    this.getState().then(function(res){
      self.issue=res.issue;
      self.end_time=res.end_time;
      self.state=res.state;
      //更新当前期号
      $(self.issue_el).text(res.issue);
      self.countdown(res.end_time,function(time){
      	//倒计时更新
        $(self.countdown_el).html(time)
      },function(){
        setTimeout(function () {
          self.updateState();
          self.getOmit(self.issue).then(function(res){

          });
          self.getOpenCode(self.issue).then(function(res){

          })
        }, 500);
      })
    })
  }
  

  /**
   * [initEvent 初始化事件]
   * @return {[type]} [description]
   */
  initEvent(){
    let self=this;
    $('#plays').on('click','li',self.changePlayNav.bind(self));
    $('.boll-list').on('click','.btn-boll',self.toggleCodeActive.bind(self));
    $('#confirm_sel_code').on('click',self.addCode.bind(self));
    $('.dxjo').on('click','li',self.assistHandle.bind(self));
    $('.qkmethod').on('click','.btn-middle',self.getRandomCode.bind(self));
  }
}

export default Lottery;
```

index.js

```
import 'babel-polyfill';
import Lottery from './lottery';

const syy=new Lottery();
```



## 