# 简单工厂模式（Simple Factory）

## 概念

由一个工厂对象决定创建出哪一种产品类的实例

![%E7%AE%80%E5%8D%95%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F%EF%BC%88Simple%20Factory%EF%BC%89%206633f77cc0644d0eb36ba06d0a73c9e5/Untitled.png](%E7%AE%80%E5%8D%95%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F%EF%BC%88Simple%20Factory%EF%BC%89%206633f77cc0644d0eb36ba06d0a73c9e5/Untitled.png)

## 代码

```go
package simplefactory

import "fmt"

// IProduct is interface
type IProduct interface {
	Create(name string) string
}

// NewProduct return Api instance by type
func NewProduct(t int) IProduct {
	if t == 1 {
		return &aProduct{}
	} else if t == 2 {
		return &bProduct{}
	}
	return nil
}

// aProduct is one of IProduct implement
type aProduct struct{}

// Create hi to name
func (*aProduct) Create(name string) string {
	return fmt.Sprintf("a, %s", name)
}

// HelloAPI is another IProduct implement
type bProduct struct{}

// Create hello to name
func (*bProduct) Create(name string) string {
	return fmt.Sprintf("b, %s", name)
}
```

## 比较

```jsx
# 工厂模式是创建型模式，适应对象的变化。
# 策略模式是行为性模式，适应行为的变化

# 工厂模式封装对象，实例化对象后调用的时候要知道具体的方法。
# 策略模式封闭的是行为，调用的时候必须先制定实例化具体的类，再调用抽象的方法。
# 策略模式和工厂模式一起使用的，用工厂来创建算法类。

# 策略模式的作用是让一个对象在许多行为中选择一种行为。
# 工厂模式是对父类进行重写，而策略模式是调用不同类方法。
```