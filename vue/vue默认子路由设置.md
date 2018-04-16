```javascript
export default new Router({
  routes: [
    {
      path: '/hello',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/',
      redirect: '/index'
    },
    {
      path: '/index',
      // name: 'Index', 要有默认子路由，那父路由的名字name得去掉
      component: Index,
      children: [
        {
          path: '/',
          name: 'Home',
          component: Home
        },
        {
          path: 'login',
          name: 'Login',
          component: Login
        }
      ]
    }
  ]
})
```