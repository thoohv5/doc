# 备忘录模式（Memento）

## 概念

在不破坏封闭的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。

![%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F%EF%BC%88Memento%EF%BC%89%2046c5d7fa0fae4487b0d7daf8bb105c7d/Untitled.png](%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F%EF%BC%88Memento%EF%BC%89%2046c5d7fa0fae4487b0d7daf8bb105c7d/Untitled.png)

## 代码

```go
package memento

import "fmt"

type Memento interface{}

type Game struct {
	hp, mp int
}

type gameMemento struct {
	hp, mp int
}

func (g *Game) Play(mpDelta, hpDelta int) {
	g.mp += mpDelta
	g.hp += hpDelta
}

func (g *Game) Save() Memento {
	return &gameMemento{
		hp: g.hp,
		mp: g.mp,
	}
}

func (g *Game) Load(m Memento) {
	gm := m.(*gameMemento)
	g.mp = gm.mp
	g.hp = gm.hp
}

func (g *Game) Status() {
	fmt.Printf("Current HP:%d, MP:%d\n", g.hp, g.mp)
}
```