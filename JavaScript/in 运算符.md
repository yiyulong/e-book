测试一个对象中是否存在一种属性。
## 语法
```
result = property in object
```
## 参数

> *result*：必需。任何变量。
*property*：必须。计算结果为字符串表达式的表达式。
*object*：必须。任意对象。

## 事例
```javascript
// Create an object that has some properties.
var myObject = new Object();
myObject.name = "James";
myObject.age = "22";
myObject.phone = "555 0234";

if ("phone" in myObject)
   document.write ("property is present");
else
   document.write ("property is not present");

// Output: property is present
```