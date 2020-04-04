# Interface
在 Go 语言中, 接口定义了一套方法的集合, 任何实现这些方法的对象都可以被认为实现了这个接口, 这也称作 `Duck Type`. 这不像其他语言比如 java 需要预先声明类型实现了某个或某些接口, 这使得 Go 语言的接口和类型变得很轻量级, 它解耦了接口和具体实现的硬绑定.    

> If something looks like a duck, swims like a duck and quacks like a duck then it's probably a duck.   