### 自学vue项目笔记

#### 初始化项目

**vue-cli2.0**

初始化一个项目 vue init webpack  xxx

npm run dev == npm start运行

**vue-cli3.0** 

初始化一个项目vue create xxx

也可以用vue ui

```
(1. babel
(2. TypeScript  
(3. Progressive Web App (PWA) Support  支持渐进式网页应用程序
(4. Router 路由管理器
(5. Vuex 状态管理模式（构建一个中大型单页应用时）
(6. CSS Pre-processors css预处理
(7. Linter / Formatter 代码风格、格式校验
(8. Unit Testing 单元测试
(9. E2E Testing E2E（End To End）即端对端测试
一般选择如下1456
并且使用配置文件，保存在各自的配置文件
6选择less或者stylus
7选择eslint+standard config
Lint on save 保存时检查
不保存预设
```

google浏览器下载vue-detools配合使用

启动用yarn serve

安装插件 yarn add xxx

其他用法和npm差不多，具体看文档



ps：一个坑

在安装vue-cli3.x版本时，node版本>8.9

一定要删除cli2.x版本并把npm缓存和module这些包都删了

重新装一下yarn



#### 打包发布 

**vue-cli2.0**

npm run build打包  生成的文件在dist下

npm i -g serve         serve dist

config里 index.js里是运行相关配置



如果你希望你打包生成的文件放在某一个文件夹下再给后台 

在config  index.js中

会看到一个`build`打包部分的配置项，找到`assetPublicPath`，改为‘`/project`’ 

然后把dist改名为project（项目名）



webpack打包后 有css和js文件 source.map是辅助性文件

manifest.js是配置文件

vendor.js是各个组件、页面公用的代码

chunk-vendors.js放的是import或第三方库的一些js

前面两个是在开发环境中并没有

app.js放的是业务逻辑代码，自己依赖、开发的模块

打包后是hash的方式，有本地缓存的作用



**vue-cli3.0新增**

package.json里有可视化打包report可以生成界面展示

"report": "vue-cli-service build --report" 

命令npm run report



优化 上下文替换插件 ContextReplacementPlugin
配置plugin<https://github.com/Yatoo2018/webpack-chain/tree/zh-cmn-Hans> 

npm install -g --g @vue/cli@xx --registry=https://registry.npm.taobao.org

一定要删除cli2.x版本并把npm缓存和module这些包都删了

你可以使用 `vue-cli-service inspect` 来审查一个 Vue CLI 项目的 webpack config 
命令 vue inspect >> xx.js

启动一个node server去打开打包好的index.html
python -m SimpleHTTPServer 8080


关于图片：
public用require引入或者import是base64格式
一般项目里用import引入....后续再研究图片的打包路径问题...


#### 目录结构 

**public下**
favicon.ico是网址上方的标题的小图标。
index.html：是入口文件模板。

**editorconfig** 

最外层新建.editorconfig 搭配VScode插件**EditorConfig for VS Code**配置编辑器习惯，有了这个插件配置才会起作用

```
#开启
root = true
#所有文件都有效
[*]
charset = utf-8
#缩进
indent_style = space/tabs
#缩进尺寸
indent_size = 2
.....
```

**vue.config.js**

根文件目录下vue.config.js

```
//引入node path模块
const path = require('path')
const resolve = dir => path.join(__dirname, dir)
//项目基本路径 如果是生产环境 具体路径  如果是开发环境 / 
const BASE_URL = process.env.NODE_ENV === 'production' ? '/iview-admin/' : '/'
module.exports = {
  lintOnSave: true,
  baseUrl: BASE_URL,
  // vuecli3.0配置相对路径
  chainWebpack: config => {
    config.resolve.alias
      //表示当前路由拼接上了src
      .set('@', resolve('src'))
      .set('_c', resolve('src/components')
  },
  //打包时不生成map文件 可以减少打包体积
  production:false，
  //跨域  写下需要代理的url
  devServer: {
    proxy: "http://localhost:888"
  }
}
```

**src目录下**

新建config配置文件夹，这里面可以放各种配置文件

例如新建index.js
在其他文件夹引入import config from './config'即可引入

**api 交互**

common/assets 静态资源文件夹 font/img/stylus

components 通用组件

filters自定义过滤器

direction 自定义指令

mock 模拟数据
下载mockjs --save-dev

pages /views页面组件

lib/util.js文件夹下分类放与自己项目有关的业务方法

lib/tools.js放通用方法或者说工具方法

mock 模拟数据

pages/views 页面

store文件夹 vuex相关
文件夹下mutations.js、actions.js、state.js、index.js分类
再分模块modules，比如用户模块，新建user.js 定义state、getters、mutations、actions

router文件夹下放路由文件，抽离出index.js和router.js
router.js放路由列表，index.js下去做一些路由拦截并引入路由列表

App.vue 应用组件

main.js入口

eslint规则
https://blog.csdn.net/weixin_41767649/article/details/90115453
规则可以写在package.json里或者单独写.eslintrc.js
例如"rules": {"space-before-function-paren": ["error", "never"]}

修复eslint报错
package.json修改为：（中间插入–fix）
lint": "eslint --fix --ext .js,.vue src test/unit"，
2、终端运行npm run lint修改代码样式

#### 项目准备

stylus、less、sass需要下载依赖包

移动端

1.添加视口 在index.html

2.300ms延迟

网易云、饿了么项目：

import fastclick from 'fastclick' 

fastclick.attach(document.body) 

3.1px变框问题 

- 去哪儿网main.js直接引用的border.css
- cube-ui、minit-ui有封装好的方法
- 伪类解决 新建mixin.styl 
- 淘宝flexble也有解决

```
// 1px
border-1px($color)
  position relative
  &:after
    display block
    position absolute
    left 0
    bottom 0
    width 100%
    border-top 1px solid $color
    content ' '
 
// 无边框
border-none()
  &:after
    display none
    
.text{
    border-1px(red);
}

<span class="text border-1px">
      <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>            
      {{item.name}}
</span>
```

4.适配

<https://blog.csdn.net/qq_29247935/article/details/72478262> 

- 一般js基准值设置rem+flex

基准值100 根节点html50px

1rem=100px

![](.\img\19.JPG)

- flexble方案


flexble元素用rem，字体用px，用dpr属性设置不同dpr下字体大小

或者flexble+px2rem 

直接写px，编译后会直接转化成rem ---- 

除开下面两种情况，

其他长度用这个 在px后面添加/*no*/，不会转化为rem，会原样输出px。 --- 一般border需用这个 

在px后面添加/*px*/,会根据dpr的不同，生成三套代码。---- 一般字体需用这个 

<https://www.cnblogs.com/wenxinsj/p/10184982.html> 

之所以使用px2rem(vscode插件)而不使用px2rem-loader是原因是px2rem-loader会把ui框架的样式也一并转成rem,会导致样式变形 （flexible不编译第三方库的文件 ）

  解决方案2excluede:/node_modules/<https://www.cnblogs.com/skylineStar/p/10036525.html> 

与cube-ui样式冲突<https://blog.csdn.net/qq_34470541/article/details/90518930> 

- vw方案<https://www.jianshu.com/p/1f1b23f8348f> 


5.路径

cli2.0

build目录下：webpack.base.conf.js

webpack.dev.conf.js

改为config/index.js中进行数据配置 

cli3.0

vue.config.js里配置搭配webpack4



ps：flex布局左右布局

display: flex;

-webkit-box-align: stretch;

-ms-flex-align: stretch;

align-items: stretch;




经常用到的flex布局

	{
	    display: flex;
	
	    flex-direction: column;
	
	    align-items: center;
	
	    justify-content: center
	
		{
			flex:1   //所有的子元素平均分配
	 	}
	
	}
   

#### 关于路由 router 

在main.js new Vue中如果去配了它 有以下作用 

多了几个标签

keep-alive 

router-link（跳转）  相当于a标签

router-view（显示，这个可以为单标签）视图渲染

多了两个属性可以访问 $router 和 $route

- 如果是param则动态路由必须带上参数

  如果是query则不用带

- 如果是路由视图，就有父子路由，页面的区分比如主页就这样靠路由区分

##### 动态路由匹配

#### tab组件

：表示动态

![](.\vue-img\1.JPG)

this.$router.replace() 

##### linkActiveClass 两种方式

![1566485425461](.\vue-img\25.JPG)

![或者](.\vue-img\26.JPG)

#### 公共组件

slot占位  不同slot通过name属性识别

![](.\vue-img\3.JPG)

用组件：引入、注册、使用标签

![](.\vue-img\2.JPG)

#### swiper组件

在适用页去引入  或者在main.js全局引入

![](.\vue-img\6.JPG)

container包含两部分东西 wrapper和pagination

![](.\vue-img\4.JPG)

保证new Swiper时页面已经渲染了  用mounted

![](.\vue-img\5.JPG)



#### 路由组件

登录 返回上一级$router.back()

当前 路由器 $route（对象）

当前 路由 $route（对象）

如果给路由添加元素域meta （对象）



![](.\vue-img\7.JPG)

空对象就为false，就不加

![](.\vue-img\8.JPG)

- [x] v-show表示有这个组件 只是不显示

  

#### 封装axios

npm i --save axios

return new Promise是因为不同promise返回数据不一样   相当于是在axios外面再去返回response.data 简化外部调用

异步返回的数据是response.data而不是response

![](.\vue-img\9.JPG)

![](.\vue-img\10.JPG)



封装接口请求函数

param参数 /xx/这种形式

query ？id=？这种形式

一个参数传两个 用对象 两个属性

![](.\vue-img\11.JPG)



#### 配置代理（跨域）

去哪儿网和此项目都是在config/index.js下去配置代理

服务器端去“欺骗浏览器端”

![](.\vue-img\13.JPG)

![](.\vue-img\12.JPG)

##### async函数

变成 ==》》async函数（异步）返回一个 Promise 对象，可以使用then方法添加回调函数。**当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句** 

![](.\vue-img\14.JPG)

1、await必须在async里面

2、async作用于函数

3、async返回一个promise对象

遇到了await, await 表示等一下，代码就暂停到这里，不再向下执行了，它等什么呢？等后面的promise对象执行完毕，然后拿到promise resolve 的值并进行返回，返回值拿到之后，它继续向下执行 



易混淆：created:在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。

mounted:在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。



#### vuex（为了更好异步显示）

安装vuex

核心管理对象 store

状态对象  state

操作对象 actions 通过mutations间接更新多个方法的对象

直接更新state的多个方法的对象 mutations

基于state的getter计算属性的对象 getters

包含N个mutation的type名称常量mutation-types

![](.\vue-img\22.JPG)

引入模块并配置 声明使用 index.js

![](.\vue-img\14.JPG)

1.state.js

![](.\vue-img\15.JPG)

2.mutation-type.js

![](.\vue-img\16.JPG)

3.mutations.js

![](.\vue-img\17.JPG)

需要接收的是包含数据和传递数据的对象！

4.actions.js

![](.\vue-img\18.JPG)

![](.\vue-img\19.JPG)

![](.\vue-img\20.JPG)

![](.\vue-img\21.JPG)

##### 封装接口请求函数里获取接口方法

dispatch=》commit

2种方式 dispatch或者映射函数（这个是在app.vue中，地址是全局信息  在mouted去发请求获取）

![](.\vue-img\23.JPG)

![](.\vue-img\24.JPG)

总结：1.在methods  中mapActions（实际就是触发dispatch）调用action去发起请求获取数据

间接的方式 在actions.js中去发ajax请求获取result.data==去commit给mutation的值，再在mutation里去更新状态，数据存到vuex的state中  2.读状态mapState（在computed里去读取） 3.渲染

#### 权限控制

简单权限控制

iview-admin

 https://blog.csdn.net/qq_43436432/article/details/84374700 

 canTurnTo会通过第二个参数（用户的权限字段列表）进行匹配，如果当前页面，当前用户是有权限的，显示当前页面，否则跳转到401页面 
component，代表组件级别的路由权限  为true，代表可以访问这个页面
为false代表访问不了
这种方式有一个弊端，路由实例里每一个路由都要有一个name
并且不能和path重复

 进行权限过滤时候，过滤的就是routerMap数组，匹配不到直接显示404页面 


#### git命令

密钥配对<https://blog.csdn.net/u013778905/article/details/83501204> 

1.克隆到本地 git clone  xxx

2.创建本地仓库 git init

git status 用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。
git add . 把代码添加到Git的缓冲区
git commit -m 'xxx' 提交代码并编写日志

创建远程仓库（在github上）

先关联 git remote add origin < remote-project-repository-address > 

比如git remote add origin https://github.com/heqiliao007/xxx

再推送

git push origin master提交代码至线上

	//多人开发 
	git branch //查看分支
	git checkout -b xxx新创建一个分支
	git checkout xxx 切换到某个分支  
	
	git diff //查看不同
	git add .
	git commit -m "提交信息"//提交信息
	git push origin xxx分支 //提交到远程xxx分支
	
	git checkout master // 切换到master分支
	git pull origin master//先拉取一遍别人可能更新了
	
	git merge origin/xxx分支 // 把xxx分支上新增的内容合并到master分支上
	git push // 把master分支的内容提交到线上git push origin master
	
	//git 如何在A分支merge B分支的单个文件
	git checkout -p B file.txt
	
	git reset --soft HEAD^ //撤销本次提交，将本地仓库回滚到上一个版本，工作区和暂存区不变。
	HEAD^表示上个提交版本，HEAD^^表示上上个提交版本，HEAD~n代表前面第n个版本
gitbash 不能复制粘贴的使用快捷键 Ctrl/Shift+Insert 
