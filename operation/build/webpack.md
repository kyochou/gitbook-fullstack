# Webpack

## 环境变量
一些工具: 
* [env-cmd](https://github.com/toddbluhm/env-cmd)
* [dotenv](https://github.com/motdotla/dotenv)
* [cross-env](https://github.com/kentcdodds/cross-env)
* [fuck-env](https://github.com/cnlon/fuck-env)

环境变量在编译时会进行字符替换. 所以编译后都是以字面值的形式存在的(不再是变量). 如果想在运行时改变, 可以考虑将变量绑定在全局对象(如 window)上.

### Vuejs
Vue CLI 3 内置了对环境变量的支持. 参考 [环境变量和模式](https://cli.vuejs.org/zh/guide/mode-and-env.html).

### Refs
* [如何更好的管理前端环境变量](https://lon.im/post/use-environment-variables-better-in-front_end.html)