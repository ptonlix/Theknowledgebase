# Go语言基础知识常见问题

1.字符串转化为切片[]byte(s)要慎用，尤其是当数据量较大时（每转换一次都需复制内容）

例如：

```go
a := "hello, world!"
b := []byte(a)
```

2.字符串尾部不包含NULL字符，这一点和C/C++不一样

3.Go不支持指针的运算

4.函数允许返回局部变量的地址，Go编译器使用“栈逃逸”机制将这种局部变量的空间分配到堆上。

```Go
func sum (a ,b int) *int {
	sum := a + b 
	return &sum
}
```

5.可以使用range遍历一个map类型变量，但是不保证每次迭代元素的顺序

6.Go内置的map不是并发安全的，并发安全的map可以使用标准包sync中的map

7.不要直接修改map value内某个元素的值，如果想修改map的某个键值，则必须整体赋值。例如：

```go
type User struct {
	name string
	age int
}
ma := make(map[int]User)
andes := User{
	name: "andes",
	age: 18,
}

ma[1] = andes
//ma[1].age = 19 //ERROR, 不能通过map引用直接修改
andes.age = 19

ma[1] = andes //必须整体替换value
fmt.Printf("%v\n", ma)
```

8. struct类型变量初始化，推荐指定field名字的初始化方式，没有指定的字段则默认初始化为类型的零值。

   按照类型声明顺序，逐个赋值（不推荐），一旦struct增加字段，则整个初始化语句会报错

9. if语句

   if后面可以带一个简单的初始化语句，并以分号分割，该简单语句声明的变量的作用域是整个if语句块，包括后面的else if和else分支。

   Go语言没有条件运算符（a>b?a:b），这也符合Go的设计哲学，只提供一种方法做事情。

   **最佳实践：**

   尽量减少条件语句的复杂度，如果语句太多、太复杂，则建议放到函数里面封装起来

   尽量减少if语句的嵌套层次，通过重构代码让代码变得扁平，便于阅读

```go
if err, file := os.Open("xxxx"); err == nil {
    defer file.Close()
    //do something
} else {
    return nil, err
}
//改写后的代码
err, file  := os.Open("xxxx")
if err != nil {
    return nil, err
}
defer file.Close()
//do something
```

10. switch 条件表达式的值不像C语言那样必须限制为整数，可以是任意支持相等比较运算的类型变量
11. 通过fallthough语句来强制执行下一个case子句（不在判断下一个case子句的条件是否满足）
12. Go语言仅支持一种循环语句，即for语句，同样遵循Go的设计哲学，只提供一种方法做事情，把事情做好。