### this 绑定

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后就
可以顺序应用下面这四条规则来判断 this 的绑定对象。箭头函数会继承外层函数调用的 this
绑定。

1. 由 new 调用？绑定到新创建的对象。
2. 由 call 或者 apply（或者 bind）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到 undefined，否则绑定到全局对象。

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行原型连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

### call 和 apply 

`func.call(context, ...args)` 和 `func.apply(context, args)` 都是在给定的上下文中调用
 func，即将 `context` 作为 `this`，它们之间唯一的语法区别是，call 期望一个参数列表，而
apply 期望一个包含这些参数的类数组对象。

### bind

`func.bind(context)` 在指定了 `this` 为 `context` 同时，所有参数被原样传递给了 `func`。它的结果
是 `func` 的绑定的变体，它可以像函数一样被单独使用或者作为参数传递给其它方法，函数
上下文都是正确的。`bind` 的完整语法是 `func.bind(context, [arg1], [arg2], ...)`，它不仅可以绑定
上下文为 `this` ，还可以绑定函数的起始参数。当我们绑定一个现有的函数的某些参数时，
绑定后的（不太通用的）函数被称为偏函数。

### 原型

如果要访问对象中并不存在的一个属性，`[[Get]]` 操作就会查找对象内部 `[[Prototype]]` 关联的对
象。这个关联关系实际上定义了一条“原型链”（有点像嵌套的作用域链），在查找属性时
会对它进行遍历。所有普通对象都有内置的 Object.prototype，指向原型链的顶端（比如说全局
作用域），如果在原型链中找不到指定的属性就会停止。

### 原型方法

要使用给定的原型创建对象，使用：字面量语法：`{ __proto__: ... }`，允许指定多个属性。或
`Object.create(proto, [descriptors])`，允许指定属性描述符。

设置和访问原型的现代方法有：`Object.getPrototypeOf(obj)` —— 返回对象 obj 的 `[[Prototype]]`（与
`__proto__` 的 getter 相同）。`Object.setPrototypeOf(obj, proto)` —— 将对象 obj 的 `[[Prototype]]`
设置为 `proto`（与 `__proto__` 的 setter 相同）。

### 行为委托

行为委托认为对象之间是兄弟关系，互相委托，而不是父类和子类的关系。JavaScript 的
`[[Prototype]]` 机制本质上就是行为委托机制。也就是说，我们可以选择在 JavaScript 中努力实现
类机制，也可以拥抱更自然的 `[[Prototype]]` 委托机制。对象关联（对象之前互相关联）是一
种编码风格，它倡导的是直接创建和关联对象，不把它们抽象成类。对象关联可以用基于
`[[Prototype]]` 的行为委托非常自然地实现。

下面是原型面向对象风格：

```JavaScript
function Foo(name) {
    this.name = name;
}
Foo.prototype.identify = function() {
    console.log(this.name);
};
function Bar(name) {
    Foo.call(this, name);
}
Bar.prototype = Object.create(Foo.prototype);
var b1 = new Bar("b1");
```

下面是对象关联风格：

```JavaScript
Foo = {
    init: function(name) {
        this.name = name;
    },
    identify: function() {
        console.log(this.name)
    }
};
Bar = Object.create(Foo);
var b1 = Object.create(Bar);
b1.init("b1");
```
