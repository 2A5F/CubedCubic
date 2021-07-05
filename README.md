# CubedCubic

对面向对象的概念研究

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
但是执行的顺序不是固定的  
如果两个模块在不同的机器上可能会由于物理原因导致乱序  

消息默认返回单元类型  

如果使用了消息的返回值  
将会自动中断等待消息的执行  
使用 `let _ =` 将会立即使用也就是立即等待消息返回  
或者也可以使用 `!` 显式标注来等待  

```
let _ = foo:inc
foo!:inc
```

消息的返回值是惰性的  
只有在实际使用时才会产生等待  

```
let a = foo:bar
let b = foo:bar
let _ = a + b     # 在这条语句时才会产生等待，将并行执行 a 和 b，但不是到这条语句才开始执行 a 和 b，消息的发送是立即的，等待是惰性的  
```

消息中断等待时模块将会继续执行下一队列的消息  

### 实列化

本质上，所有模块都是实例  
但是复制一个现有模块而不是完全静态定义很有用  
它提供了动态性  

```
mod foo {
    let a = 1
    pact new = mod {
        ...foo
        pact add v = self.a + v   # 直接使用 a 将会使用 foo 上的 a，使用 self 将使用自己的 a
    }
}
let bar = foo:new
let v = bar:add 1
```

也可以选择不使用复制只是单纯返回一个新模块  
如果是不可以的变量就没有必要复制到实例上  

```
mod foo {
    let a = 1
    pact new = mod {
      pact add v = a + v
    }
}
```




