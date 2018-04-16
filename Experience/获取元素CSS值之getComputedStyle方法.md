`getComputedStyle` 是一个可以获取当前元素所有最终使用的CSS属性值。返回的是一个CSS样式声明对象([object CSSStyleDeclaration])，只读。

### 语法
```javascript
var style = window.getComputedStyle("元素", "伪类");
```
### 例如:
```javascript
var dom = document.getElementById("test"),
style = window.getComputedStyle(dom , ":after");
```
> 就两个参数，只是额外提示下：Gecko 2.0 (Firefox 4 / Thunderbird 3.3 / SeaMonkey 2.1)之前，第二个参数“伪类”是必需的（如果不是伪类，设置为null），不过现在嘛，不是必需参数了。

### getComputedStyle与style的区别
---
#### 1. 只读与可写
* 正如上面提到的`getComputedStyle`方法是只读的，只能获取样式，不能设置；而`element.style`能读能写，能屈能伸。
#### 2. 获取的对象范围
* `getComputedStyle`方法获取的是最终应用在元素上的所有CSS属性对象（即使没有CSS代码，也会把默认的祖宗八代都显示出来）；
* 而element.style只能获取元素style属性中的CSS样式。因此对于一个光秃秃的元素`<p>`，`getComputedStyle`方法返回对象中length属性值（如果有）就是`190+`(据我测试FF:192, IE9:195, Chrome:253, 不同环境结果可能有差异), 而`element.style`就是0。

### getComputedStyle与currentStyle
---
`currentStyle` 是IE浏览器自娱自乐的一个属性，其与`element.style` 可以说是近亲，至少在使用形式上类似，差别在于`element.currentStyle` 返回的是元素当前应用的最终CSS属性值（包括外链CSS文件，页面中嵌入的`<style>`属性等）。

因此，从作用上将，`getComputedStyle` 方法与`currentStyle` 属性走的很近，形式上则`style` 与`currentStyle` 走的近。不过，`currentStyle` 属性貌似不支持伪类样式获取，这是与`getComputedStyle` 方法的差异，也是jQuery css()方法无法体现的一点。


例如，我们要获取一个元素的高度，可以类似下面的代码：
```javascript
alert((element.currentStyle? element.currentStyle : window.getComputedStyle(element, null)).height);
```
`getComputedStyle` 方法与`currentStyle` 属性其他具体差异还有很多,具体点击[demo](http://www.zhangxinxu.com/study/201205/currentstyle-getcomputedstyle-test.html)
仔细对比查看，我们可以看到不少差异，例如浮动属性，FireFox浏览器下是这个(`cssFloat`)
![IMAGE](resources/AABEC13A7CCE614BB06D99E4C047A557.jpg =227x107)
IE7浏览器下则是`styleFloat` ：
![IMAGE](resources/1617D815A2F1D73D3DC3CF47E54F5218.jpg =263x136)
而IE9浏览器下则是`cssFloat` 和`styleFloat` 都有。

### getPropertyValue方法
---
`getPropertyValue` 方法可以获取CSS样式申明对象上的属性值（直接属性名称），例如：
```javascript
window.getComputedStyle(element, null).getPropertyValue("float");
```
> 使用getPropertyValue方法不必可以驼峰书写形式（不支持驼峰写法），例如：*style.getPropertyValue("border-top-left-radius");*

### getPropertyValue和getAttribute
---
在老的IE浏览器（包括最新的），`getAttribute` 方法提供了与`getPropertyValue` 方法类似的功能，可以访问CSS样式对象的属性。用法与`getPropertyValue` 类似：
```javascript
style.getAttribute("float");
```
注意到没，使用`getAttribute` 方法也不需要`cssFloat` 与`styleFloat` 的怪异写法与兼容性处理。不过，还是有一点差异的，就是属性名需要驼峰写法，如下：
```javascript
style.getAttribute("backgroundColor");
```
如果不考虑IE6浏览器，貌似也是可以这么写：
```javascript
style.getAttribute("background-color");
```
[demo点击查看](http://www.zhangxinxu.com/study/201205/getpropertyvalue-getAttribute-background-color.html)打包
结果FireFox下一如既往的rgb颜色返回(Chrome也是返回rgb颜色)：
![IMAGE](resources/342F33697EC90714AD44E460A31921DC.jpg =340x120)
对于IE9浏览器，虽然应用的是`currentStyle`, 但是从结果上来讲，`currentStyle` 返回的对象是完全支持`getPropertyValue` 方法的。
![IMAGE](resources/7679A242A2A5ABAC74B692891A4CBBBD.jpg =340x120)