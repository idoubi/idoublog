## 循环

Go 语言中只有一种循环语法，通过 `for` 关键词来控制，有这么几种循环场景。

- 数值递进

临时变量递增

```go
for i := 0; i < 10; i++ {
    println(i)
}
```

- 数据遍历

数组遍历，输出数组元素下标和值

```go
data := []int{2, 6, 9, -1}
for k, v := range data {
    println(k, v)
}
```

键值对遍历，输出键和值

```go
data := map[string]int{
    "php":    211,
    "go":     985,
    "python": 666,
}
for k, v := range data {
    println(k, v)
}
```
