# Golang learn note ____ 2

### ***pyke elysia***

## Sync.WaitGroup

`sync.WaitGroup` 也是一个经常会用到的同步原语，它的使用场景是在一个`goroutine`等待一组`goroutine`执行完成。

`sync.WaitGroup` 拥有一个内部计数器。当计数器等于0时，则 `Wait()` 方法会立即返回。否则它将阻塞执行 `Wait()` 方法的 `goroutine` 直到计数器等于0时为止。

要增加计数器，我们必须使用`Add(int)` 方法。要减少它，我们可以使用`Done()`（将计数器减1），也可以传递负数给`Add`方法把计数器减少指定大小，`Done()`方法底层就是通过`Add(-1)`实现的。

我们使用 `Sync.WaitGroup` 时，希望程序在执行下列后续语句时，可以完成之前 `main goroutine` 之外的所有 `goroutine` 。因此， `Sync.WaitGroup` 更像是一种计数器，在开启 `goroutine` 时自增，在 `goroutine` 结束时自减。

```go
wg := Sync.WaitGroup{}  // count := 0

wg.Add(1)               // count ++

wg.Done()               // count --

wg.Wait()               // while(true) {
                        //     if count <= 0 {
                        //         break;
                        //     }
                        // }
```

#

***创建于2025/7/14***