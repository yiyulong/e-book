```javascript
var fn1 = function() {
  var num = 0
  console.log(num)
  return Promise.resolve(++num)
}

fn1()  // 0
fn1().then(function(data) {
  console.log(data)
})
/*
*  0
*  1
*/
```