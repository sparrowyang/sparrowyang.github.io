---
title: "《精通Linux》笔记"
date: "2021-06-26"
tags : ["linux"]
categories : ["读书笔记"]
author: "sparrowyang"
draft: ture 

---

>很久之前就读了这本书，当时略看了一遍，觉得比较基础，面向新手，之后有想写读书笔记的想法，所以细看了一遍，感觉很多细节还是很讲的很清楚的。所以打算摘录一些个人觉得有一些值得学习的地方，当然如果是新手的话，全书都还是值得看的。


# 命令行的快捷键
书中提到了命令例的快捷键，原来有这么多，用了快两年的命令行了只知道`CTRL+A`和`CTRL+E`，惭愧。

CTRL-B 左移光标

CTRL-F 右移光标

CTRL-P 查看上一条命令(或上移光标)

CTRL-N 查看下一条命令(或下移光标)

CTRL-A 移动光标至行首

CTRL-E 移动光标至行尾

CTRL-W 删除前一个词

CTRL-U 删除从光标至行首的内容

CTRL-K 删除从光标至行尾的内容

CTRL-Y 粘贴已删除的文本(例如粘贴CTRL-U所删除的内容)


# man命令的`-k`选项
之前，只会用`man`查询某个命令的用法，而`-k`可以根据关键词搜索相关的命令，例如搜索`reverse`：
```
# man -k reverse
bswap (3)            - reverse order of bytes
bswap_16 (3)         - reverse order of bytes
bswap_32 (3)         - reverse order of bytes
bswap_64 (3)         - reverse order of bytes
col (1)              - filter reverse line feeds from input
git-rev-list (1)     - Lists commit objects in reverse chronological order
rev (1)              - reverse lines characterwise
revoutput (3am)      - Reverse output strings sample extension
revtwoway (3am)      - Reverse strings sample two-way processor extension
tac (1)              - concatenate and print files in reverse
TAILQ_FOREACH_REVERSE (3) - implementations of singly-linked lists, singly-linked tail queues, lists and tail queues
xxd (1)              - make a hexdump or do the reverse.
```

>其中，`tac`（行反转）和`rev`（字符反转）都是挺有用的命令。

# 标准错误输出
于标准输出`/dev/stdout`相对应的是标准错误输出`/dev/stderr`。当我们使用重定向符号的时候，错误仍然会输出到终端，要想将错误信息也重定向，则可以这样做：
```bash
 ls /fffffffff >content.txt 2> error.txt
```
如果`/fffffffff`文件存在，则会列出文件信息到`content.txt`，如果产生错误，比如文件不存在，则会将错误信息重定向到`error.txt`。而下面则会将正确和错误都重定向到一个文件
```bash
 ls /fffffffff > result.txt 2>&1
```

## 编程时过滤错误信息
这个在书中没有提到，之前在编程时遇到了，所以顺带想起来提一下。在c++中，大部分人都会使用`std::cout`输出信息，当然这样输出的信息都算在标准输出内。要想写的程序在重定向的时候一些自定义的错误信息仍然在终端中显示，这需要用`std::cerr`，当然还有也介于错误输出和标准输出之间的`std::clog`，下面的例子我截取之前写过的代码，其中省略了变量的定义、赋值和大部分逻辑（当然编译会不通过），只保留和本内容的部分：
```cpp
#include <iostream>
using namespace std;

//some code...

void BSL_Build_Trie(double e, double theta, int l) {
    //some code ...
    clog << "theta:" <<global_theta<<endl;
    clog << "readline:" <<global_readline<<endl;
    cout << "readline," <<global_readline<<endl;
    cout << "theta," <<global_theta<<endl;
}

int main(int argc, char const *argv[]) {
    try{
        //some code..
    }
    catch(const FileIOException &fioex){
        //...
        std::cerr << "Please choose a conf file" << std::endl;
        return(EXIT_FAILURE);
    }
    catch(const ParseException &pex){
        //...
        std::cerr <<"Check the file format."<< std::endl;
        return(EXIT_FAILURE);
    }
    return 0;
}

```