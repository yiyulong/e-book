## 效果:
![IMAGE](resources/B7AE74E44D418847461C344D6B3F0269.jpg =400x180)

## 原理:  [原文链接](http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)

html结构
```html
<div class="stage"><!--舞台-->
  <div class="container"><!--容器-->
    <img>
    <img>
    <img>
    <img>
    <img>
    ...
  </div>
<div>
```
对于舞台加个视距：
```css
prespective: 800px;
```
对于容器加个3D识图声明：
```css
transform-style: preserve-3d;
```
然后就是图片们了。为了不至于产生类似DNA的螺旋状效果，我们让所有图片`position:absolute`，公用同一个中心点。
因此，图片`rotateY`值正好0~360等分
```css
img:nth-child(1) { transform: rotateY(   0deg ); }
img:nth-child(2) { transform: rotateY(  40deg ); }
img:nth-child(3) { transform: rotateY(  80deg ); }
img:nth-child(4) { transform: rotateY( 120deg ); }
img:nth-child(5) { transform: rotateY( 160deg ); }
img:nth-child(6) { transform: rotateY( 200deg ); }
img:nth-child(7) { transform: rotateY( 240deg ); }
img:nth-child(8) { transform: rotateY( 280deg ); }
img:nth-child(9) { transform: rotateY( 320deg ); }
```
每个图片的方位不一样，但都站在同一个点上,我们需要拉开空间
> 假设一共有9张图片，图片的宽度为128像素

![IMAGE](resources/228C7644743ECCC485E71C95FF9AFED8.jpg =340x200)
上图中红色标注的`r`就是的demo页面中图片要`translateZ`的理想值（该值可以让所有图片无缝围成一个圆）
```javascript
r = 64 / Math.tan(20 / 180 * Math.PI) ≈ 175.8
```
> demo页面为了好看，图片之间留了点间距，使用的`translateZ`的值为175.8 + 20 = 195.8.

最后，要让木马旋转起来，只要让容器每次旋转40度就可以了。