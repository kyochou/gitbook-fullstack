# 运维

## Blogs
* [运维之美](https://www.hi-linux.com/)

## 版本管理
### Refs
* [语义化版本](http://semver.org/)
* [node-semver](https://github.com/npm/node-semver)


## 故障处理
### Refs
* [如何应对在线故障](https://www.rowkey.me/blog/2018/11/22/online-debug/)

## Release
 各种 "部署/发布" 方式:
 * 蓝绿部署: 指同时运行应用的两个版本. 部署新版本后, 并不停掉老版本的应用. 等新版本运行一段时间确认没有问题后再停止老版本的应用. 这样一但新版本有问题就可以快速回滚到老版本的应用.   
 * 灰度发布(金丝雀发布): 部署后, 并不直接一下把所有流量都切到新版本. 而是分批次转移流量到新版本上.

### Refs
 * [蓝绿部署、金丝雀发布（灰度发布）、A/B测试的准确定义](https://www.lijiaocn.com/%E6%96%B9%E6%B3%95/2018/10/23/devops-blue-green-deployment-ab-test-canary.html)