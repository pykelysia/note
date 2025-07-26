# Golang learn note ____ 1

### ***pyke elysia***

### panic
Go 语言中，比较常见的错误处理方法是返回 error，由调用者决定后续如何处理。但是如果是无法恢复的错误，可以手动触发 panic，当然如果在程序运行过程中出现了类似于数组越界的错误，panic 也会被触发。panic 会中止当前执行的程序，退出。

下面是主动触发的例子：

```go
// hello.go
func main() {
	fmt.Println("before panic")
	panic("crash")
	fmt.Println("after panic")
}
```

```
before panic
panic: crash

goroutine 1 [running]:
main.main()
        ~/go_demo/hello/hello.go:7 +0x95
exit status 2
```

下面是数组越界触发的 panic

```go
// hello.go
func main() {
	arr := []int{1, 2, 3}
	fmt.Println(arr[4])
}
```

```
panic: runtime error: index out of range [4] with length 3
```

### defer
panic 会导致程序被中止，但是在退出前，会先处理完当前协程上已经defer 的任务，执行完成后再退出。效果类似于 java 语言的 try...catch。

```go
// hello.go
func main() {
	defer func() {
		fmt.Println("defer func")
	}()

	arr := []int{1, 2, 3}
	fmt.Println(arr[4])
}
```

```
defer func
panic: runtime error: index out of range [4] with length 3
```

可以 defer 多个任务，在同一个函数中 defer 多个任务，会逆序执行。即先执行最后 defer 的任务。

在这里，defer 的任务执行完成之后，panic 还会继续被抛出，导致程序非正常结束。

### recover
Go 语言还提供了 recover 函数，可以避免因为 panic 发生而导致整个程序终止，recover 函数只在 defer 中生效。

```go
// hello.go
func test_recover() {
	defer func() {
		fmt.Println("defer func")
		if err := recover(); err != nil {
			fmt.Println("recover success")
		}
	}()

	arr := []int{1, 2, 3}
	fmt.Println(arr[4])
	fmt.Println("after panic")
}

func main() {
	test_recover()
	fmt.Println("after recover")
}
```

```
$ go run hello.go 
defer func
recover success
after recover
```

我们可以看到，recover 捕获了 panic，程序正常结束。test_recover() 中的 after panic 没有打印，这是正确的，当 panic 被触发时，控制权就被交给了 defer 。就像在 java 中，try代码块中发生了异常，控制权交给了 catch，接下来执行 catch 代码块中的代码。而在 main() 中打印了 after recover，说明程序已经恢复正常，继续往下执行直到结束。

#

***创建于2025/7/26***