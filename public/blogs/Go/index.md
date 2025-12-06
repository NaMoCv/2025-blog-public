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