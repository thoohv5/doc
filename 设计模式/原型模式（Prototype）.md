# 原型模式（Prototype）

## 概念

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象；

![%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F%EF%BC%88Prototype%EF%BC%89%205afa26aab279489c881377c54e52414e/Untitled.png](%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F%EF%BC%88Prototype%EF%BC%89%205afa26aab279489c881377c54e52414e/Untitled.png)

## 代码

```go
package prototype

// Cloneable 是原型对象需要实现的接口
type Cloneable interface {
	Clone() Cloneable
}

type PrototypeManager struct {
	prototypes map[string]Cloneable
}

func NewPrototypeManager() *PrototypeManager {
	return &PrototypeManager{
		prototypes: make(map[string]Cloneable),
	}
}

func (p *PrototypeManager) Get(name string) Cloneable {
	return p.prototypes[name]
}

func (p *PrototypeManager) Set(name string, prototype Cloneable) {
	p.prototypes[name] = prototype
}
```