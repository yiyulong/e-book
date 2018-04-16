`this`的动态切换，固然为JavaScript创造了巨大的灵活性，但也使得编程变得困难和模糊。有时，需要把`this`固定下来，避免出现意想不到的情况。JavaScript提供了`call`、`apply`、`bind`这三个方法，来切换/固定`this`的指向。

### function.prototype.call()

函数实例的`call`方法，可以指定函数内部`this`的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数。

```
var obj = {};

var f = function () {
  return this;
};

f() === this // true
f.call(obj) === obj // true

```

上面代码中，在全局环境运行函数`f`时，`this`指向全局环境；`call`方法可以改变`this`的指向，指定`this`指向对象`obj`，然后在对象`obj`的作用域中运行函数`f`。

`call`方法的参数，应该是一个对象。如果参数为空、`null`和`undefined`，则默认传入全局对象。

```
var n = 123;
var obj = { n: 456 };

function a() {
  console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(obj) // 456

```

上面代码中，`a`函数中的`this`关键字，如果指向全局对象，返回结果为`123`。如果使用`call`方法将`this`关键字指向`obj`对象，返回结果为`456`。可以看到，如果`call`方法没有参数，或者参数为`null`或`undefined`，则等同于指向全局对象。

如果`call`方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入`call`方法。

```
var f = function () {
  return this;
};

f.call(5)
// Number {[[PrimitiveValue]]: 5}

```

上面代码中，`call`的参数为`5`，不是对象，会被自动转成包装对象（`Number`的实例），绑定`f`内部的`this`。

`call`方法还可以接受多个参数。

```
func.call(thisValue, arg1, arg2, ...)

```

`call`的第一个参数就是`this`所要指向的那个对象，后面的参数则是函数调用时所需的参数。

```
function add(a, b) {
  return a + b;
}

add.call(this, 1, 2) // 3

```

上面代码中，`call`方法指定函数`add`内部的`this`绑定当前环境（对象），并且参数为`1`和`2`，因此函数`add`运行后得到`3`。

`call`方法的一个应用是调用对象的原生方法。

```
var obj = {};
obj.hasOwnProperty('toString') // false

// 覆盖掉继承的 hasOwnProperty 方法
obj.hasOwnProperty = function () {
  return true;
};
obj.hasOwnProperty('toString') // true

Object.prototype.hasOwnProperty.call(obj, 'toString') // false

```

上面代码中，`hasOwnProperty`是`obj`对象继承的方法，如果这个方法一旦被覆盖，就不会得到正确结果。`call`方法可以解决这个问题，它将`hasOwnProperty`方法的原始定义放到`obj`对象上执行，这样无论`obj`上有没有同名方法，都不会影响结果。

### function.prototype.apply()

`apply`方法的作用与`call`方法类似，也是改变`this`指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数，使用格式如下。

```
func.apply(thisValue, [arg1, arg2, ...])

```

`apply`方法的第一个参数也是`this`所要指向的那个对象，如果设为null或undefined，则等同于指定全局对象。第二个参数则是一个数组，该数组的所有成员依次作为参数，传入原函数。原函数的参数，在`call`方法中必须一个个添加，但是在`apply`方法中，必须以数组形式添加。

请看下面的例子。

```
function f(x,y){
  console.log(x+y);
}

f.call(null,1,1) // 2
f.apply(null,[1,1]) // 2

```

上面的f函数本来接受两个参数，使用apply方法以后，就变成可以接受一个数组作为参数。

利用这一点，可以做一些有趣的应用。

（1）找出数组最大元素

JavaScript不提供找出数组最大元素的函数。结合使用apply方法和Math.max方法，就可以返回数组的最大元素。

```
var a = [10, 2, 4, 15, 9];

Math.max.apply(null, a)
// 15

```

（2）将数组的空元素变为`undefined`

通过`apply`方法，利用Array构造函数将数组的空元素变成undefined。

```
Array.apply(null, ["a",,"b"])
// [ 'a', undefined, 'b' ]

```

空元素与`undefined`的差别在于，数组的`forEach`方法会跳过空元素，但是不会跳过`undefined`。因此，遍历内部元素的时候，会得到不同的结果。

```
var a = ['a', , 'b'];

function print(i) {
  console.log(i);
}

a.forEach(print)
// a
// b

Array.apply(null, a).forEach(print)
// a
// undefined
// b

```

（3）转换类似数组的对象

另外，利用数组对象的`slice`方法，可以将一个类似数组的对象（比如`arguments`对象）转为真正的数组。

```
Array.prototype.slice.apply({0:1,length:1})
// [1]

Array.prototype.slice.apply({0:1})
// []

Array.prototype.slice.apply({0:1,length:2})
// [1, undefined]

Array.prototype.slice.apply({length:1})
// [undefined]

```

上面代码的`apply`方法的参数都是对象，但是返回结果都是数组，这就起到了将对象转成数组的目的。从上面代码可以看到，这个方法起作用的前提是，被处理的对象必须有length属性，以及相对应的数字键。

（4）绑定回调函数的对象

上一节按钮点击事件的例子，可以改写成

```
var o = new Object();

o.f = function () {
  console.log(this === o);
}

var f = function (){
  o.f.apply(o);
  // 或者 o.f.call(o);
};

$('\#button').on('click', f);

```

点击按钮以后，控制台将会显示`true`。由于`apply`方法（或者`call`方法）不仅绑定函数执行时所在的对象，还会立即执行函数，因此不得不把绑定语句写在一个函数体内。更简洁的写法是采用下面介绍的`bind`方法。

### function.prototype.bind()

`bind`方法用于将函数体内的`this`绑定到某个对象，然后返回一个新函数。

```
var d = new Date();
d.getTime() // 1481869925657

var print = d.getTime;
print() // Uncaught TypeError: this is not a Date object.

```

上面代码中，我们将`d.getTime`方法赋给变量`print`，然后调用`print`就报错了。这是因为`getTime`方法内部的`this`，绑定`Date`对象的实例，赋给变量`print`以后，内部的`this`已经不指向`Date`对象的实例了。

`bind`方法可以解决这个问题，让`log`方法绑定`console`对象。

```
var print = d.getTime.bind(d);
print() // 1481869925657

```

上面代码中，`bind`方法将`getTime`方法内部的`this`绑定到`d`对象，这时就可以安全地将这个方法赋值给其他变量了。

下面是一个更清晰的例子。

```
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};

counter.count // 0
counter.inc()
counter.count // 1

```

上面代码中，`counter.inc`内部的`this`，默认指向`counter`对象。如果将这个方法赋值给另一个变量，就会出错。

```
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};

var func = counter.inc;
func();
counter.count // 0
count // NaN

```

上面代码中，函数`func`是在全局环境中运行的，这时`inc`内部的`this`指向顶层对象`window`，所以`counter.count`是不会变的，反而创建了一个全局变量`count`。因为`window.count`原来等于`undefined`，进行递增运算后`undefined++`就等于`NaN`。

为了解决这个问题，可以使用`bind`方法，将`inc`内部的`this`绑定到`counter`对象。

```
var func = counter.inc.bind(counter);
func();
counter.count // 1

```

上面代码中，`bind`方法将`inc`方法绑定到`counter`以后，再运行`func`就会得到正确结果。

`this`绑定到其他对象也是可以的。

```
var obj = {
  count: 100
};
var func = counter.inc.bind(obj);
func();
obj.count // 101

```

上面代码中，`bind`方法将`inc`方法内部的`this`，绑定到`obj`对象。结果调用`func`函数以后，递增的就是`obj`内部的`count`属性。

`bind`比`call`方法和`apply`方法更进一步的是，除了绑定`this`以外，还可以绑定原函数的参数。

```
var add = function (x, y) {
  return x \* this.m + y \* this.n;
}

var obj = {
  m: 2,
  n: 2
};

var newAdd = add.bind(obj, 5);

newAdd(5)
// 20

```

上面代码中，`bind`方法除了绑定`this`对象，还将`add`函数的第一个参数`x`绑定成`5`，然后返回一个新函数`newAdd`，这个函数只要再接受一个参数`y`就能运行了。

如果`bind`方法的第一个参数是`null`或`undefined`，等于将`this`绑定到全局对象，函数运行时`this`指向顶层对象（在浏览器中为`window`）。

```
function add(x, y) {
  return x + y;
}

var plus5 = add.bind(null, 5);
plus5(10) // 15

```

上面代码中，函数`add`内部并没有`this`，使用`bind`方法的主要目的是绑定参数`x`，以后每次运行新函数`plus5`，就只需要提供另一个参数`y`就够了。而且因为`add`内部没有`this`，所以`bind`的第一个参数是`null`，不过这里如果是其他对象，也没有影响。

对于那些不支持`bind`方法的老式浏览器，可以自行定义`bind`方法。

```
if(!('bind' in Function.prototype)){
  Function.prototype.bind = function(){
    var fn = this;
    var context = arguments[0];
    var args = Array.prototype.slice.call(arguments, 1);
    return function(){
      return fn.apply(context, args);
    }
  }
}

```

`bind`方法有一些使用注意点。

（1）每一次返回一个新函数

`bind`方法每运行一次，就返回一个新函数，这会产生一些问题。比如，监听事件的时候，不能写成下面这样。

```
element.addEventListener('click', o.m.bind(o));

```

上面代码中，`click`事件绑定`bind`方法生成的一个匿名函数。这样会导致无法取消绑定，所以，下面的代码是无效的。

```
element.removeEventListener('click', o.m.bind(o));

```

正确的方法是写成下面这样：

```
var listener = o.m.bind(o);
element.addEventListener('click', listener);
// ...
element.removeEventListener('click', listener);

```

（2）结合回调函数使用

回调函数是JavaScript最常用的模式之一，但是一个常见的错误是，将包含`this`的方法直接当作回调函数。

```
var counter = {
  count: 0,
  inc: function () {
    'use strict';
    this.count++;
  }
};

function callIt(callback) {
  callback();
}

callIt(counter.inc)
// TypeError: Cannot read property 'count' of undefined

```

上面代码中，`counter.inc`方法被当作回调函数，传入了`callIt`，调用时其内部的`this`指向`callIt`运行时所在的对象，即顶层对象`window`，所以得不到预想结果。注意，上面的`counter.inc`方法内部使用了严格模式，在该模式下，`this`指向顶层对象时会报错，一般模式不会。

解决方法就是使用`bind`方法，将`counter.inc`绑定`counter`。

```
callIt(counter.inc.bind(counter));
counter.count // 1

```

还有一种情况比较隐蔽，就是某些数组方法可以接受一个函数当作参数。这些函数内部的`this`指向，很可能也会出错。

```
var obj = {
  name: '张三',
  times: [1, 2, 3],
  print: function () {
    this.times.forEach(function (n) {
      console.log(this.name);
    });
  }
};

obj.print()
// 没有任何输出

```

上面代码中，`obj.print`内部`this.times`的`this`是指向`obj`的，这个没有问题。但是，`forEach`方法的回调函数内部的`this.name`却是指向全局对象，导致没有办法取到值。稍微改动一下，就可以看得更清楚。

```
obj.print = function () {
  this.times.forEach(function (n) {
    console.log(this === window);
  });
};

obj.print()
// true
// true
// true

```

解决这个问题，也是通过`bind`方法绑定`this`。

```
obj.print = function () {
  this.times.forEach(function (n) {
    console.log(this.name);
  }.bind(this));
};

obj.print()
// 张三
// 张三
// 张三

```

（3）结合`call`方法使用

利用`bind`方法，可以改写一些JavaScript原生方法的使用形式，以数组的`slice`方法为例。

```
[1, 2, 3].slice(0, 1)
// [1]

// 等同于

Array.prototype.slice.call([1, 2, 3], 0, 1)
// [1]

```

上面的代码中，数组的`slice`方法从`[1, 2, 3]`里面，按照指定位置和长度切分出另一个数组。这样做的本质是在`[1, 2, 3]`上面调用`Array.prototype.slice`方法，因此可以用`call`方法表达这个过程，得到同样的结果。

`call`方法实质上是调用`Function.prototype.call`方法，因此上面的表达式可以用`bind`方法改写。

```
var slice = Function.prototype.call.bind(Array.prototype.slice);

slice([1, 2, 3], 0, 1) // [1]

```

可以看到，利用`bind`方法，将`[1, 2, 3].slice(0, 1)`变成了`slice([1, 2, 3], 0, 1)`的形式。这种形式的改变还可以用于其他数组方法。

```
var push = Function.prototype.call.bind(Array.prototype.push);
var pop = Function.prototype.call.bind(Array.prototype.pop);

var a = [1 ,2 ,3];
push(a, 4)
a // [1, 2, 3, 4]

pop(a)
a // [1, 2, 3]

```

如果再进一步，将`Function.prototype.call`方法绑定到`Function.prototype.bind`对象，就意味着`bind`的调用形式也可以被改写。

```
function f() {
  console.log(this.v);
}

var o = { v: 123 };

var bind = Function.prototype.call.bind(Function.prototype.bind);

bind(f, o)() // 123

```

上面代码表示，将`Function.prototype.call`方法绑定`Function.prototype.bind`以后，`bind`方法的使用形式从`f.bind(o)`，变成了`bind(f, o)`。