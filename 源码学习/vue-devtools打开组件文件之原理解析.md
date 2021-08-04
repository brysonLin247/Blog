## vue-devtools打开组件文件之原理解析

为了响应若川大佬的号召，一起攻克源码，本文将对vue-devtools打开对应组件文件的原理进行解析。

### 尴尬的处境

平时在工作中，如果你接手了前同事开发留下来的代码，可能会因为没有文档或注释，导致你束手无策，无法很快找到对应的源代码。

平时自己在开发中，也没有使用过vue-devtools，一直觉得它是一个没有什么用的工具，当我阅读完若川大佬的文章后，感觉发现了新大陆啊，原来这个工具可以这么用！

vue-devtools提供了一个功能：**点击页面按钮会让编辑器自动打开对应文件**。

![image-20210804195356407](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804195356407.png)

类似地，react也有react-dev-inspector实现了这个功能。同时，它们也都支持主流的编辑器，不用担心不兼容的情况。

### 原理

通过源码发现，vue-devtools做的事情其实并不复杂，其中利用了nodejs中的子进程，去执行**通过拿到文件路径其找文件**的功能，再利用命令（mac和linux使用`ps x`，Window使用`Get-Process`）去查找编辑器来打开，当然也可以自己指定编辑器。

如果按照上述图片的做法无法用vscode打开组件，要考虑：

- cmd命令行不支持vscode的code命令，这时候需要自己去安装
- 在环境变量中指定编辑器

接下来，在我们解析源码之前，先看一下vue-devtools的[官方文档](https://devtools.vuejs.org/)。

### 阅读官方文档

![image-20210804201433164](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804201433164.png)

从文档中，我们获悉Vue CLI3已经默认有了vue-devtools的功能，我们无需再进行额外的安装。也从中了解了devtools的使用方法：

```javascript
// 1.导入launch-editor-middleware包
var openInEditor = require('launch-editor-middleware')

// 2. 在devServer选项中，注册‘/__open-in-editor’HTTP路由
devServer: {
  before (app) {
    app.use('/__open-in-editor', openInEditor())
  }
}

// 3.猜测要启动的编辑器
openInEditor('code')

// 4.现在可以在组件窗口中单击组件的名称（如果 devtools 知道其文件源，则会出现一个提示）。
```

当然，我们也可以自定义请求，使用以下代码更改请求主机：

```javascript
if (process.env.NODE_ENV !== 'production')
  // App served from port 4000
  // Webpack dev server on port 9000
  window.VUE_DEVTOOLS_CONFIG = {
    openInEditorHost: 'http://localhost:9000/'
  }
}
```

接下来，在开始源码分析前，我们需要搭建一个vue cli3的环境，直接根据官方的步骤做即可，这里不多做解释。

在一切环境搭建之后，我们开始我们的调试之旅~，首先先让我们打开调试模式：

![image-20210804202434689](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804202434689.png)

并找到`launch-editor-middleware`中间件的具体位置：

![image-20210804202712806](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804202712806.png)

接下来，我们开始对源码进行分析。

### 源码分析

首先，我们先找到在官方文档中代码出现的位置：

```javascript
// myProject\node_modules\@vue\cli-service\lib\commands\serve.js
const launchEditorMiddleware = require('launch-editor-middleware')
...
/* webpack-dev-server是webpack官方提供的一个小型Express服务
 * 使用它可以为webpack打包生成的资源文件提供web服务
 * 功能：为静态文件提供服务、自动刷新和热替换
*/
const server = new WebpackDevServer(compiler, Object.assign({
  ...
  before (app, server) {
  // launch editor support.
  // this works with vue-devtools & @vue/cli-overlay
  app.use('/__open-in-editor', launchEditorMiddleware(() => console.log(
  `To specify an editor, specify the EDITOR env variable or ` +
  `add "editor" field to your Vue project config.\n`
)))
...
```

可以看出，利用expresss在当我们点击打开编辑器时，发出了一个请求。

![image-20210804203511379](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804203511379.png)

原来为了实现这样的效果，还用到了webpack、express，漫漫前端路，道阻且长啊~

我们进入`launchEditorMiddleware`去看一下这个函数的具体实现：

```javascript
// myProject\node_modules\launch-editor-middleware\index.js
// 我们对这句代码打个断点，因为它是这个文件中最核心的一段代码。
launch(path.resolve(srcRoot, file), specifiedEditor, onErrorCallback)
```

打开调试模式后，我们点击浏览器打开HelloWorld组件的文件，开始调试。

#### launch-editor-middleware

```javascript
// myProject\node_modules\launch-editor-middleware\index.js
const url = require('url')
const path = require('path')
const launch = require('launch-editor')

module.exports = (specifiedEditor, srcRoot, onErrorCallback) => {
  // specifiedEditor => console.log(...)函数
  // 如果specifiedEditor是函数，把它赋值给错误回调函数。功能：切换参数
  if (typeof specifiedEditor === 'function') {
    onErrorCallback = specifiedEditor
    specifiedEditor = undefined
  }
  // 如果srcRoot是函数，把它赋值给错误回调函数。功能：切换参数
  if (typeof srcRoot === 'function') {
    onErrorCallback = srcRoot
    srcRoot = undefined
  }
  
  // srcRoot 是传递过来的参数，或者当前node进程的目录
  srcRoot = srcRoot || process.cwd()
	// express中间件
  return function launchEditorMiddleware (req, res, next) {
    // 从请求过程中我们解析出file路径
    const { file } = url.parse(req.url, true).query || {}
    // 无文件路径，则报错
    if (!file) {
      res.statusCode = 500
      res.end(`launch-editor-middleware: required query param "file" is missing.`)
    } else {
      // 否则，将srcRoot和file进行拼接，进入下一个函数：launch-editor
      launch(path.resolve(srcRoot, file), specifiedEditor, onErrorCallback)
      res.end()
    }
  }
}
```

![image-20210804205801103](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804205801103.png)

**上面这种切换参数的写法，为的是方便用户调用时传参。虽然是多个参数，但可以传一个或者两个**，又涨知识了~

以上操作之后，可以看出这个文件会将一个路径传入到launchEditor函数进行操作。

这里的切换参数在很多源码中都会体现到，若川大佬给了我个例子，豁然开朗：

```javascript
// 定义一个person函数
function person(name,age,say){
  console.log(name,age);
  if(typeof age === 'function'){
    say = age;
  }
  say && say();
}
// 调用方法一
person('Bryson',22,function(){
  console.log('我是Bryson啊')
})
// Bryson 22
// 我是Bryson啊

// 调用方法二，不传age，让function()直接切换给age
person('Bryson',function(){
  console.log('我是Bryson啊')
})
// Bryson f(){console.log('我是Bryson啊')}
// 我是Bryson啊
```

#### launch-editor

进入launchEditor()函数。

```javascript
// myProject\node_modules\launch-editor\index.js
function launchEditor (file, specifiedEditor, onErrorCallback) {
  // 对file进行解析，得出文件路径和行号列号
  const parsed = parseFile(file)
  let { fileName } = parsed
  const { lineNumber, columnNumber } = parsed
	
  // 利用fs去查看是否存在对应文件
  if (!fs.existsSync(fileName)) {
    return
  }
  
  // 如果specifiedEditor是函数，把它赋值给错误回调函数。功能：切换参数
  if (typeof specifiedEditor === 'function') {
    onErrorCallback = specifiedEditor
    specifiedEditor = undefined
  }
	// 包裹错误回调
  onErrorCallback = wrapErrorCallback(onErrorCallback)
	// 字面意思，判断使用哪个编译器
  const [editor, ...args] = guessEditor(specifiedEditor)
  if (!editor) {
    onErrorCallback(fileName, null)
    return
  }
	...
}
```

包裹错误回调的函数：

![image-20210804211212409](C:\Users\bryson.lin\AppData\Roaming\Typora\typora-user-images\image-20210804211212409.png)

用于打印出错误信息。

#### guessEditor

调试进入到guess文件，我们先从字面意思知道，这个函数就是用来判断使用哪个编辑器的，带着这个疑问查看源码：

```javascript
// myProject/node_modules/launch-editor/guess.js
const path = require('path')
const shellQuote = require('shell-quote')
const childProcess = require('child_process')

// 从完整进程名映射到启动进程的二进制文件
const COMMON_EDITORS_OSX = require('./editor-info/osx')
const COMMON_EDITORS_LINUX = require('./editor-info/linux')
const COMMON_EDITORS_WIN = require('./editor-info/windows')

module.exports = function guessEditor (specifiedEditor) {
  // 1.如果我们自己指定了编辑器，会进行解析，然后直接返回，不用进行下面的猜测编辑器的操作。
  if (specifiedEditor) {
    return shellQuote.parse(specifiedEditor) // 切割空格功能
  }
  // We can find out which editor is currently running by:
  // `ps x` on macOS and Linux
  // `Get-Process` on Windows
  // 2.通过命令查看哪个编辑器在运行
  try {
    if (process.platform === 'darwin') {
      const output = childProcess.execSync('ps x').toString()
      ...
    } else if (process.platform === 'win32') {
      const output = childProcess
        .execSync('powershell -Command "Get-Process | Select-Object Path"', {
          stdio: ['pipe', 'pipe', 'ignore']
        })
        .toString()
      ...
    } else if (process.platform === 'linux') {
      // --no-heading No header line
      // x List all processes owned by you
      // -o comm Need only names column
      const output = childProcess
        .execSync('ps x --no-heading -o comm --sort=comm')
        .toString()
      ...
    }
  } catch (error) {
    // Ignore...
  }

  // Last resort, use old skool env vars
  // 3.如果没有找到当前进程有运行的编辑器，则使用环境变量指定编辑器
  if (process.env.VISUAL) {
    return [process.env.VISUAL]
  } else if (process.env.EDITOR) {
    return [process.env.EDITOR]
  }
	// 4.如果都没找到就返回null
  return [null]
}
```

得出使用的编辑器后，则：

```javascript
// myProject/node_modules/launch-editor/guess.js
const childProcess = require('child_process')
let _childProcess = null // 设置子进程

function launchEditor (file, specifiedEditor, onErrorCallback) {
  ...
  if (process.platform === 'win32') {
    // On Windows, launch the editor in a shell because spawn can only
    // launch .exe files.
    _childProcess = childProcess.spawn( // 衍生新进程
      'cmd.exe',
      ['/C', editor].concat(args),
      { stdio: 'inherit' }
    )
  } else {
    _childProcess = childProcess.spawn(editor, args, { stdio: 'inherit' })
  }
  _childProcess.on('exit', function (errorCode) { // exit事件
    _childProcess = null

    if (errorCode) {
      onErrorCallback(fileName, '(code ' + errorCode + ')')
    }
  })

  _childProcess.on('error', function (error) { // error事件
    onErrorCallback(fileName, error.message)
  })
}

module.exports = launchEditor
```

从这段源码中，可以看出editor可以获取出：

![image-20210804231414163](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210804231414163.png)

得到了我的editor为vscode，运行命令并执行exit清空子进程后退出，至此前面的代码均运行完毕。

### 总结

跟着若川大佬的源码共读，学到了蛮多东西：

- vscode调试代码的技巧
- vue-devtools打开组件文件原理解析
- node子进程的相关知识

这个部分源码虽少，但是却给人一种拓宽视野的感觉。

****

如果文章对您有帮助，请帮忙`点赞`哟！

