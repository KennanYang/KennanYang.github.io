---
title: go入门，学习用goroutine开启并发
date: 2022-03-02 08:03:04
categories: go
tags: 
- go
- 并发
---

**为了学习go语言，和了解并行的基本写法，这里通过一个demo练习**
由于已经有了c++和python的基础，所以了解基本语法很快，主要是学习go面向消息的传递机制，以及使用goroutine进行并发操作。
参考资料：
[Go 语言教程-Go并发](https://www.runoob.com/go/go-concurrent.html)

### 代码
hello.go

```go
/*一个go的并行计算demo,学习go的基本语法，并计算两个数组元素值为1的数目*/

//包声明：必须在源文件中非注释的第一行指明这个文件属于哪个包
//每个Go程序都包含一个名为main的包
package main 

//import告诉Go编译器使用"fmt"包里的元素
//fmt包实现了格式化IO函数
import "fmt"


/*
函数声明方式
func function_name([parameter list])[return_types]{
    函数体
}
*/
func sum(arr []int, c chan int){
    /*
    变量声明的两种方式：
    1.var v_name v_type = value
    2.v_name := value 根据值自行判定变量类型

    注：标识符首字母大写，如Group 1，则可被外部包调用，称为导出。以小写字母开头则只能内部可见。
    */
    sum := 0

    /*
    循环定义方式 
    1.for init; condition； post{}
    2.for condition{}
    3.for key, value := range iter{}  用range对slice、map、数组等迭代循环
    */

    for _, value := range arr {
        /*
        条件语句
        if 布尔表达式{
            //语句
        }
        */
        if value == 1{
            //运算符同c++，包括++，--
            sum++
        }
    }
    //值保存到通道（channel）方便并行
    c <- sum
}

//main函数是程序开始执行的函数
func main( ){
    //字符串输出到控制台，并结尾增加换行符'\n'
    fmt.Println("hello world!")

    // 创建数组方式一: var variable_name = [SIZE] variable_type{,,,...}
    var arr_a =[]int  {2, 1, 3, 1, 4, 1, 5}
    // 创建数组方式二: variable_name := [SIZE] variable_type{,,,...}
    arr_b := []int {1, 4, 5, 2, 1}
    
    /*

    */
    ch := make(chan int)

    /*
    函数调用方式: func([parameter_list])
    
    goroutine: 轻量级线程，支持并发
    如果用goroutine调度，则前加go关键字，如go f(x,y,z)
    */
    go sum(arr_a, ch)
    go sum(arr_b, ch)

    /*
    通道（channel）可用于两个goroutine之间通过传递一个指定的值来同步运行和通讯。
    操作符<-用于指定通道的方向，发送或接收。如：
    ch <- v //把v发送到通道ch
    v := <- ch //从ch接收数据，并把值赋给v
    */
    result_a, result_b := <- ch, <- ch
    
    //输出计算结果
    fmt.Println("results", result_a, result_b, result_a + result_b)
}

```

### 输出结果
![输出结果](/images/helloworld.png)
