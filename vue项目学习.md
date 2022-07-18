## vue项目笔记

### 创建项目

#### 1.初始化项目

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



#### 2.打包发布 

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


#### 3.目录结构 

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

#### 4.项目准备

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

### vue组件相关

#### 1. 函数式组件 

Vue.js 组件提供了一个 functional 开关，设置为 true 后，就可以让组件变为无状态、无实例的函数化组件。因为只是函数，所以渲染的开销相对来说，较小。
函数化的组件中的 Render 函数，提供了第二个参数 context 作为上下文，data、props、slots、children 以及 parent 都可以通过 context 来访问

```
export default {
    functional: true,
    props: {
        name: String, // 组件渲染的文字内容
        renderFunc: Function // 传入的render函数
    },
    /**
     * render渲染函数
     * @param {Function} h - render函数的回调方法，用于生成dom节点
     * @param {Object} ctx - 指代当前的这个对象
     */
    render: (h, ctx) => {
        return ctx.props.renderFunc(h, ctx.props.name)
    }
}
```

### 登陆/登出以及JWT认证

https://www.kancloud.cn/wangjiachong/vue_notes/2033892

### 权限控制

简单权限控制

iview-admin

 https://blog.csdn.net/qq_43436432/article/details/84374700 

 canTurnTo会通过第二个参数（用户的权限字段列表）进行匹配，如果当前页面，当前用户是有权限的，显示当前页面，否则跳转到401页面 
component，代表组件级别的路由权限  为true，代表可以访问这个页面
为false代表访问不了
这种方式有一个弊端，路由实例里每一个路由都要有一个name
并且不能和path重复

 进行权限过滤时候，过滤的就是routerMap数组，匹配不到直接显示404页面 


