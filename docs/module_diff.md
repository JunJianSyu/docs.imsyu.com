> Author: JunJianSyu <br />
> Slogan: Stay focused and keep shipping.

## JavaScript AMD、CommonJS、ES  Harmony

首先说下模块化这个概念，最初AMD模块化开发出来时是为了高度分离功能模块，在最初的JavaScript迭代(ECMA-262)是无法为开发人员提供干净整齐的方式导入代码模块的方法。
> AMD 依赖前置执行，2.0版本也支持延迟执行。CMD 是延迟执行，就近依赖。<br/>
> CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。<br/>
> CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

### AMD 一种在浏览器宿主中编写模块化的方式

AMD(Asynchronous Module Definition) 它最初源于Dojo的XHR+eval

```javascript
define(
    module_id /*模块名 id*/, 
    [dependencies] /*依赖项*/, 
    definition /* 函数方法返回module或者object暴露*/
);
// 动态加载定义模块
define(function(require) {
  require([a, b], function(a, b) {
    content = a + b
  })
  return {
      content: content
  }
})

// require
require([a, b], function(a, b) {
  // code
})

// 依赖模块延迟
define([Deferred], function(Deferred) {
  var defer = new Deferred()
  require(['a', 'b'], function(a, b) {
    defer.resolve({a: a, b: b})
  })
  return defer.promise();
})
```


### CommonJS 一种在服务端的规范 Nodejs

```javascript
var lib = require('package/lib');
function foo() {
  // no code
}
exports.foo = foo;
```

### ES 在宿主对象上使用转换或者支持ES新语法

```typescript
import a from 'a.js'
function foo() {
  // no code
}
export default {foo}
```
