结构体的默认值

```go
type Data struct {
    Url     string
    Content string
}
d := Data{
    Url: "https",
}
fmt.Println(d.Content == "") // true
```

创建数组与数组索引

```go
nums := make([]int, 0)
nums = append(nums, 1)
nums = append(nums, 2)
nums = append(nums, 3)
fmt.Println(nums)                 // [1 2 3]
fmt.Println(len(nums), cap(nums)) // 3 4
fmt.Println(nums[1])              // 2
```

panic与recover

```go
type ErrorCode int
type MyError struct {
    Code    ErrorCode `json:"code"`
    Message string    `json:"message"`
}

defer func() {
    if err := recover(); err != nil {
        myErr, ok := err.(MyError)
        if ok {
            fmt.Printf("Recovered from MyError - Code: %d, Message: %s\n", myErr.Code, myErr.Message)
            return
        }
    }
}()

panic(MyError{Code: 500, Message: "Internal Server Error"})
```

> recover只能捕捉当前协程的panic，不捕捉会引起主进程中断

斐波那契（迭代器）

```go
func Fibonacci(n int) func(yield func(int) bool) {
	a, b, c := 0, 1, 1
	return func(yield func(int) bool) {
		for range n {
			if !yield(a) { // 这里是把a传递出去的地方
				return
			}
			a, b = b, c
			c = a + b
		}
	}
}

func main() {
	n := 8
	for f := range Fibonacci(n) {
		fmt.Println(f)
	}
}
```

使用channel实现生成器

```go
func Generate() <-chan int {
	ch := make(chan int)

	go func() {
		defer close(ch)
		for i := range 10 {
			time.Sleep(1 * time.Second)
			ch <- i
		}
	}()

	return ch
}

func main() {
    for v := range Generate() {
		fmt.Println("收到:", v)
	}
}
```
