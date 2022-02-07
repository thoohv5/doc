# 建造者模式（Builder）

## 概念

将一个复杂对象的**构建**与它的**表示**分离，使得同样的构建过程可以创建不同的表示。

![%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F%EF%BC%88Builder%EF%BC%89%2027bda66a135c4d29a7e7779ab87c87b1/Untitled.png](%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F%EF%BC%88Builder%EF%BC%89%2027bda66a135c4d29a7e7779ab87c87b1/Untitled.png)

## 代码

```go
package builder

// Builder 是生成器接口
type Builder interface {
	Part1()
	Part2()
	Part3()
}

type Director struct {
	builder Builder
}

// NewDirector ...
func NewDirector(builder Builder) *Director {
	return &Director{
		builder: builder,
	}
}

// Construct Product
func (d *Director) Construct() {
	d.builder.Part1()
	d.builder.Part2()
	d.builder.Part3()
}

type Builder1 struct {
	result string
}

func (b *Builder1) Part1() {
	b.result += "1"
}

func (b *Builder1) Part2() {
	b.result += "2"
}

func (b *Builder1) Part3() {
	b.result += "3"
}

func (b *Builder1) GetResult() string {
	return b.result
}

type Builder2 struct {
	result int
}

func (b *Builder2) Part1() {
	b.result += 1
}

func (b *Builder2) Part2() {
	b.result += 2
}

func (b *Builder2) Part3() {
	b.result += 3
}

func (b *Builder2) GetResult() int {
	return b.result
}
```