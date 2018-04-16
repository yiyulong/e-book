前端开发中的touchstart, touchmove, touchend三个事件，点击元素并拖动时，获取到了touchmove事件， 但是event.touches[0].target所指向的元素却是touchstart时获取到的元素，而并非手指所移动到上方的元素，所以在处理获取移动到哪个元素上方之类的需求时，普通方法已不奏效，解决方式如下：
```javascript
var element = document.elementFromPoint(event.originalEvent.pageX, event.originalEvent.pageY);
```


这样获取到的即为所需元素。

备注：不同版本可能获取方式不同，有的是利用下面的代码来获取
```javascript
document.elementFromPoint(event.clientX, event.clientY);
```

```javascript
onElementTouchMove: function(e) {  
  if (Ext.browser.is.ChromeMobile) {  
      var x = e.changedTouches[0].screenX;  
      var y = e.changedTouches[0].screenY;  
  } else {  
      var x = e.pageX;  
      var y = e.pageY  
  }  
  var target = document.elementFromPoint(x, y);
}
```