---
title: "Rust中的枚举类"
date: "2021-05-28"
tags : [ "enum","rust"]
categories : ["rust"]
draft: false 
---

这关试了很久，终于过了。

**枚举**中嵌套了**结构体**和**元组**。
由`Message::Move{ x: 10, y: 30 }`可知，`Move`是一个结构体，Move定义在`Message`里，所以`Move`是一个匿名结构体。
`Echo`和`ChangeColor`是元组，元组的定义只要加元素类型即可。
`Quit`是一个普通枚举元素。

可参考文档[Enum](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html)
```rust
// enums2.rs
// Make me compile! Execute `rustlings hint enums2` for hints!

#[derive(Debug)]
enum Message {
    // TODO: define the different variants used below
    Move{
        x:i32,
        y:i32
    },
    Echo(String),
    ChangeColor(
        i32,
        i32,
        i32
    ),
    Quit
}
impl Message {
    fn call(&self) {
        println!("{:?}", &self);
    }
}
fn main() {
    let messages = [
        Message::Move{ x: 10, y: 30 },
        Message::Echo(String::from("hello world")),
        Message::ChangeColor(200, 255, 255),
        Message::Quit
    ];
    for message in &messages {
        message.call();
    }
}

```
