# 日志
## 实践
### 级别
Fatal, Error, Warn, Info, Debug, Trace(Verbose)

#### Refs
[日志的 5 个级别](http://www.infoq.com/cn/articles/five-levels-of-logging)

## Tools
### [Sentry](https://github.com/getsentry/sentry)
Sentry is cross-platform application monitoring, with a focus on error reporting.
主要用来进行错误的收集, 可通过 `captureEvent` 实现自定义数据的收集.
```javascript
// Event 的定义参考 https://github.com/getsentry/sentry-javascript/blob/master/packages/types/src/index.ts
Sentry.captureEvent({
    message: '10',
    level: 'debug',
    tags: {type: 'pageLoadedTime'},
})
```

#### Refs
* [前端异常监控平台对比](https://www.jianshu.com/p/900e638648a7)