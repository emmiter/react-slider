## [export default](http://es6.ruanyifeng.com/#docs/module#export-default命令)

> 使用 `import` 命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。但是用户希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。
>
> 为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到 `export default` 命令，**为模块指定默认输出**。

注意: 

```
// 输出
export default function crc32(){
  //...
}
// 输入
export crc32 from 'crc32';

//输出
export function crc32(){
  //...
}
//输入
import {crc32} from 'crc32';
```

总结:

1. 如果某个模块定义了默认输出，如果在用到该模块的模块中，输入的是所需模块的默认输出，则无需加 `{}`; 如果输入的是非默认输出，则需要加`{}`,如果输入了多个变量名/函数/对象，在它们之间以`,`隔开。

2. 如果某个模块未定义默认输出，则在用到该模块的模块中，输入的变量名/函数/对象要加`{}`，多个的话用`,`分隔。

3. 由于模块内的变量名/函数/对象在模块外都是不可见的，所以输入到别的模块时，要记得起名字，可以同名，也可以使用`as`对输入的变量重命名。比如:

   ```javascript
   import {lastName as surname} from './profile';
   // `surname`是重命名后的新名字
   ```
   ​

