---
title: "在 C++ 中使用第三方库"
date: 2021-05-26T10:53:12+08:00
draft: false
---

1. 创建项目

```shell
mkdir demo
cd demo
```

2. 创建 cmake 工程

`vi CMakeLists.txt`

```txt
cmake_minimum_required(VERSION 2.8)

project(demo)
```

3. 引入第三方库

```shell
mkdir 3rd
git clone https://github.com/fmtlib/fmt.git 3rd/fmt
```

4. 链接第三方库

`vi CMakeLists.txt`

```txt
cmake_minimum_required(VERSION 2.8)

project(demo)

add_executable(demo main.cpp)

add_subdirectory(3rd/fmt)

target_link_libraries(demo fmt)
```

5. 编写业务逻辑

`vi main.cpp`

```cpp
#include <fmt/core.h>

int main()
{
    fmt::print("Hello, world!\n");
}
```

6. 项目编译

```shell
cmake .
make
```

7. 项目运行

```shell
./demo
```

可以看到控制台输出：

`Hello, world!`
