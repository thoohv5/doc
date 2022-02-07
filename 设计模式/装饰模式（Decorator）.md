# 装饰模式（Decorator）

## 概念

动态地给一个对象添加一些额外地职责，就增加功能来说，装饰模式比生成子类更加灵活。

![%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F%EF%BC%88Decorator%EF%BC%89%2006f9a602c9bd4fdf9e98e4fc48697507/Untitled.png](%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F%EF%BC%88Decorator%EF%BC%89%2006f9a602c9bd4fdf9e98e4fc48697507/Untitled.png)

![%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F%EF%BC%88Decorator%EF%BC%89%2006f9a602c9bd4fdf9e98e4fc48697507/Untitled%201.png](%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F%EF%BC%88Decorator%EF%BC%89%2006f9a602c9bd4fdf9e98e4fc48697507/Untitled%201.png)

## 代码

```go
package composite

import "fmt"

type Component interface {
	Parent() Component
	SetParent(Component)
	Name() string
	SetName(string)
	AddChild(Component)
	Print(string)
}

const (
	LeafNode = iota
	CompositeNode
)

func NewComponent(kind int, name string) Component {
	var c Component
	switch kind {
	case LeafNode:
		c = NewLeaf()
	case CompositeNode:
		c = NewComposite()
	}

	c.SetName(name)
	return c
}

type component struct {
	parent Component
	name   string
}

func (c *component) Parent() Component {
	return c.parent
}

func (c *component) SetParent(parent Component) {
	c.parent = parent
}

func (c *component) Name() string {
	return c.name
}

func (c *component) SetName(name string) {
	c.name = name
}

func (c *component) AddChild(Component) {}

func (c *component) Print(string) {}

type Leaf struct {
	component
}

func NewLeaf() *Leaf {
	return &Leaf{}
}

func (c *Leaf) Print(pre string) {
	fmt.Printf("%s-%s\n", pre, c.Name())
}

type Composite struct {
	component
	childs []Component
}

func NewComposite() *Composite {
	return &Composite{
		childs: make([]Component, 0),
	}
}

func (c *Composite) AddChild(child Component) {
	child.SetParent(c)
	c.childs = append(c.childs, child)
}

func (c *Composite) Print(pre string) {
	fmt.Printf("%s+%s\n", pre, c.Name())
	pre += " "
	for _, comp := range c.childs {
		comp.Print(pre)
	}
}
```