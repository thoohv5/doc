# 适配器模式（Adapter）

## 概念

将一个类的接口适配成用户所期待的

![%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F%EF%BC%88Adapter%EF%BC%89%209c9955a37e9042a3b9aaae1d6c0c15da/Untitled.png](%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F%EF%BC%88Adapter%EF%BC%89%209c9955a37e9042a3b9aaae1d6c0c15da/Untitled.png)

## 代码

```go
package adapter

// Target 是适配的目标接口
type Target interface {
	Request() string
}

// Adaptee 是被适配的目标接口
type Adaptee interface {
	SpecificRequest() string
}

// NewAdaptee 是被适配接口的工厂函数
func NewAdaptee() Adaptee {
	return &adapteeImpl{}
}

// AdapteeImpl 是被适配的目标类
type adapteeImpl struct{}

// SpecificRequest 是目标类的一个方法
func (*adapteeImpl) SpecificRequest() string {
	return "adaptee method"
}

// NewAdapter 是Adapter的工厂函数
func NewAdapter(adaptee Adaptee) Target {
	return &adapter{
		Adaptee: adaptee,
	}
}

// Adapter 是转换Adaptee为Target接口的适配器
type adapter struct {
	Adaptee
}

// Request 实现Target接口
func (a *adapter) Request() string {
	return a.SpecificRequest()
}
```