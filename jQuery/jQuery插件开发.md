# 理解jquery的`$.extend()`、`$.fn` 和 `$.fn.extend()`
---

jQuery为开发插件提拱了两个方法，分别是：

* $.fn.extend();
* $.extend();

## $.fn

```javascript
$.fn = jQuery.prototype = {
  init: function( selector, context ) {
    // ...
  }
  //……
};
```

> 原来 jQuery.fn = jQuery.prototype.对prototype肯定不会陌生啦。
> 虽然 javascript　没有明确的类的概念，但是用类来理解它，会更方便。
> jQuery便是一个封装得非常好的类，比如我们用 语句　$(“#btn1″) 会生成一个 jQuery类的实例。

## $.extend(object)

为jQuery类添加类方法，可以理解为添加静态方法。如：

```javascript
$.extend({
  min: function(a, b) { return a < b ? a : b; },
  max: function(a, b) { return a > b ? a : b; }
});
$.min(2,3); //  2 
$.max(4,5); //  5
```

## $.extend( target, object1, [objectN])

用一个或多个其他对象来扩展一个对象，返回被扩展的对象

**注意：**
> 如果只为$.extend()指定了一个参数，则意味着参数target被省略。此时，target就是jQuery对象本身。通过这种方式，我们可以为全局对象jQuery添加新的函数。
> 如果多个对象具有相同的属性，则后者会覆盖前者的属性值。

### 语法

```javascript
$.extend( target [, object1 ] [, objectN ] )
```
是否深合并
```javascript
$.extend( [deep ], target, object1 [, objectN ] )
```
例：
```javascript
var defaults = { validate: false, limit: 5, name: "foo" };
var options = { validate: true, name: "bar" };
/* 合并默认值和选项, 不修改默认对象 */
var settings = $.extend({}, defaults, options);
/**
* 结果
* defaults -- {"validate":false,"limit":5,"name":"foo"}
* options -- {"validate":true,"name":"bar"}
* settings -- {"validate":true,"limit":5,"name":"bar"}
*／
```
> **警告**: 不支持第一个参数传递 false 。



```javascript
var settings = { validate: false, limit: 5, name: "foo" }; 
var options = { validate: true, name: "bar" }; 
$.extend(settings, options); 
//结果：settings == { validate: true, limit: 5, name: "bar" }
```

## $.fn.extend(object);

对jQuery.prototype进得扩展，就是为jQuery类添加“成员函数”。jQuery类的实例可以使用这个“成员函数”。
比如我们要开发一个插件，做一个特殊的编辑框，当它被点击时，便alert 当前编辑框里的内容。可以这么做：

```javascript
$.fn.extend({          
  alertWhileClick:function() {            
    $(this).click(function(){                 
      alert($(this).val());           
    });           
  }       
});       
$("#input1").alertWhileClick(); // 页面上为：
```

$("#input1")　为一个jQuery实例，当它调用成员方法 alertWhileClick后，便实现了扩展，每次被点击时它会先弹出目前编辑里的内容。

> jQuery.extend() 的调用并不会把方法扩展到对象的实例上，引用它的方法也需要通过jQuery类来实现，如jQuery.init()，而jQuery.fn.extend()的调用把方法扩展到了对象的prototype上，所以实例化一个jQuery对象的时候，它就具有了这些方法，这是很重要的，在jQuery.js中到处体现这一点

## jQuery.fn.extend = jQuery.prototype.extend

你可以拓展一个对象到jQuery的 prototype里去，这样的话就是插件机制了。

```javascript
(function( $ ){
  $.fn.tooltip = function( options ) {};
  //等价于
  var tooltip = {
    function(options){}
  };
  $.fn.extend(tooltip) = $.prototype.extend(tooltip) = $.fn.tooltip
})( jQuery );
```
完整案例
```javascript
$.fn.myscroll = function(opations) {
  var defaults = {
    picElem: this.find('div').first(),
    playTime: 2000,
    effect: 'left',
  }
  
  var settings = $.extend({}, defaults, opations);
  
  return function () {
    var $moveWrap = settings.picElem.find('ul');
    var $playTime = settings.playTime;
    var $effect = settings.effect;
    
    function init () {
      // ...
    }
  }();
}
```