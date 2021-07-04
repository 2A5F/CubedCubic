# CubedCubic
# 基础

### 变量定义

```
let a = 1

mut b = 1
alt b + 1
```

### 模块

```
mod {
  let a = 1
  let b = mod { }
}
```

### 协议

```
mod foo {
 mut a = 0
 pact add v = alt a + v
 pact inc = add 1
}
foo:add 1
foo:inc
```

协议的调用，实际上是发送消息  
所有消息发送都是异步的  
但并不是完全并行的  
以模块为单位队列执行  
先发送的消息肯定先处理  

如果收集了消息的返回值  
将会自动中断等待消息的执行  
或者也可以使用 `!` 显式标注  

消息默认返回单元  

```
let _ = foo:inc
foo!:inc
```

### 实列化

本质上，所有模块都是实例  
但是复制一个现有模块而不是完全静态定义很有用  
它提供了动态性  

```
mod foo {
  let a = 1
  pact new = mod {
    ...foo
    pact add v = a + v
  }
}
let bar = foo:new
let v = bar:add 1
```


