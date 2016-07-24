## [class 的静态属性和实例属性](http://es6.ruanyifeng.com/#docs/class#Class的静态属性和实例属性)

静态属性指的是`class`本身的属性，即`Class.propname`,而不是定义在实例对象`this`上的属性。

```javascript
class Foo{
}
Foo.prop = 1;
Foo.prop //1
```

上面的写法为`Foo`类定义了一个静态属性`prop`。

目前，只有这种写法可行，因为 es6 明确规定，class 内部只有静态方法，没有静态属性。

```
//按照 es6 标准的规定，下面两种写法都是无效的
class Foo {
  // 写法一
  prop:2
  
  // 写法二
  static prop:2
}
Foo.prop //undefined
```

***不过***，es7 有一个静态属性的[提案](https://github.com/jeffmo/es-class-public-fields),目前 Babel 转码器支持。

这个提案对实例属性和静态属性，都规定了新的写法。具体参考[这里](http://es6.ruanyifeng.com/#docs/class#Class的静态属性和实例属性)

