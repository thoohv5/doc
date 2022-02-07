# 策略模式（Strategy）

## 概念

定义了一系列的算法，并将每一个算法封装起来，而且使它们还可以相互替换，让算法独立于使用它的客户而独立变化

![%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Strategy%EF%BC%89%208adffc8f41cc48a3a614746d28bd76d1/Untitled.png](%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Strategy%EF%BC%89%208adffc8f41cc48a3a614746d28bd76d1/Untitled.png)

![%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Strategy%EF%BC%89%208adffc8f41cc48a3a614746d28bd76d1/Untitled%201.png](%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F%EF%BC%88Strategy%EF%BC%89%208adffc8f41cc48a3a614746d28bd76d1/Untitled%201.png)

## 代码

```
package strategy

import "fmt"

type Payment struct {
   context  *PaymentContext
   strategy PaymentStrategy
}

type PaymentContext struct {
   Name, CardID string
   Money        int
}

func NewPayment(name, cardid string, money int, strategy PaymentStrategy) *Payment {
   return &Payment{
      context: &PaymentContext{
         Name:   name,
         CardID: cardid,
         Money:  money,
      },
      strategy: strategy,
   }
}

func (p *Payment) Pay() {
   p.strategy.Pay(p.context)
}

type PaymentStrategy interface {
   Pay(*PaymentContext)
}

type Cash struct{}

func (*Cash) Pay(ctx *PaymentContext) {
   fmt.Printf("Pay $%d to %s by cash", ctx.Money, ctx.Name)
}

type Bank struct{}

func (*Bank) Pay(ctx *PaymentContext) {
   fmt.Printf("Pay $%d to %s by bank account %s", ctx.Money, ctx.Name, ctx.CardID)

}

```

## 附录

[设计完美的策略模式，消除If-else](https://www.cnblogs.com/jiujiduilie/p/9191629.html)