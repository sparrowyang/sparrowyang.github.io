---
tags : [ "enum","rust","rustlings"]
categories : ["rust"]
author: "sparrowyang"
title: "Rust中的枚举类2"
date: "2021-05-30"
draft: false 
---

好家伙，我还是低估了这奇怪的语法。

首先枚举里每个元素都是不同类型，可以由下面的几个trait推断出，行吧，这也不算奇怪。


```diff
enum Message {
    // TODO: implement the message variant types based on their usage below
+   ChangeColor(u8, u8, u8),
+   Echo(String),
+   Move(Point),
+   Quit
}
```

其次根据`process`函数的调用，推出了函数大致的功能是判断出`message`的枚举值，然后做出相应的赋值。

## 错误写法
一开始我这样写：
```rust
match message {
    Message::ChangeColor => self.change_color(message),
    Message::Echo => self.echo(message),
    Message::Move => self.move_position(message),
    Message::Quit => self.quit()
}
```

## 正确写法
编译不通过，因为`process`函数的参数不是枚举，但我不知道怎么获取到message里各种类型的值。弄了半天，还是考靠百度解决了。
正确做法，在枚举后使用括号获得元素值，这个值和`message`是怎么联系上的？？：
```rust
match message {
    Message::ChangeColor(r,g,b) => self.change_color((r,g,b)),
    Message::Echo(s) => self.echo(s),
    Message::Move(p) => self.move_position(p),
    Message::Quit => self.quit()
}
```


## 完整代码

```rust
// enums3.rs

enum Message {
    // TODO: implement the message variant types based on their usage below
    ChangeColor(u8, u8, u8),
    Echo(String),
    Move(Point),
    Quit
}

struct Point {
    x: u8,
    y: u8
}

struct State {
    color: (u8, u8, u8),
    position: Point,
    quit: bool
}

impl State {
    fn change_color(&mut self, color: (u8, u8, u8)) {
        self.color = color;
    }

    fn quit(&mut self) {
        self.quit = true;
    }

    fn echo(&self, s: String) {
        println!("{}", s);
    }

    fn move_position(&mut self, p: Point) {
        self.position = p;
    }

    fn process(&mut self, message: Message) {
        // TODO: create a match expression to process the different message variants

        match message {
            Message::ChangeColor(r,g,b) => self.change_color((r,g,b)),
            Message::Echo(s) => self.echo(s),
            Message::Move(p) => self.move_position(p),
            Message::Quit => self.quit()
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_match_message_call() {
        let mut state = State{
            quit: false,
            position: Point{ x: 0, y: 0 },
            color: (0, 0, 0)
        };

        state.process(Message::ChangeColor(255, 0, 255));
        state.process(Message::Echo(String::from("hello world")));
        state.process(Message::Move(Point{ x: 10, y: 15 }));
        state.process(Message::Quit);

        assert_eq!(state.color, (255, 0, 255));
        assert_eq!(state.position.x, 10);
        assert_eq!(state.position.y, 15);
        assert_eq!(state.quit, true);
    }

}
```