```js

    //同源策略
    var tem=document.createElement("script");
    tem.type="text/script";
    tem.src="jsonp.js>callback=getData";
    
    document.getElementsByTagName("head")[0].appendChild(tem);
    function getData(data){
        console.log(JSON.stringify(data));
    }
    
```