# 桥接模式（Bridge）

## 概念

将抽象部分与它的实现部分分离，使他们都可以独立地变化

![%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Bridge%EF%BC%89%209b1f33e9411f4e9392f0b588b24ed381/Untitled.png](%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Bridge%EF%BC%89%209b1f33e9411f4e9392f0b588b24ed381/Untitled.png)

## 代码

```go
package bridge

import "fmt"

type AbstractMessage interface {
	SendMessage(text, to string)
}

type MessageImplementer interface {
	Send(text, to string)
}

type MessageSMS struct{}

func ViaSMS() MessageImplementer {
	return &MessageSMS{}
}

func (*MessageSMS) Send(text, to string) {
	fmt.Printf("send %s to %s via SMS", text, to)
}

type MessageEmail struct{}

func ViaEmail() MessageImplementer {
	return &MessageEmail{}
}

func (*MessageEmail) Send(text, to string) {
	fmt.Printf("send %s to %s via Email", text, to)
}

type CommonMessage struct {
	method MessageImplementer
}

func NewCommonMessage(method MessageImplementer) *CommonMessage {
	return &CommonMessage{
		method: method,
	}
}

func (m *CommonMessage) SendMessage(text, to string) {
	m.method.Send(text, to)
}

type UrgencyMessage struct {
	method MessageImplementer
}

func NewUrgencyMessage(method MessageImplementer) *UrgencyMessage {
	return &UrgencyMessage{
		method: method,
	}
}

func (m *UrgencyMessage) SendMessage(text, to string) {
	m.method.Send(fmt.Sprintf("[Urgency] %s", text), to)
}
```