# gops
## API
```shell
# 第一步是为用户设置 token
# 后面所有 API 的使用都需要使用 Token 认证
echo '{"name": "ops.kyo"}'  | http -a ops.kyo:pwd POST http://host/api/v1/users/ops.kyo/tokens
```

### Refs
* [gogs/docs-api](https://github.com/gogs/docs-api)