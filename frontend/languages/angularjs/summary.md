# AngularJS
## Faqs
* Provider, Factory, Service 的区别

    ```js
    $provide.provider('myDate', {
        $get: function() {
          return new Date();
        }
    });
    // 可以写成
    $provide.factory('myDate', function(){
        return new Date();
    });
    // 可以写成
    $provide.service('myDate', Date);
    ```

* Value 与 Constant 的区别
    * value 可以被修改, constant 一旦声明无法被修改.
    * value 不可在 config 里注入, constant 可以.
    
## Refs
[[AngularJS系列(4)] 那伤不起的provider们啊~ (Provider, Value, Constant, Service, Factory, Decorator)](http://hellobug.github.io/blog/angularjs-providers/)