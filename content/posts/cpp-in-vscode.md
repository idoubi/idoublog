---
title: "在「VS Code」搭建「C++」开发环境"
date: 2020-07-25T12:29:30+08:00
---

大家都知道「VS Code」是一个“真香”编辑器，用来做「C++」开发也是一样的香！

## 开始写 C++ 程序

第一次接触 C++，我们先不折腾编辑器的事情，随便用个能写代码的软件，写一个能输出“Hello World”的可执行程序再说。

在 `Linux` 机器上用 `vi hello.cpp`，先写几行代码：

```cpp
#include <iostream>
using namespace std;
 
int main()
{
   cout << "Hello World"; 
   return 0;
}
```

安装 C++ 环境依赖

```shell
yum install gcc gcc-c++
```

确保 `gcc`、`g++` 用的同一个版本

```shell
/usr/bin/gcc -v
/usr/bin/g++ -v
```

编译 C++ 程序

```shell
g++ hello.cpp -o hello
```

运行编译后的程序

```shell
./hello
```

如果看到如下输出，证明我们的 C++ 编码环境是 OK 的

```shell
$ ./hello 
Hello World#  
```

## VS Code 编译/运行 C++ 代码

下面我们开始对 VS Code 进行一系列配置，用来做日常的 C++ 开发工作。

- 安装 C++ 扩展

按下快捷键 `command+shift+X` 打开扩展安装面板，搜索并安装 `C/C++` 扩展。

- 工程配置

创建一个新的工作区，在工作区的 `.vscode` 目录下新建任务配置文件 `task.json`，

创建 `build`、`run` 两个任务：

```json
{
    "version": "2.0.0",
    "tasks": [{
        "label": "build",
        "type": "shell",
        "command": "g++",
        "args": [
            "${file}",
            "-o",
            "${workspaceFolder}/bin/${fileBasenameNoExtension}",
            "-g",
            "-Wall",
            "-std=c++11"
        ],
        "group": {
            "kind": "build",
            "isDefault": true
        },
        "presentation": {
            "echo": true,
            "reveal": "always",
            "focus": false,
            "panel": "shared",
            "showReuseMessage": true,
            "clear": false
        },
        "problemMatcher": "$gcc"
    }, {
        "label": "run",
        "type": "shell",
        "dependsOn": "build",
        "command": "${workspaceFolder}/bin/${fileBasenameNoExtension}",
        "group": {
            "kind": "test",
            "isDefault": true
        },
        "presentation": {
            "echo": true,
            "reveal": "always",
            "focus": true,
            "panel": "shared",
            "showReuseMessage": true,
            "clear": false
        }
    }]
}
```

按下快捷键 `command+shift+B` 可以执行 `build` 任务，对 C++ 源文件进行编译，在 `bin` 目录下生成同名的可执行文件。

我们需要为 `run` 任务绑定一个快捷键：

```json
[
  {
    "key": "cmd+t",
    "command": "workbench.action.tasks.test"
  }
]
```

按下快捷键 `command+T` 可以执行 `run` 任务，运行 `bin` 目录下的可执行文件，输出 `Hello World` 消息。

## VS Code 断点调试 C++ 代码

在工作区的 `.vscode` 目录下新建 `launch.json`，填好配置：

```json
{
    "version": "0.2.0",
    "configurations": [{
        "name": "Debug",
        "type": "cppdbg",
        "request": "launch",
        "program": "${workspaceFolder}/bin/${fileBasenameNoExtension}",
        "args": [],
        "stopAtEntry": false,
        "cwd": "${workspaceFolder}",
        "environment": [],
        "externalConsole": false,
        "MIMode": "gdb",
        "setupCommands": [{
            "description": "Enable pretty-printing for gdb",
            "text": "-enable-pretty-printing",
            "ignoreFailures": true
        }],
        "preLaunchTask": "build"
    }]
}
```

打开调试面板，就可以对 C++ 代码进行断点调试了。

![debug](http://blogcdn.idoustudio.com/pic/20200725130519.png)


> 工欲善其事必先利其器。C++ 学习任重而道远，搭建好了 VS Code 开发环境，对于我们以后的学习和开发，肯定会带来非常大的便利。