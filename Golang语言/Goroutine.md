# Goroutine

# 顺序进程通信

顺序进程通信（communicating sequential processess, CSP）

[CSP并发模型](https://www.notion.so/CSP-39b017af4363482681ee4e9f218fa15e)

# Goroutines

[MPG模型](https://www.notion.so/MPG-1114cb78d65d4dd0af948a522f09d62c)

除了从主函数退出或者直接退出程序之外，没有其他的编程方法能够让一个goroutines来打断另一个执行，但是可以通过goroutine之间的通信 来让一个goroutine请求请求其他的goroutine，并让其自己结束执行。

```go
runtime.Goexit()
```

# Channels

[通道(channel)](https://www.notion.so/channel-c34e1cf92bce4df48b3fe125e0b3e080)

**不要通过共享内存来通信，而是通过通信来实现内存共享（Do not communicate by sharing memory; instead, share memory by communicating）**

对关闭的channel的发送操作都将导致panic异常

对关闭的channel的接收操作都可以接收到之前已经成功发送的数据；如果channel中已经没有数据的话产生一个零值的数据

对于一个nil的channel发送操作或者接受操作都会发生阻塞

## 不带缓存的Channels

一个基于无缓存Channels的发送操作将导致发送者goroutine阻塞，直到另一个goroutine在相同的Channels上执行接收操作，当发送的值通过Channels成功传输之后，两个goroutine可以继续执行后面的语句；反之，如果接收操作先发生，那么接受者goroutine也将阻塞，直到有另一个goroutine在相同的Channels上执行发送操作。

## 带缓存的Channels

带缓存的Channel内部持有一个元素队列，队列的最大容量是在调用make函数创建channel时通过第二个参数指定的

## range

for-range是使用频率很高的结构，常用它来遍历数据，range能够感知channel的关闭，当channel被发送数据的协程关闭时，range就会结束，接着退出for循环。

```go
func TestGo(t *testing.T) {

	c := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			c <- i
			fmt.Println(i)
		}
		close(c)
	}()

	time.Sleep(2 * time.Second)

	for {
		if cc, ok := <-c; !ok {
			fmt.Println(cc, ok)
			break
		} else {
			fmt.Println("----", cc)
		}
	}

	// for cc := range c {
	// 	fmt.Println("----", cc)
	// }

}
```

## 基于select的多路复用

select 会等待case中有能够执行的case时去执行。当条件满足时，select才回去通信并执行case之后的语句；这时候其他通信是不会执行的。select{}，会永远地等待下去。

如果多个case同时就绪时，select会随机地选择一个执行，这样来保证 每一个channel都有平等的被select的机会。

```go
func TestGo(t *testing.T) {

	c := make(chan int)
	done := make(chan struct{})
	go func() {
		for i := 0; i < 10; i++ {
			c <- i
			if i == 11 {
				done <- struct{}{}
				close(done)
			}

		}
		close(c)
	}()

	for {
		select {
		case cc, ok := <-c:
			if !ok {
				fmt.Println("c finish")
				return
			}
			fmt.Println(cc)
		case <-done:
			fmt.Println("done")
			return
		case <-time.After(time.Second):
			fmt.Println("timeout")
			return
		}
	}

}
```

# 注意

## 并发的退出

不要向channel发送值，而是用关闭一个channel来进行广播

```go
//!+1
var done = make(chan struct{})

func cancelled() bool {
	select {
	case <-done:
		return true
	default:
		return false
	}
}

//!-1
```