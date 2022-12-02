# Visual Studio Code

## Settings

* Creating your own snippets: `Code > Preferences > User Snippets`




### User Settings
```js
{
    // terminal
    "terminal.external.osxExec": "iTerm.app",
    "terminal.integrated.copyOnSelection": true
}
```


## PHP
* 为方法内定义变量开启提示功能(`@var Type name`):
    
    ```php
    /** @var Response $respone */
    $respone = $next($request);
    ```