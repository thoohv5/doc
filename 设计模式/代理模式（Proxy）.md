# 代理模式（Proxy）

## 概念

在访问对象时引入一定程度的间接性，因为这种间接性，可以附加多种用途。

![%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F%EF%BC%88Proxy%EF%BC%89%20f5aca8735fd74d949338af2bf784fb42/Untitled.png](%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F%EF%BC%88Proxy%EF%BC%89%20f5aca8735fd74d949338af2bf784fb42/Untitled.png)

## 代码

```go
package proxy

type Subject interface {
	Do() string
}

type RealSubject struct{}

func (RealSubject) Do() string {
	return "real"
}

type Proxy struct {
	real RealSubject
}

func (p Proxy) Do() string {
	var res string

	// 在调用真实对象之前的工作，检查缓存，判断权限，实例化真实对象等。。
	res += "pre:"

	// 调用真实对象
	res += p.real.Do()

	// 调用之后的操作，如缓存结果，对结果进行处理等。。
	res += ":after"

	return res
}
```