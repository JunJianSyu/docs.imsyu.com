> Author: JunJianSyu <br />
> Slogan: The Ultimate Test Machine.

## 介绍

**单元测试到底是什么?**

!> 需要访问数据库的测试不是单元测试<br/>
   需要访问网络的测试不是单元测试<br/>
   需要访问文件系统的测试不是单元测试

**我们在单元测试中应该避免什么？**

* 太多的条件逻辑
* 构造函数中做了太多事情
* too many全局变量
* too many静态方法
* 无关逻辑
* 过多外部依赖

## TDD(Test-driven development)

*测试驱动开发*

TDD需要遵循如下规则:

* 写一个单元测试去描述程序的一个方面
* 运行它应该会失败，因为程序还缺少这个特性
* 为这个程序添加一些尽可能简单的代码保证测试通过
* 重构这部分代码，直到代码没有重复、代码责任清晰并且结构简单
* 持续重复这样做，积累代码

另外，衡量是否使用了TDD的一个重要标准是测试对代码的覆盖率，覆盖率在80%以下说明一个团队没有充分掌握TDD，当然高覆盖率也不能说一定使用了TDD，这仅仅是一个参考指标。

在我看来，TDD是一种开发技术，而非测试技术，所以它对于代码构建的意义远大于代码测试。也许最终的代码和先开发再测试写的测试代码基本一致，但它们仍然是有很大不同的。TDD具有很强的目的性，在直接结果的指导下开发生产代码，然后不断围绕这个目标去改进代码，其优势是高效和去冗余的。所以其特点应该是由需求得出测试，由测试代码得出生产代码。打个比方就像是自行车的两个轮子，虽然都是在向同一个方向转动，但是后轮是施力的，带动车子向前，而前轮是受力的，被向前的车子带动而转。

## BDD(Behaviour-driven Development)

*行为驱动开发*

所谓的BDD行为驱动开发，即Behaviour Driven Development，是一种新的敏捷开发方法。它更趋向于需求，需要共同利益者的参与，强调用户故事（User Story）和行为。

> BDD是第二代的、由外及内的、基于拉(pull)的、多方利益相关者的(stakeholder)、多种可扩展的、高自动化的敏捷方法。它描述了一个交互循环，可以具有带有良好定义的输出（即工作中交付的结果）：已测试过的软件。

简单来说，我认为BDD更加注重业务需求而不是技术，虽然看起来BDD确实是比ATDD做的更好，但这是一种误导，这仅仅是就某种环境下而言的。而且以国内的现状来看TDD要比BDD更适合，因为它不需要所有人员的理解和加入。

## 如何进行单元测试

**测试管理工具**

测试管理工具是用来组织和运行整个测试的工具，它能够将测试框架、断言库、测试浏览器、测试代码和被测试代码组织起来，并运行测试代码进行测试。测试工具有很多选择:

* [Selenium](http://www.seleniumhq.org/)
* [WebDriver/Selenium 2](https://www.ibm.com/developerworks/cn/web/wa-selenium2/)
* [Mocha](http://mochajs.org/)
* [JsTestDriver](https://code.google.com/archive/p/js-test-driver/)
* [Karma](http://karma-runner.github.io/1.0/index.html)

关于这些工具的对比 [Karma测试框架的前世今生](http://taobaofed.org/blog/2016/01/08/karma-origin/)

**测试框架**

测试框架是单元测试的核心，它提供了单元测试所需的各种API，你可以使用它们来对你的代码进行单元测试。
[Javascript测试框架列表](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#JavaScript)

**断言库**

断言库提供了用于描述你的具体测试的API，有了它们你的测试代码便能简单直接，也更为语义化，理想状态下你甚至可以让非开发人员来撰写单元测试。

* [better-assert](https://github.com/tj/better-assert)
* [should.js](https://github.com/shouldjs/should.js)
* [expect.js](https://github.com/Automattic/expect.js)
* [chai](http://chaijs.com/)

**测试浏览器**

前端代码是运行在浏览器中的，要对其进行单元测试，只能将其运行在浏览器上。目前大部分测试工具都支持调用和运行本地浏览器来进行测试，但如果你的测试仅仅是针对函数和模块的单元测试，则完全可以使用一款无界面的浏览器：[PhantomJs](http://phantomjs.org/)

**测试覆盖率统计工具**

Karma-Coverage是Karma官方提供的覆盖率统计插件，自然成为项目的首选。

下面我们首选以下工具清单:

* 测试管理工具: Karma
* 测试框架: Mocha
* 断言库: Chai
* 测试浏览器: PhantomJs
* 测试覆盖率统计工具: Karam-Coverage

```bash
// 初始化package.json
npm init
// 安装需要的包
npm install chai --save-dev
npm install karma --save-dev
npm install karma-chai --save-dev
npm install karma-coverage --save-dev
npm install karma-mocha --save-dev
npm install karma-phantomjs-launcher --save-dev
npm install mocha --save-dev

// 初始化Karma配置文件
karma init
```

修改`package.json`里面的`scripts`

```json
"scripts": {
    "test": "karma start ./karma.conf.js"
}
```

生成的`karma.conf.js`只是最基本的`karma`配置，我们还需要手动修改一些地方。在其中的`frameworks`一项中增加`chai`

```bash
frameworks: ['mocha','chai'],
```

然后在`config.set({})`中添加

```bash
plugins : [
  'karma-mocha',
  'karma-chai',
  'karma-phantomjs-launcher'
],
```

由于包依赖是npm的弊病,经过`github issues`了解到`Yarn`

> Fast, reliable, and secure dependency management.

```bash
npm install -g yarn
```

!> 如果出现phantomjs之类的错误信息

```bash
rm -rf node_modules
removeing the yarn cache (~/Library/Cache/Yarn on MacOS, ~/.cache/yarn on Linux)
yarn
yarn test
```

**提供需要测试的代码并编写测试代码**

在`karma init`初始化配置时,需要测试的代码和测试代码防止的文件夹 `src/` 和 `test/`。`src/`放置被测试代码, `test/`放置测试代码

*src/index.js*

```javascript
function isNum (num) {
  return typeof num === 'number'
}
function isString (str) {
  return typeof str === 'string'
}
```

*test/index.test.js*

```javascript
describe('index.js的测试', function () {
  it('1应该是数字', function() {
      // expect(isNum(1)).to.be.true
      isNum(1).should.equal(true)
  })
  it('"1" 应该是字符', function() {
      // expect(isString('1')).to.be.true
      isString('1').should.equal(true)
  })
})
```

**统计测试覆盖率**

单元测试很多时候需要统计测试覆盖率。使用`karma-coverage`来统计你的单元测试覆盖率。修改`karma.conf.js`：

```json
preprocessors: {
  'src/*.js': ['coverage']
},
reporters: ['progress', 'coverage'],
```

*karma.conf.js*

```json
plugins: [
 ...
 'karma-coverage'
]
```

执行`npm run test`,此时会在项目根目录生成`coverage`文件夹, 目录里面的`index.html`打开可查看测试覆盖率


**集成Webpack**

很多时候，项目中会用到[webpack](https://webpack.github.io/)来进行打包，有了Webpack和[Babel](https://babeljs.io/)我们可以使用ES6甚至ES7语法，可以轻松打包[Vue](https://vuejs.org/)、[React](https://facebook.github.io/react/)等主流框架，可以使用[Eslint](http://eslint.org/)进行代码检查。所以将Webpack集成进Karma后，我们可以使用最新的JS语法来编写测试代码，也可以对使用了主流框架的代码进行单元测试了。

这里不多做介绍,配置各种主流框架例如Vue

详细参考: [vuejs-templates](https://github.com/vuejs-templates/webpack)





