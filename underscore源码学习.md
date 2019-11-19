### 1.为什么用「void 0」代替「undefined」？

第一点，答案很简单，undefined 并不是保留词（reserved word），它只是全局对象的一个属性，在低版本 IE 中能被重写，在局部作用域中，还是可以被重写的，void 是不能被重写的。

此外，用 void 0 代替 undefined 能节省不少字节的大小，事实上，不少 JavaScript 压缩工具在压缩过程中，正是将 undefined 用 void 0 代替掉了 。

### 2.常用类型判断以及一些有用的工具方法

#### 判断一个元素是否是 DOM 元素

保证它不为空，且 nodeType 属性为 1:

```
// Is a given value a DOM element?
// 判断是否为 DOM 元素
_.isElement = function(obj) {
  // 确保 obj 不是 null 
  // 并且 obj.nodeType === 1
  return !!(obj && obj.nodeType === 1);
};
```
#### 判断数组

- 用typeof来检查该变量，不论是array还是object，都将返回‘object' 
- 跨frame实例化的对象彼此是不共享原型链的 ，instanceof或者constructor失效
- 为什么不直接o.toString()?嗯，虽然Array继承自Object，也会有 toString方法，但是这个方法有可能会被改写而达不到我们的要求，而Object.prototype则能一定程度保证其“纯洁性” 

ps：在IE8之前的版本是不支持的 Array.isArray()（es5）用于确定传递的值是否是一个 Array 

```
function isArray(a) {
  Array.isArray ? Array.isArray(a) : Object.prototype.toString.call(a) === '[object Array]';
}

或者
function isArrayFn(value){ 
    if (typeof Array.isArray === "function") { 
        return Array.isArray(value); 
    }else{ 
        return Object.prototype.toString.call(value) === "[object Array]"; 
    } 
} 
```

####  判断对象

```
//1.判断是否为对象
// 这里的对象包括 function 和 object
_.isObject = function(obj) {
  var type = typeof obj;
  return type === 'function' || type === 'object' && !!obj;
};
//2.其他类型判断
_.each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp', 'Error'], function(name) {
  _['is' + name] = function(obj) {
    return toString.call(obj) === '[object ' + name + ']';
  };
});
//3. _.isArguments 方法在 IE < 9 下的兼容
// IE < 9 下对 arguments 调用 Object.prototype.toString.call 方法
// 结果是 [object Object]
// 而并非我们期望的 [object Arguments]。
// so 用是否含有 callee 属性来判断   arguments.callee 能返回当前 arguments 所在的函数
if (!_.isArguments(arguments)) {
  _.isArguments = function(obj) {
    return _.has(obj, 'callee');
  };
}
```

