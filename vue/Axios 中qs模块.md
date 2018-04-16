## 作用
只做个一个工作就是把data、里的参数转化成键值对


## 示例
```javascript
import qs from 'querystring'
// http请求拦截器
axios.interceptors.request.use(
  config => {
    // 修改了axios的post调用方法，将post参数转化成键值对
    if (config.method === 'post' || config.method === 'put') {
      config.data = qs.stringify(config.data)
    }
    return config
  },
  error => {
    return Promise.reject(error)
  })
```