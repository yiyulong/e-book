水平翻转不同于翻转180度  [原文链接](http://www.zhangxinxu.com/wordpress/?p=1637)

    /\*水平翻转\*/
    .flipx {
        -moz-transform:scaleX(-1);
        -webkit-transform:scaleX(-1);
        -o-transform:scaleX(-1);
        transform:scaleX(-1);
        /\*IE\*/
        filter:FlipH;
    }

    /\*垂直翻转\*/
    .flipy {
        -moz-transform:scaleY(-1);
        -webkit-transform:scaleY(-1);
        -o-transform:scaleY(-1);
        transform:scaleY(-1);
        /\*IE\*/
        filter:FlipV;
    }

其中，就目前而言，对于基于webkit核心的浏览器，如Chrome以及Safari，实现元素的垂直翻转或是水平翻转也可以使用如下样式：

    /\*水平翻转\*/
    .flipx { transform: rotateY(180deg); }

    /\*垂直翻转\*/
    .flipy { transform: rotateX(180deg); }

**注意：**

1. 水平翻转或垂直翻转不同于旋转180度。前者以轴为镜像，后者以点为镜像。
2. 如果是对称元素，旋转180度和翻转的显示效果基本上就是一致的，但是，非对称元素就会看到明显差异。
3. 对于后面提到的目前仅webkit核心浏览器支持的rotateY(180deg)，这里有个大写的Y。我们都知道Y表示纵轴，所以我们可能会误以为这里实现的是垂直翻转，其实非也，这里的Y表示元素以纵轴为镜像翻转，也就是水平翻转了。