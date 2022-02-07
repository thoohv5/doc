# 观察者模式（Observer）

## 概念

定义一种一对多的关系，让多个观察者对象同时监听某一主题对象；

![%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F%EF%BC%88Observer%EF%BC%89%201b1fd9ceb0e74077b2c78c0cb173a2a7/Untitled.png](%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F%EF%BC%88Observer%EF%BC%89%201b1fd9ceb0e74077b2c78c0cb173a2a7/Untitled.png)

## 代码

```go
package observer

import "fmt"

type Subject struct {
	observers []Observer
	context   string
}

func NewSubject() *Subject {
	return &Subject{
		observers: make([]Observer, 0),
	}
}

func (s *Subject) Attach(o Observer) {
	s.observers = append(s.observers, o)
}

func (s *Subject) notify() {
	for _, o := range s.observers {
		o.Update(s)
	}
}

func (s *Subject) UpdateContext(context string) {
	s.context = context
	s.notify()
}

type Observer interface {
	Update(*Subject)
}

type Reader struct {
	name string
}

func NewReader(name string) *Reader {
	return &Reader{
		name: name,
	}
}

func (r *Reader) Update(s *Subject) {
	fmt.Printf("%s receive %s\n", r.name, s.context)
}
```