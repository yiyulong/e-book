## 核心代码展示

html
```html
<div class="carousel">
  <figure>
    <img>
    <img>
    <img>
  </figure>
</div>
```
css
```css
.carosel {
  perspective: 500px;
}
figure {
  transform-style: preserve-3d;
}
```
script
```javascript
var theta = 2 * Math.PI / n; // 弧度rad
var n = images.length;
var s = parseFloat(getComputedStyle(images[0]).width);

function setCarousel (n, s) {
  var apothem = s / (2 * Math.tan(Math.PI / n));    // 边心距
  
  figure.style.transformOrigin = '50% 50% ' + -apothem + 'px';
  
  for (i = 0; i < n; i++) {
    images[i].style.transformOrigin = '50% 50% ' + -apothem + 'px';
    images[i].style.transform = 'rotateY(' + i * theta + 'rad)';
  }
}

function rotateCarousel(imageIndex) {
	figure.style.transform = 'rotateY(' + imageIndex * -theta + 'rad)';
}
```