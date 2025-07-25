# Golang learn note ____ 2

### ***pyke elysia***

### grom 处理字符串数组

例：

```go
type TeamBelong []string

func (t *TeamBelong) Scan(value interface{}) error {
	bytesValue, _ := value.([]byte)
	return json.Unmarshal(bytesValue, t)
}

func (t TeamBelong) Value() (driver.Value, error) {
	return json.Marshal(t)
}
```

#

***创建于2025/7/26***