JSON格式的attr访问 - 点号或者中括号 -注意：中括号里面可以放变量

```js
var imgData={ "name":"图片描述和图片相对路径", "title":"logo图片","url":"logo.png" };
 alert( imgData.name );
 alert( imgData["name"] ); 
 var str="name" ;  alert( imgData[str] ); 
```

for-in 访问json属性名称，属性值

```js
<script>
     var json1= {
        "name":"july",
        "age":18,
        "imgSrc":["1.png","2.png","3.png" ]
     };
 
 /* for-in 访问属性名称： 
            name
            age
            imgSrc*/
     for( var attr in json1){
        console.log( attr);
        }
  /* for-in 访问属性值：
       july
       18
       ["1.png", "2.png", "3.png"]
        */
    
     for( var attr in json1){
        console.log( json1[attr]); 
  
     }
</script>
```