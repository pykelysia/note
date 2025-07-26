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

#

***创建于2025/7/26***