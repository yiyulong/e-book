```html
<div data-drink="coffee" data-food="sushi" data-meal="lunch"></div>
```
获取data
```javascript
var divObj = document.getElementByTagName('div')[0];
var typeOfDrink = divObj.dataset;
```
设置data
```javascript
// 删除一个data属性
delete divObj.dataset.meal;
```
```javascript
// 给元素添加一个属性
divObj.dataset.dessert = 'icecream';
```