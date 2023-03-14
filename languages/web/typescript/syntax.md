# Syntax

```typescript

// 定义变量(`const`, `var`, `let`)
const name:string = `kyo` 
const name = `kyo` // TSC 可自动推断类型


// 定义函数
function greet(name:string) void {
    return `hello` + name
}

```

## Variables
声明变量(`const`, `var`, `let`).

### Types
`string`, `number`, `boolean`, `T[]`(数组).
`any` 指任意类型. 当你没指定类型, 并且 TSC(typescript 编译器) 不能从上下文推断它的类型时, TSC 通常会将其类型默认为 `any`. 
