## 文字的阴影效果
![IMAGE](resources/C893D0A0D88604BF37A87BAD144879FF.jpg =470x70)
css
```css
.zxx_show{padding:20px; background:#f0f3f9; color:#aaaaaa; font-size:14px;}
.zxx_text{position:absolute; margin:-1px 0 0 -1px; color:#333333;}
```
html
```html
<div class="zxx_show">
    <span class="zxx_text">这是一段用来测试的文字，看看是否有投影效果~~</span>
    这是一段用来测试的文字，看看是否有投影效果~~
</div>
```
## 自适应布局
![IMAGE](resources/8F09A27ABC5AC950DAD67C3DA034C3C5.jpg =402x69)
由于头像的宽度固定，所以对于描述标签，我们可以使用margin或是padding撑开一段距离，头像使用无定位值的absolute定位，这样就实现了头像与描述的自适应布局效果了

> `absolute`绝对定位的非绝对定位应用肯定还有其他，只要记住无定位值的absolute元素就是个连实际宽度也没有的float浮动元素就可以了，然后利用这个特性，发挥您的创造力，实现更多更精彩的效果吧。