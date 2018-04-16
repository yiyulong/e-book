[CSS百分比padding实现比例固定图片自适应布局](http://www.zhangxinxu.com/wordpress/?p=6348)
```css
.img-wrap{
      width: 60%; /* 这里设置占屏幕宽度百分比 */
      margin: 0 auto; 
  }
  
  /* 创建一个正方形容器 */
  .img-container{
      width: 100%;
      height: 0px;
      padding-bottom: 100%;
      overflow:hidden;
      margin: 0;
      position:relative;
  }

  /* 采用绝对定位 */
  .img-wrap img{
      position:absolute;
      width: 100%;
      height: 100%;
  }
```
```html
<body> 
    
    <div class='img-wrap'>
        <div class="img-container">
            <img src="3.png" />
        </div>
    </div>
    
 </body>
```