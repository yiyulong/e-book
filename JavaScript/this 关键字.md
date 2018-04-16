目录
--

1. 含义
2. 使用场合
3. 使用注意点
4. 绑定 this 的方法[call()、apply()、bind()](quiver:///notes/890D4720-6655-4C0B-A2B7-A21603A7FB9A)
    4.1 function.prototype.call()
    4.2 function.prototype.apply()
    4.3 function.prototype.bind()

涵义
--

`this`关键字是一个非常重要的语法点。毫不夸张地说，不理解它的含义，大部分开发任务都无法完成。

首先，`this`总是返回一个对象，简单说，就是返回属性或方法“当前”所在的对象。

```
this.property

```

上面代码中，`this`就代表`property`属性当前所在的对象。

下面是一个实际的例子。

```
var person = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

person.describe()
// "姓名：张三"

```

上面代码中，`this.name`表示`describe`方法所在的当前对象的`name`属性。调用`person.describe`方法时，`describe`方法所在的当前对象是`person`，所以就是调用`person.name`。

由于对象的属性可以赋给另一个对象，所以属性所在的当前对象是可变的，即`this`的指向是可变的。

```
var A = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

var B = {
  name: '李四'
};

B.describe = A.describe;
B.describe()
// "姓名：李四"

```

上面代码中，`A.describe`属性被赋给`B`，于是`B.describe`就表示`describe`方法所在的当前对象是`B`，所以`this.name`就指向`B.name`。

稍稍重构这个例子，`this`的动态指向就能看得更清楚。

```
function f() {
  return '姓名：'+ this.name;
}

var A = {
  name: '张三',
  describe: f
};

var B = {
  name: '李四',
  describe: f
};

A.describe() // "姓名：张三"
B.describe() // "姓名：李四"

```

上面代码中，函数`f`内部使用了`this`关键字，随着`f`所在的对象不同，`this`的指向也不同。

只要函数被赋给另一个变量，`this`的指向就会变。

```
var A = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

var name = '李四';
var f = A.describe;
f() // "姓名：李四"

```

上面代码中，`A.describe`被赋值给变量`f`，内部的`this`就会指向`f`运行时所在的对象（本例是顶层对象）。

再看一个网页编程的例子。

```
\<input type="text" name="age" size=3 onChange="validate(this, 18, 99);"\>

\<script\>
function validate(obj, lowval, hival){
  if ((obj.value \< lowval) || (obj.value \> hival))
    console.log('Invalid Value!');
}
\</script\>

```

上面代码是一个文本输入框，每当用户输入一个值，就会调用`onChange`回调函数，验证这个值是否在指定范围。回调函数传入`this`，就代表传入当前对象（即文本框），然后就可以从`this.value`上面读到用户的输入值。

总结一下，JavaScript 语言之中，一切皆对象，运行环境也是对象，所以函数都是在某个对象之中运行，`this`就是这个对象（环境）。这本来并不会让用户糊涂，但是 JavaScript 支持运行环境动态切换，也就是说，`this`的指向是动态的，没有办法事先确定到底指向哪个对象，这才是最让初学者感到困惑的地方。

如果一个函数在全局环境中运行，那么`this`就是指顶层对象（浏览器中为`window`对象）。

```
function f() {
  return this;
}

f() === window // true

```

上面代码中，函数`f`在全局环境运行，它内部的`this`就指向顶层对象`window`。

可以近似地认为，`this`是所有函数运行时的一个隐藏参数，指向函数的运行环境。

使用场合
----

`this`的使用可以分成以下几个场合。

（1）全局环境

在全局环境使用`this`，它指的就是顶层对象`window`。

```
this === window // true

function f() {
  console.log(this === window); // true
}

```

上面代码说明，不管是不是在函数内部，只要是在全局环境下运行，`this`就是指顶层对象`window`。

（2）构造函数

构造函数中的`this`，指的是实例对象。

```
var Obj = function (p) {
  this.p = p;
};

Obj.prototype.m = function() {
  return this.p;
};

```

上面代码定义了一个构造函数`Obj`。由于`this`指向实例对象，所以在构造函数内部定义`this.p`，就相当于定义实例对象有一个`p`属性；然后`m`方法可以返回这个`p`属性。

```
var o = new Obj('Hello World!');

o.p // "Hello World!"
o.m() // "Hello World!"

```

（3）对象的方法

当 A 对象的方法被赋予 B 对象，该方法中的`this`就从指向 A 对象变成了指向 B 对象。所以要特别小心，将某个对象的方法赋值给另一个对象，会改变`this`的指向。

请看下面的代码。

```
var obj ={
  foo: function () {
    console.log(this);
  }
};

obj.foo() // obj

```

上面代码中，`obj.foo`方法执行时，它内部的`this`指向`obj`。

但是，只有这一种用法（直接在`obj`对象上调用`foo`方法），`this`指向`obj`；其他用法时，`this`都指向代码块当前所在对象（浏览器为`window`对象）。

```
// 情况一
(obj.foo = obj.foo)() // window

// 情况二
(false || obj.foo)() // window

// 情况三
(1, obj.foo)() // window

```

上面代码中，`obj.foo`先运算再执行，即使值根本没有变化，`this`也不再指向`obj`了。这是因为这时它就脱离了运行环境`obj`，而是在全局环境执行。

可以这样理解，在 JavaScript 引擎内部，`obj`和`obj.foo`储存在两个内存地址，简称为`M1`和`M2`。只有`obj.foo()`这样调用时，是从`M1`调用`M2`，因此`this`指向`obj`。但是，上面三种情况，都是直接取出`M2`进行运算，然后就在全局环境执行运算结果（还是`M2`），因此`this`指向全局环境。

上面三种情况等同于下面的代码。

```
// 情况一
(obj.foo = function () {
  console.log(this);
})()
// 等同于
(function () {
  console.log(this);
})()

// 情况二
(false || function () {
  console.log(this);
})()

// 情况三
(1, function () {
  console.log(this);
})()

```

如果某个方法位于多层对象的内部，这时`this`只是指向当前一层的对象，而不会继承更上面的层。

```
var a = {
  p: 'Hello',
  b: {
    m: function() {
      console.log(this.p);
    }
  }
};

a.b.m() // undefined

```

上面代码中，`a.b.m`方法在`a`对象的第二层，该方法内部的`this`不是指向`a`，而是指向`a.b`。这是因为实际执行的是下面的代码。

```
var b = {
  m: function() {
   console.log(this.p);
  }
};

var a = {
  p: 'Hello',
  b: b
};

(a.b).m() // 等同于 b.m()

```

如果要达到预期效果，只有写成下面这样。

```
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};

```

如果这时将嵌套对象内部的方法赋值给一个变量，`this`依然会指向全局对象。

```
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};

var hello = a.b.m;
hello() // undefined

```

上面代码中，`m`是多层对象内部的一个方法。为求简便，将其赋值给`hello`变量，结果调用时，`this`指向了顶层对象。为了避免这个问题，可以只将`m`所在的对象赋值给`hello`，这样调用时，`this`的指向就不会变。

```
var hello = a.b;
hello.m() // Hello

```

使用注意点
-----

（1）避免多层 this

由于`this`的指向是不确定的，所以切勿在函数中包含多层的`this`。

```
var o = {
  f1: function () {
    console.log(this);
    var f2 = function () {
      console.log(this);
    }();
  }
}

o.f1()
// Object
// Window

```

上面代码包含两层`this`，结果运行后，第一层指向该对象，第二层指向全局对象。实际执行的是下面的代码。

```
var temp = function () {
  console.log(this);
};

var o = {
  f1: function () {
    console.log(this);
    var f2 = temp();
  }
}

```

一个解决方法是在第二层改用一个指向外层`this`的变量。

```
var o = {
  f1: function() {
    console.log(this);
    var that = this;
    var f2 = function() {
      console.log(that);
    }();
  }
}

o.f1()
// Object
// Object

```

上面代码定义了变量`that`，固定指向外层的`this`，然后在内层使用`that`，就不会发生`this`指向的改变。

事实上，使用一个变量固定`this`的值，然后内层函数调用这个变量，是非常常见的做法，有大量应用，请务必掌握。

JavaScript 提供了严格模式，也可以硬性避免这种问题。在严格模式下，如果函数内部的`this`指向顶层对象，就会报错。

```
var counter = {
  count: 0
};
counter.inc = function () {
  'use strict';
  this.count++
};
var f = counter.inc;
f()
// TypeError: Cannot read property 'count' of undefined

```

上面代码中，`inc`方法通过`'use strict'`声明采用严格模式，这时内部的`this`一旦指向顶层对象，就会报错。

（2）避免数组处理方法中的this

数组的`map`和`foreach`方法，允许提供一个函数作为参数。这个函数内部不应该使用`this`。

```
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    this.p.forEach(function (item) {
      console.log(this.v + ' ' + item);
    });
  }
}

o.f()
// undefined a1
// undefined a2

```

上面代码中，`foreach`方法的回调函数中的`this`，其实是指向`window`对象，因此取不到`o.v`的值。原因跟上一段的多层`this`是一样的，就是内层的`this`不指向外部，而指向顶层对象。

解决这个问题的一种方法，是使用中间变量。

```
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    var that = this;
    this.p.forEach(function (item) {
      console.log(that.v+' '+item);
    });
  }
}

o.f()
// hello a1
// hello a2

```

另一种方法是将`this`当作`foreach`方法的第二个参数，固定它的运行环境。

```
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    this.p.forEach(function (item) {
      console.log(this.v + ' ' + item);
    }, this);
  }
}

o.f()
// hello a1
// hello a2

```

（3）避免回调函数中的this

回调函数中的`this`往往会改变指向，最好避免使用。

```
var o = new Object();

o.f = function () {
  console.log(this === o);
}

o.f() // true

```

上面代码表示，如果调用`o`对象的f方法，其中的`this`就是指向`o`对象。

但是，如果将`f`方法指定给某个按钮的`click`事件，this的指向就变了。

```
$('\#button').on('click', o.f);

```

点击按钮以后，控制台会显示`false`。原因是此时`this`不再指向`o`对象，而是指向按钮的DOM对象，因为`f`方法是在按钮对象的环境中被调用的。这种细微的差别，很容易在编程中忽视，导致难以察觉的错误。

为了解决这个问题，可以采用下面的一些方法对`this`进行绑定，也就是使得`this`固定指向某个对象，减少不确定性。