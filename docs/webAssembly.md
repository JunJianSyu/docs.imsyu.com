> Author: JunJianSyu <br />
> Slogan: Getting into the soul.

#### WebAssembly 历史
2015年6月`WebAssembly`首次发布,2016年 被各大浏览器商共同参与规范讨论,在浏览器里面实现,被设计为 便携式堆栈机,旨在要比javascript更快解析. </br>
体验新技术当然首选google浏览器, 升级到最新版.
* Chrome: 打开 `chrome://flags/#enable-webassembly`，选择 `enable`.

首先根据官方要求我们需要做以下工作:
* 在Linux和OS X上 安装[Git](https://git-scm.com/)分布式版本控制系统
* 在Linux和OS X上 安装[CMake](https://cmake.org/download/)
* Linux 安装GCC, Mac OS 有 Xcode就ok , windows 安装 Visual Studio
* python2.7 在 Mac OS、linux上自带,开箱即用

##### 编译 Emscripten
重要之中的重要工具, 把其他语言编译成WebAssembly的二进制, 首先安装
我自己mac os 安装在 `/usr/local`目录下
```shell
$ git clone https://github.com/juj/emsdk.git
$ cd emsdk
$ ./emsdk install sdk-incoming-64bit binaryen-master-64bit
$ ./emsdk activate sdk-incoming-64bit binaryen-master-64bit
```
上面执行完之后, 直接执行下面语句 编译后的环境变量添加:
```shelll
$ source ./emsdk_env.sh
```
一切完事, 控制台键入 `emcc` 测试是否安装成功.

创建一个`hello.c`文件:
```c
#include <stdio.h>
int main(int argc, char ** argv) {
    printf("Hello, world!\n");
}
```

控制台执行`emcc hello.c -s WASM=1 -o hello.html`, 然后启动一个web服务, 就用默认的emrun `emrun --no_browser --port 8080 .` 执行`hello.html`页面console将打印`hello world!`

上面的例子生成直接可执行的demo例子,但是例子太大了, 我们直接生成体积较小的`wasm`文件 `emcc hello.c -Os -s WASM=1 -s SIDE_MODULE=1 -o hello.wasm`

##### 如何运行 WebAssembly 二进制文件

目前只有一种方式能调用`wasm`里的提供接口，用`javascript`

`WebAssembly`目前只设计也只实现了`javascript API`，就像我刚开始提供的那个例子一样，只有通过`js`代码来编译、实例化才可以调用其中的接口。这也很好的说明了`WebAssembly`并不是要替代`javascript`，而是用来增强`javascript`和`Web`平台的能力的。我觉得`WebAssembly`更适合用于写模块，承接各种复杂的计算，如图像处理、3D运算、语音识别、视音频编码解码这种工作，主体程序还是要用`javascript`来写的。

如何加载 -> 转换buffer -> 编译 -> 实例化:
```javascript
/**
 * @param {String} path wasm 文件路径
 * @param {Object} imports 传递到 wasm 代码中的变量
 */
function loadWebAssembly (path, imports = {}) {
    return fetch(path)
        .then(response => response.arrayBuffer())
        .then(buffer => WebAssembly.compile(buffer))
        .then(module => {
            imports.env = imports.env || {}

            // 开辟内存空间
            imports.env.memoryBase = imports.env.memoryBase || 0
            if (!imports.env.memory) {
                imports.env.memory = new WebAssembly.Memory({ initial: 256 })
            }

            // 创建变量映射表
            imports.env.tableBase = imports.env.tableBase || 0
                if (!imports.env.table) {
                // 在 MVP 版本中 element 只能是 "anyfunc"
                imports.env.table = new WebAssembly.Table({ initial: 0, element: 'anyfunc' })
            }

            // 创建 WebAssembly 实例
            return new WebAssembly.Instance(module, imports)
    })
}
```

这个`loadWebAssembly`函数还接受第二个参数，表示要传递给`wasm`的变量，在初始化`WebAssembly`实例的时候，可以把一些接口传递给`wasm`代码。

调用`loadWebAssembly`方法:
```javascript
loadWebAssembly('./hello.wasm').then(instance => {
    console.log(instance)
})
```