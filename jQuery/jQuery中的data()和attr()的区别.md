html
```html
<div data-myname></div>
```
script
```javascript
// get属性值
var getAttr = $('div').attr('data-myname');
var getData = $('div').data('myname');

// set属性值
var setAttr = $('div').attr('data-myname', 'yyl');
var setData = $('div').data('myname', 'yyl');
```
* $.attr() 每次都从 `DOM元素` 中取属性的值，即和视图中标签内的属性值保持一致。
  * $.attr('data-myname') 会从标签内获得data-myname属性值；
  * $.attr('data-myname', 'world') 会将字符串'world'塞到标签的'data-myname'属性中；
* $.data() 是从 `Jquery对象` 中取值，由于对象属性值保存在内存中，因此可能和视图里的属性值不一致的情况。
  * $.data('myname') 会从 Jquery对象 myname，不是从标签内获得data-myname； 
  * $.data('myname', 'world') 会将字符串'world'塞到 Jquery对象 的'myname'属性中，而不是塞到视图标签的data-myname属性中。
  
总结：
> 从性能的角度来说，建议使用$.data()来进行set和get操作，因为它仅仅修改的 Jquey对象 的属性值，不会引起额外的DOM操作。