> Author: JunJianSyu <br />
> Slogan: Asynchronous magic.

!> 注: 此文章内容为笔者结合互联网众多关于`Promise`文章,综合而来.

## Promise的历史
`Promise`是一个很古老的概念,最初提议与(1976年)通常与`future`结合在一起。`Future`指的是未来的值，通常在`Promise`里被作为参数和返回值传来传去(但是在有的语境下`Future`又被用来指代类似`Promise`的东西。下文中，我们所说的`future`表示第一种概念).

`Promise`只是一个概念，很多常见语言的标准库都有`Promise`关联的特性(比如 C++ 11 的 std::promise 和 std::future、Java 8 的 java.util.concurrent.Future、Python 3.2+ 的 concurrent.futures等).即使标准库里没有，大部分语言里也有第三方实现的 Promise（比如 Ruby 的 Promise gem）。这里主要讨论`JavaScript`，特别是 ECMAScript 2015 中的`Promise`.

最早得到广泛使用的`Promise`是`jQuery`的`AJAX`模块中出现的`jQuery.Deferred()`.`Promise/A+`标准规定了一系列 API，并配有大量的测试用例，ECMAScript 2015 直接整合了这个标准.

`Promise/A+`提供的`API`非常有限，很多`Promise`库(Q、bluebird 等)在兼容`Promise/A+`的基础上添加了一些其他的扩展。`jQuery 3.0`也已经将`Deferred`迁移成了与`Promise/A+`兼容.

#### jQuery deferred

首先来说说jquery 1.5 之后新加的deferred方法, `$.Deferred()`会返回一个`deferred`对象

```javascript
function waitHandle() {
    var dtd = $.Deferred()  // 创建一个 deferred 对象
    var wait = function (dtd) {  // 要求传入一个 deferred 对象
        var task = function () {
            console.log('执行完成')
            dtd.resolve()  // 表示异步任务已经完成
        }
        setTimeout(task, 2000)
        return dtd  // 要求返回 deferred 对象
    }
    // 注意，这里一定要有返回值
    return wait(dtd)
}

// 接着上面的代码继续写
var w = waitHandle()
w.then(function () {
    console.log('ok 1')
}, function () {
    console.log('err 1')
}).then(function () {
    console.log('ok 2')
}, function () {
    console.log('err 2')
})

// result
// 执行完成
// ok 1
// ok 2
```

`waitHandle`函数最终返回一个`deferred`对象，而`deferred`对象具有`done fail then`方法，现在我们正在使用的是`then`方法. </br>
`dtd.resolve()`表示革命已经成功，会触发then中第一个参数（函数）的执行  </br>
`dtd.reject()`表示革命失败了，会触发then中第二个参数（函数）执行

#### jQuery promise

修改上面的例子
```javascript
function waitHandle() {
    var dtd = $.Deferred()
    var wait = function (dtd) {
        var task = function () {
            console.log('执行完成')
            dtd.resolve()
        }
        setTimeout(task, 2000)
        return dtd.promise()  // 注意，这里返回的是 promise 而不是直接返回 deferred 对象
    }
    return wait(dtd)
}

var w = waitHandle() // 经过上面的改动，w 接收的就是一个 promise 对象
$.when(w)
    .then(function () {
        console.log('ok 1')
    })
    .then(function () {
        console.log('ok 2')
    })
```

改动的一行在这里`return dtd.promise()`，之前是`return dtd`. `dtd`是一个`deferred`对象，而`dtd.promise`就是一个`promise`对象. </br>
`promise`对象和`deferred`对象最重要的区别，记住了`promise`对象相比于`deferred`对象，缺少了`.resolve`和`.reject`这俩函数属性.

返回promise的好处: </br>
`waitHandle`函数内部，使用`dtd.resolve()`来该表状态，做主动的修改操作 <br/>
`waitHandle`最终返回`promise`对象，只能去被动监听变化（then函数），而不能去主动修改操作

#### Promise加入ES6标准

在ES6中 promise 如何使用

```javascript
const wait =  function () {
    // 定义一个 promise 对象
    const promise = new Promise((resolve, reject) => {
        // 将之前的异步操作，包括到这个 new Promise 函数之内
        const task = function () {
            console.log('执行完成')
            resolve()  // callback 中去执行 resolve 或者 reject
        }
        setTimeout(task, 2000)
    })
    // 返回 promise 对象
    return promise
}
```
wait函数中 创建 promise的实例 直接返回, Promise构造函数内 执行setTimeout 来改变状态.

```javascript
const w = wait()
w.then(() => {
    console.log('ok 1')
}, () => {
    console.log('err 1')
}).then(() => {
    console.log('ok 2')
}, () => {
    console.log('err 2')
})
```

`then`还是接收2个参数,一个成功(resolve)后执行, 一个失败(reject)后执行.

接下来看一个具体promise的应用, 读取文件 io文件的promise 封装操作

```javascript
const fs = require('fs')
const path = require('path')  // 后面获取文件路径时候会用到
const readFilePromise = function (fileName) {
    return new Promise((resolve, reject) => {
        fs.readFile(fileName, (err, data) => {
            if (err) {
                reject(err)  // 注意，这里执行 reject 是传递了参数，后面会有地方接收到这个参数
            } else {
                resolve(data.toString())  // 注意，这里执行 resolve 时传递了参数，后面会有地方接收到这个参数
            }
        })
    })
}

const fullFileName = path.resolve(__dirname, '../data/data2.json')
const result = readFilePromise(fullFileName)
result.then(data => {
    // 第一步操作
    console.log(data)
    return JSON.parse(data).a  // 这里将 a 属性的值 return
}).then(a => {
    // 第二步操作
    console.log(a)  // 这里可以获取上一步 return 过来的值
})
```

总结一下，这一段内容提到的“参数传递”其实有两个方面：</br>
执行`resolve`传递的值，会被第一个then处理时接收到</br>
如果`then`有链式操作，前面步骤返回的值，会被后面的步骤获取到

#### 异常捕获
可以看出来上面的例子在then中 我们都没有传第二个参数 reject 失败捕获异常

```javascript
const fullFileName = path.resolve(__dirname, '../data/data2.json')
const result = readFilePromise(fullFileName)
result.then(data => {
    console.log(data)
    return JSON.parse(data).a
}).then(a => {
    console.log(a)
}).catch(err => {
    console.log(err.stack)  // 这里的 catch 就能捕获 readFilePromise 中触发的 reject ，而且能接收 reject 传递的参数
})
```

在若干个then串联之后，我们一般会在最后跟一个.catch来捕获异常，而且执行reject时传递的参数也会在catch中获取到。这样做的好处是：</br>
让程序看起来更加简洁，是一个串联的关系，没有分支（如果用then的两个参数，就会出现分支，影响阅读）</br>
看起来更像是try - catch的样子，更易理解

#### 串联异步操作
需求是先读取json2文件,读取完毕之后再去读取json1文件

```javascript
const fullFileName2 = path.resolve(__dirname, '../data/data2.json')
const result2 = readFilePromise(fullFileName2)
const fullFileName1 = path.resolve(__dirname, '../data/data1.json')
const result1 = readFilePromise(fullFileName1)

result2.then(data => {
    console.log('data2.json', data)
    return result1  // 此处只需返回读取 data1.json 的 Promise 即可
}).then(data => {
    console.log('data1.json', data) // data 即可接收到 data1.json 的内容
})
```
链式传递的操作, `return result1`返回的promise对象 可直接使用`then`方法

#### Promise.all和Promise.race的应用

```javascript
const fullFileName2 = path.resolve(__dirname, '../data/data2.json')
const result2 = readFilePromise(fullFileName2)
const fullFileName1 = path.resolve(__dirname, '../data/data1.json')
const result1 = readFilePromise(fullFileName1)

// Promise.all 接收一个包含多个 promise 对象的数组
Promise.all([result1, result2]).then(datas => {
    // 接收到的 datas 是一个数组，依次包含了多个 promise 返回的内容
    console.log(datas[0])
    console.log(datas[1])
})

// Promise.race 接收一个包含多个 promise 对象的数组
Promise.race([result1, result2]).then(data => {
    // data 即最先执行完成的 promise 的返回值
    console.log(data)
})
```

#### Promise.resolve的应用

Promise.resolve方法能将具有`thenable`的对象转换成`Promise`对象

```javascript
// 在浏览器环境下运行，而非 node 环境
cosnt jsPromise = Promise.resolve($.ajax('/whatever.json'))
jsPromise.then(data => {
    // ...
})
```

这个例子中`ajax`会返回一个`deferred`对象,而这个`deferred`对象就是一个`thenable`对象

```javascript
// 定义一个 thenable 对象
const thenable = {
    // 所谓 thenable 对象，就是具有 then 属性，而且属性值是如下格式函数的对象
    then: (resolve, reject) => {
        resolve(200)
    }
}

// thenable 对象可以转换为 Promise 对象
const promise = Promise.resolve(thenable)
promise.then(data => {
    // ...
})
```

其实异步最好用的地方 就是将一些异步操作方法 封装成promise对象 (如fs.readFile）转换为`Promise`, 这样我们就可以在文件读取之后做一些相应的操作了

#### ES6 中的 Generator

接下来我们来看一下 Generator 如何处理链式操作

先来一段最基础的Generator代码
```javascript
function* Hello() {
    yield 100
    yield (function () {return 200})()
    return 300
}

var h = Hello()
console.log(typeof h)  // object

console.log(h.next())  // { value: 100, done: false }
console.log(h.next())  // { value: 200, done: false }
console.log(h.next())  // { value: 300, done: true }
console.log(h.next())  // { value: undefined, done: true }
```

* Generator不是函数，不是函数，不是函数 可以把它理解为一个(生成器)
* Hello()不会立即出发执行，而是一上来就暂停
* 每次h.next()都会打破暂停状态去执行，直到遇到下一个yield或者return
* 遇到yield时，会执行yeild后面的表达式，并返回执行之后的值，然后再次进入暂停状态，此时done: false。
* 遇到return时，会返回值，执行结束，即done: true
* 每次h.next()的返回值永远都是{value: ... , done: ...}的形式

接下来来看看 Generator 如何处理链式操作

```javascript
// promise方式
readFilePromise('some1.json').then(data => {
    console.log(data)  // 打印第 1 个文件内容
    return readFilePromise('some2.json')
}).then(data => {
    console.log(data)  // 打印第 2 个文件内容
    return readFilePromise('some3.json')
}).then(data => {
    console.log(data)  // 打印第 3 个文件内容
    return readFilePromise('some4.json')
}).then(data=> {
    console.log(data)  // 打印第 4 个文件内容
})

// generator方式
co(function* () {
    const r1 = yield readFilePromise('some1.json')
    console.log(r1)  // 打印第 1 个文件内容
    const r2 = yield readFilePromise('some2.json')
    console.log(r2)  // 打印第 2 个文件内容
    const r3 = yield readFilePromise('some3.json')
    console.log(r3)  // 打印第 3 个文件内容
    const r4 = yield readFilePromise('some4.json')
    console.log(r4)  // 打印第 4 个文件内容
})
```

`Generator`是一个什么东西！说来话长，这要从 ES6 的另一个概念Iterator说起.

#### Iterator 遍历器

ES6 中引入了很多此前没有但是却非常重要的概念，Iterator就是其中一个。Iterator对象是一个指针对象，实现类似于单项链表的数据结构，通过next()将指针指向下一个节点

```javascript
const arr = [100, 200, 300]
const iterator = arr[Symbol.iterator]()  // 通过执行 [Symbol.iterator] 的属性值（函数）来返回一个 iterator 对象
```

这里返回了一个 iterator, 也就可以使用 `next` or `for of`, 先看第一种

```javascript
// next
console.log(iterator.next())  // { value: 100, done: false }
console.log(iterator.next())  // { value: 200, done: false }
console.log(iterator.next())  // { value: 300, done: false }
console.log(iterator.next())  // { value: undefined, done: true }

// for of
let i
for (i of iterator) {
    console.log(i)
}
// 打印：100 200 300
```

是不是有种似曾相识的感觉, 你猜对了。generator返回的也是 iterator, value 返回值, done 表示是否获取完成

```javascript
function* Hello() {
    yield 100
    yield (function () {return 200})()
    return 300
}
const h = Hello()
console.log(h[Symbol.iterator])  // [Function: [Symbol.iterator]]
```

#### Generator 的具体应用

我们知道`yield`具有返回数据的功能, `yield`返回的数据被存在`value`属性中

```javascript
function* G() {
    const a = yield 100
    console.log('a', a)  // a aaa
    const b = yield 200
    console.log('b', b)  // b bbb
    const c = yield 300
    console.log('c', c)  // c ccc
}
const g = G()
g.next()    // value: 100, done: false
g.next('aaa') // value: 200, done: false
g.next('bbb') // value: 300, done: false
g.next('ccc') // value: undefined, done: true
```

有一个要点需要注意，就g.next('aaa')是将'aaa'传递给上一个已经执行完了的yield语句前面的变量，而不是即将执行的yield前面的变量。

#### Thunk 函数

要想让Generator和异步操作产生联系，就必须过thunk函数这一关. 首先来看看一个普通的异步函数
```javascript
// node readfile
fs.readFile('data1.json', 'utf-8', (err, data) => {
    // 获取文件内容
})
```
这个是一个多参函数,接收文件路径、编码格式、callback函数方法, 现在就将它封装成thunk函数

```javascript
const thunk = function (fileName, codeType) {
    // 返回一个只接受 callback 参数的函数
    return function (callback) {
        fs.readFile(fileName, codeType, callback)
    }
}
const readFileThunk = thunk('data1.json', 'utf-8')
readFileThunk((err, data) => {
    // 获取文件内容
})
```

或许你在想这不就是拆开callback函数吗,对 thunk就是只接收一个参数 那就是callback. 不过现在已经有了很多第三方库了 `thunkify`
```javascript
const thunkify = require('thunkify')
const thunk = thunkify(fs.readFile)
const readFileThunk = thunk('data1.json', 'utf-8')
readFileThunk((err, data) => {
    // 获取文件内容
})
```

接下来我们在 generator 中使用 thunk 函数

```javascript
const thunkify = require('thunkify')
const readFileThunk = thunkify(fs.readFile)
const gen = function* () {
    const r1 = yield readFileThunk('data1.json')
    console.log(r1)
    const r2 = yield readFileThunk('data2.json')
    console.log(r2)
}

const g = gen()

// 试着打印 g.next() 这里一定要明白 value 是一个 thunk函数 ，否则下面的代码你都看不懂
// console.log( g.next() )  // g.next() 返回 {{ value: thunk函数, done: false }}
// 下一行中，g.next().value 是一个 thunk 函数，它需要一个 callback 函数作为参数传递进去
g.next().value((err, data1) => {
    // 这里的 data1 获取的就是第一个文件的内容。下一行中，g.next(data1) 可以将数据传递给上面的 r1 变量，此前已经讲过这种参数传递的形式
    // 下一行中，g.next(data1).value 又是一个 thunk 函数，它又需要一个 callback 函数作为参数传递进去
    g.next(data1).value((err, data2) => {
        // 这里的 data2 获取的是第二个文件的内容，通过 g.next(data2) 将数据传递个上面的 r2 变量
        g.next(data2)
    })
})
```

继续改造流程,上面的流程看起来优点回调金字塔模式. 自驱动流程
```javascript
// 自动流程管理的函数
function run(generator) {
    const g = generator()
    function next(err, data) {
        const result = g.next(data)  // 返回 { value: thunk函数, done: ... }
        if (result.done) {
            // result.done 表示是否结束，如果结束了那就 return 作罢
            return
        }
        result.value(next)  // result.value 是一个 thunk 函数，需要一个 callback 函数作为参数，而 next 就是一个 callback 形式的函数
    }
    next() // 手动执行以启动第一次 next
}
// 定义 Generator
const thunkify = require('thunkify')
const readFileThunk = thunkify(fs.readFile)
const gen = function* () {
    const r1 = yield readFileThunk('data1.json')
    console.log(r1.toString())
    const r2 = yield readFileThunk('data2.json')
    console.log(r2.toString())
}
// 启动执行
run(gen)
```

其实这段代码和上面的手动编写读取两个文件内容的代码，原理上是一模一样的，只不过这里把流程驱动给封装起来了。我们简单分析一下这段代码. </br>
* 最后一行run(gen)之后，进入run函数内部执行
* 先const g = generator()创建Generator实例，然后定义一个next方法，并且立即执行next()
* 注意这个next函数的参数是err, data两个，和我们fs.readFile用到的callback函数形式完全一样
* 第一次执行next时，会执行const result = g.next(data)，而g.next(data)返回的是{ value: thunk函数, done: ... }，value是一个thunk函数，done表示是否结束
* 如果done: true，那就直接return了，否则继续进行
* result.value是一个thunk函数，需要接受一个callback函数作为参数传递进去，因此正好把next给传递进去，让next一直被执行下去

上面我们使用了 run 函数做流程控制, 现在使用 `co` 库继续简化流程

```javascript
const co = require('co')
// 定义 Generator
const readFileThunk = thunkify(fs.readFile)
const gen = function* () {
    const r1 = yield readFileThunk('data1.json')
    console.log(r1.toString())
    const r2 = yield readFileThunk('data2.json')
    console.log(r2.toString())
}
const c = co(gen)
```
而且const c = co(gen)返回的是一个Promise对象，可以接着这么写
```javascript
c.then(data => {
    console.log('结束')
})
```
如果使用co来处理Generator的话，其实yield后面可以跟thunk函数，也可以跟Promise对象.

#### Generator的本质

介绍Generator的时候，多次提到 暂停 这个词 ———— “暂停”才是Generator的本质 ———— 只有Generator能让一段程序执行到指定的位置先暂停，然后再启动，再暂停，再启动。</br>
而这个 暂停 就很容易让它和异步操作产生联系，因为我们在处理异步操作时，即需要一种“开始读取文件，然后暂停一下，等着文件读取完了，再干嘛干嘛...”这样的需求。因此将Generator和异步操作联系在一起，并且产生一些比较简明的解决方案，这是顺其自然的事儿，大家要想明白这个道理。</br>
不过，JS 还是 JS，单线程还是单线程，异步还是异步，callback还是callback。这一切都不会因为有一个Generator而有任何变化。

#### ES7 引入 async-await

async-await 这个东西是怎么出来的勒 在ES6设计的时候 generator 设计师设计完毕后,经过社区的各种封装终于出现了一个很简洁的异步处理方案.
然后你懂的, 设计师要把这个generator设计的比较清晰 还未发布的 ES7 async-await就是 generator的语法糖

```javascript
const Q = require('Q')
const readFilePromise = Q.denodeify(fs.readFile)

// 定义 async 函数
const readFileAsync = async function () {
    const f1 = await readFilePromise('data1.json')
    const f2 = await readFilePromise('data2.json')
    console.log('data1.json', f1.toString())
    console.log('data2.json', f2.toString())

    return 'done' // 先忽略，后面会讲到
}
// 执行
const result = readFileAsync()
```

使用async-await的不同之处 </br>
第一，await后面不能再跟thunk函数，而必须跟一个Promise对象（因此，Promise才是异步的终极解决方案和未来）。跟其他类型的数据也OK，但是会直接同步执行，而不是异步。</br>
第二，执行const result = readFileAsync()返回的是个Promise对象，而且上面代码中的return 'done'会直接被下面的then函数接收到 </br>
第三，从代码的易读性来将，async-await更加易读简介，也更加符合代码的语意。而且还不用引用第三方库，也无需学习Generator那一堆东西，使用成本非常低。
```javascript
result.then(data => {
    console.log(data)  // done
})
```
async 函数 返回一个promise对象, 然后就可以用then方法了。 await 等待一个表达式的返回结果, 也可用于同步。 看来未来异步终极解决方案就是 async-await了.
