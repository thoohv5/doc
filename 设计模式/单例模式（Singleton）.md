# 单例模式（Singleton）

## 概念

保证一个类仅有一个实例，并提供一个访问它的全局访问点

![%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F%EF%BC%88Singleton%EF%BC%89%20ee457c349916401882d1d991d7d5115a/Untitled.png](%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F%EF%BC%88Singleton%EF%BC%89%20ee457c349916401882d1d991d7d5115a/Untitled.png)

## 代码

### 懒汉模式，非线程安全

```go
package singleton

type singleton struct {
}

var instance *singleton

func GetInstance() *singleton {
	if instance == nil {
		instance = &singleton{}
	}
	return instance
}
```

### 线程锁

```go
package singleton

import (
	"sync"
)

type singleton struct {
}

var (
	mu       sync.Mutex
	instance *singleton
)

func GetInstance() *singleton {
	mu.Lock()
	defer mu.Unlock()
	if instance == nil {
		instance = &singleton{}
	}
	return instance
}
```

### 检查锁

```go
package singleton

import (
	"sync"
)

type singleton struct {
}

var (
	mu       sync.Mutex
	instance *singleton
)

func GetInstance() *singleton {
	if instance == nil {
		mu.Lock()
		defer mu.Unlock()
		if instance == nil {
			instance = &singleton{}
		}
	}
	return instance
}
```

Once

```go
package singleton

import (
	"sync"
)

type singleton struct {
}

var (
	once     sync.Once
	instance *singleton
)

func GetInstance() *singleton {
	once.Do(func() {
		instance = &singleton{}
	})
	return instance
}
```