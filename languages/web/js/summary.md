# Javascript

## FAQs
* 可使用代理对象保证属性的不可变性
    
    ```js
    var handler = {
      get: function(obj, prop) {
        switch (prop) {
          case 'login': {
            return {
              name: 'admin',
              password: 'admin',
              client_id: 1,
            }
          }
        }
      },
    }
    
    var api = new Proxy({}, handler)
    // 此处的修改并不会影响原数据
    api.login.name = 'kyo'
    // 输出 admin
    console.log(api.login.name)

    ```
    
    * js 的对象方法可直接用属性的方式调用

        ```js
        const req = request.post(path)
        // 等同于
        let method = 'post'
        const req = request[method](path)
        ```