# node
node规范
文章链接：http://javascript.ruanyifeng.com/nodejs/module.

总结：

1.Node 应用由模块组成，采用 CommonJS 模块规范。

每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

2.module对象

Node内部提供一个Module构建函数。所有模块都是Module的实例。
module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。
exports变量：var exports = module.exports;

3.AMD规范与CommonJS规范的兼容性
AMD规范：异步（浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范）
CommonJS规范：同步（Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用）

4.require命令

Node使用CommonJS模块规范，内置的require命令用于加载模块文件
require命令的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

加载规则：require命令用于加载文件，后缀名默认为.js。

根据参数的不同格式，require命令去不同路径寻找模块文件。

（1）如果参数字符串以“/”开头，则表示加载的是一个位于绝对路径的模块文件。比如，require('/home/marco/foo.js')将加载/home/marco/foo.js。

（2）如果参数字符串以“./”开头，则表示加载的是一个位于相对路径（跟当前执行脚本的位置相比）的模块文件。比如，require('./circle')将加载当前脚本同一目录的circle.js。

（3）如果参数字符串不以“./“或”/“开头，则表示加载的是一个默认提供的核心模块（位于Node的系统安装目录中），或者一个位于各级node_modules目录的已安装模块（全局安装或局部安装）。

（4）如果参数字符串不以“./“或”/“开头，而且是一个路径，比如require('example-module/path/to/file')，则将先找到example-module的位置，然后再以它为参数，找到后续路径。

（5）如果指定的模块文件没有发现，Node会尝试为文件名添加.js、.json、.node后，再去搜索。.js件会以文本格式的JavaScript脚本文件解析，.json文件会以JSON格式的文本文件解析，.node文件会以编译后的二进制文件解析。

（6）如果想得到require命令加载的确切文件名，使用require.resolve()方法。

目录的加载规则:
我们会把相关的文件会放在一个目录里面，便于组织。这时，最好为该目录设置一个入口文件，让require方法可以通过这个入口文件，加载整个目录。

在目录中放置一个package.json文件，并且将入口文件写入main字段。下面是一个例子。

require发现参数字符串指向一个目录以后，会自动查看该目录的package.json文件，然后加载main字段指定的入口文件。
如果package.json文件没有main字段，或者根本就没有package.json文件，则会加载该目录下的index.js文件或index.node文件。


模块的缓存

第一次加载某个模块时，Node会缓存该模块。以后再加载该模块，就直接从缓存取出该模块的module.exports属性。


require函数及其辅助方法主要如下。

require(): 加载外部模块
require.resolve()：将模块名解析到一个绝对路径
require.main：指向主模块
require.cache：指向所有缓存的模块
require.extensions：根据文件的后缀名，调用不同的执行函数
