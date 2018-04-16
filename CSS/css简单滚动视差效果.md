```css
.container {
    /* 滚动容器 */
    perspective: 1px; 
    padding: 0; height: calc(100vh - 300px); overflow: auto;
}
.box {
    /* 视差元素的父级需要3D视角 */
    height: 1280px;
    transform-style: preserve-3d;
    position: relative;
}
.background {
    /* 滚动比较慢的背景元素 */
    position: absolute; left: 50%;
    transform: translate3D(-50%, -120px, -1px) scale(2);
}
```
[原文链接](http://www.zhangxinxu.com/wordpress/?p=4720)