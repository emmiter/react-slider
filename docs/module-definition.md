## 模块的规范

参考文章和其他文档:

1. [阮一峰-Javascript模块化编程(二)：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)
2. [阮一峰-Javascript模块化编程(三)：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)
3. [stackoverflow.com-Relation between CommonJS,AMD and RequireJS?](http://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs)
4. [使用AMD,CommonJS以及ES Harmony 编写模块化的Javascript](http://justineo.github.io/singles/writing-modular-js/)

有了模块，人们就可以方便地使用别人的代码，想要什么功能，就加载什么模块。但是这样做有一个前提，那就是大家必须以同样的方式编写模块，否则你有你的写法，我有我的写法，这样才能更好地使用模块。

通行的 Javascript 模块规范有：[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)，AMD和现在 es6 自带的模块系统。

### CommonJS(为服务端优化的模块格式)

2009年的时候，美国程序员Ryan Dahl创造了[node.js](https://nodejs.org/en/)项目，将 Javascript 语言用于服务器端编程。

***这标识 Javascript 模块化编程正式诞生***。

因为，其实在浏览器环境下，没有模块化也不是特别大的问题，毕竟网页程序的复杂性有限；但是服务端，一定要有模块，与操作系统和其他应用程序互动，否则没法编程。

node.js 的[模块系统](http://nodejs.org/docs/latest/api/modules.html),就是参照 CommonJS规范实现的。在 CommonJS 中，有一个全局性方法`require()`,用于加载模块。

假设有一个叫 math 的模块,可以像下面这种方式引入:

```javascript
var math = require('math');
```

然后，就可以调用模块提供的方法:

```
var math = require('math');
math.add(2,3); //5
```

因为目前主要做的是 Javascript 在浏览器端的编程，所以对 CommonJS 就先不多做介绍，只需要知道，`require()`用于加载模块就行了。

***有了服务器端模块后，程序员们就想要客户端模块***。而且最好二者能兼容，一个模块不用修改，在服务器和浏览器都可以运行。

但是，有一个重大的局限，使得 CommonJS 规范不适用于浏览器环境。上面使用math 模块的两行代码，如果在浏览器中运行，会有一个很大的问题，你能看出来吗？

也就是说，第二行的`math.add(2,3)`，在第一行`require('math')`之后运行，因此必须等 math.js 加载完成。也就是说，如果加载时间很长，整个应用就会停在那里等待，知道加载完毕才会继续执行。

***这对服务器端不是一个问题，因为所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间。但是，对于浏览器，这却是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于"假死"状态。***

因此，浏览器端的模块，不能采用"同步加载"，只能采用"异步加载"。这就是AMD规范诞生的背景。

## AMD

[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

AMD也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：

> 　　require([module], callback);

第一个参数[module]，是一个数组，里面的成员就是要加载的模块；第二个参数callback，则是加载成功之后的回调函数。如果将前面的代码改写成AMD形式，就是下面这样：

> 　　require(['math'], function (math) {
>
> 　　　　math.add(2, 3);
>
> 　　});

math.add()与math模块加载不是同步的，浏览器不会发生假死。所以很显然，AMD比较适合浏览器环境。

目前，主要有两个Javascript库实现了AMD规范：[require.js](http://requirejs.org/)和[curl.js](https://github.com/cujojs/curl)。本系列的第三部分，将通过介绍require.js，进一步讲解AMD的用法，以及如何将模块化编程投入实战。

#### AMD模块的写法

require.js 加载的模块，采用AMD规范。也就是说，模块必须按照 AMD 的规定来写。

具体来说，就是模块必须采用特定的 define() 函数来定义。如果一个模块不依赖其他模块，那么可以直接定义在 define() 函数中。

假定现在有一个math.js文件，它定义了一个math模块。那么，math.js就要这样写：

```javascript
　　// math.js
　　define(function (){
　　　　var add = function (x,y){
　　　　　　return x+y;
　　　　};
　　　　return {
　　　　　　add: add
　　　　};
　　});
```



加载方法如下:

```
// main.js
require(['math'],(math)=>{
  alert(math.add(1,1));
});
```

如果这个模块还依赖其他模块，那么 define() 函数的第一个参数，必须是一个数组，指明该模块的依赖性。

比如下面这个例子，当require()函数加载上面这个模块时，就会先加载myLib.js文件。

```javascript
define(['myLib'], function(myLib){
　　　　function foo(){
　　　　　　myLib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　});
```

## ES6/ES Harmony/ES.next

### [ES.next() 与 ES Harmony的关系](http://www.2ality.com/2011/06/ecmascript.html)

在 ES.next 中，这些概念被以一种更为精简的方式提出，用了一个关键字 `import` 来指定模块的依赖项。`export` 则和我们想象的没有多大差别，我认为很多开发者看了下面的代码就能立刻明白。

- **import**声明把某个模块的导出绑定为本地变量，并可以重命名来避免命名冲突。
- **export**声明声明了某个模块的本地绑定是外部可见的，这样其它模块就能够读取它们但却无法进行修改。有趣的是，模块可以导出子模块，却无法导出已经在别处定义过的模块。你同样可以给导出重命名来让它们不同于本地的名字。