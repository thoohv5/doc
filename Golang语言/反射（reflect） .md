# 反射（reflect）

# 简介

Go程序在运行期使用reflect包访问程序的反射信息。

Golang关于类型设计的一些原则

- 变量包括（type, value）两部分

    nil != nil

- type 包括 static type和concrete type. 简单来说 static type是你在编码是看见的类型(如int、string)，concrete type是runtime系统看见的类型
- 类型断言能否成功，取决于变量的concrete type，而不是static type. 因此，一个 reader变量如果它的concrete type也实现了write方法的话，它也可以被类型断言为writer.

# 反射类型

通过反射获取类型信息：(reflect.TypeOf()和reflect.Type)

## 反射种类（Kind）

```go
/*
 * These data structures are known to the compiler (../../cmd/internal/gc/reflect.go).
 * A few are known to ../runtime/type.go to convey to debuggers.
 * They are also known to ../runtime/type.go.
 */

// A Kind represents the specific kind of type that a Type represents.
// The zero Kind is not a valid kind.
type Kind uint

const (
	Invalid Kind = iota
	Bool
	Int
	Int8
	Int16
	Int32
	Int64
	Uint
	Uint8
	Uint16
	Uint32
	Uint64
	Uintptr
	Float32
	Float64
	Complex64
	Complex128
	Array
	Chan
	Func
	Interface
	Map
	Ptr
	Slice
	String
	Struct
	UnsafePointer
)
```

## 区别

反射类型（Type） 

静态类型（static type），编码的类型(如int、string)

反射种类（Kind ）

基本类型（concrete type），runtime系统的类型

## 结构体反射

### 成员类型

```go
# 反射类型对象的名称
reflect.TypeOf(stru).Name()
# 反射类型对象的种类
reflect.TypeOf(stru).Kind()
# 反射类型对象指针指向的元素类型
reflect.TypeOf(stru).Elem()
# 根据索引，返回索引对应的结构体字段的信息。当值不是结构体或索引超界时发生宕机
reflect.TypeOf(stru).Field(i int) StructField
# 返回结构体成员字段数量。当类型不是结构体或索引超界时发生宕机
reflect.TypeOf(stru).NumField() int
# 根据给定字符串返回字符串对应的结构体字段的信息。没有找到时 bool 返回 false，当类型不是结构体或索引超界时发生宕机
reflect.TypeOf(stru).FieldByName(name string) (StructField, bool)
# 多层成员访问时，根据 []int 提供的每个结构体的字段索引，返回字段的信息。没有找到时返回零值。当类型不是结构体或索引超界时 发生宕机
reflect.TypeOf(stru).FieldByIndex(index []int) StructField
# 根据匹配函数匹配需要的字段。当值不是结构体或索引超界时发生宕机
reflect.TypeOf(stru).FieldByNameFunc( match func(string) bool) (StructField,bool)
```

### StructField结构

```c
type StructField struct {
    Name string          // 字段名
    PkgPath string       // 字段路径
    Type      Type       // 字段反射类型对象
    Tag       StructTag  // 字段的结构体标签
    Offset    uintptr    // 字段在结构体中的相对偏移
    Index     []int      // Type.FieldByIndex中的返回的索引值
    Anonymous bool       // 是否为匿名字段
}
```

### 创建实例

```go
reflect.New(reflect.TypeOf(stru))
```

# 反射值

通过反射获取值信息：(reflect.ValueOf()和reflect.Value)

## 结构体反射

## 成员值

```go
# 根据索引，返回索引对应的结构体成员字段的反射值对象。当值不是结构体或索引超界时发生宕机
reflect.ValueOf(stru).Field(i int) Value
# 返回结构体成员字段数量。当值不是结构体或索引超界时发生宕机
reflect.ValueOf(stru).NumField() int
# 根据给定字符串返回字符串对应的结构体字段。没有找到时返回零值，当值不是结构体或索引超界时发生宕机
reflect.ValueOf(stru).FieldByName(name string) Value
# 多层成员访问时，根据 []int 提供的每个结构体的字段索引，返回字段的值。 没有找到时返回零值，当值不是结构体或索引超界时发生宕机
reflect.ValueOf(stru).FieldByIndex(index []int) Value
# 根据匹配函数匹配需要的字段。找到时返回零值，当值不是结构体或索引超界时发生宕机
reflect.ValueOf(stru).FieldByNameFunc(match func(string) bool) Value
```

## 值的空和有效性

IsNil()和IsValid() -- 判断反射值的空和有效性

```go
# 返回值是否为 nil。如果值类型不是通道（channel）、函数、接口、map、指针或 切片时发生 panic，类似于语言层的v== nil操作
reflect.ValueOf(stru).IsNil() bool
# 判断值是否有效。 当值本身非法时，返回 false，例如 reflect Value不包含任何值，值为 nil 等
reflect.ValueOf(stru).IsValid() bool
```

## 修改变量的值

### 判断及获取元素

```go
# 取值指向的元素值，类似于语言层*操作。当值类型不是指针或接口时发生宕 机，空指针时返回 nil 的 Value
reflect.ValueOf(stru).Elem() Value	
# 对可寻址的值返回其地址，类似于语言层&操作。当值不可寻址时发生宕机
reflect.ValueOf(stru).Addr() Value	
# 表示值是否可寻址
reflect.ValueOf(stru).CanAddr() bool
# 返回值能否被修改。要求值可寻址且是导出的字段
reflect.ValueOf(stru).CanSet() bool
```

### 值修改

```go
# 将值设置为传入的反射值对象的值
Set(x Value)
Setlnt(x int64)
SetUint(x uint64)
SetFloat(x float64)
SetBool(x bool)
SetBytes(x []byte)
SetString(x string)
```

### 值可修改条件

1. 可被寻址
2. 被导出

## 调用函数

```go
package main

import (
	"fmt"
	"reflect"
)

//普通函数
func add(a, b int) int {
	return a + b
}

func main() {

	//将函数包装为反射值对象
	funcValue := reflect.ValueOf(add)

	//构造函数参数，传入两个整形值
	paramList := []reflect.Value{reflect.ValueOf(2), reflect.ValueOf(3)}

	//反射调用函数
	retList := funcValue.Call(paramList)

	fmt.Println(retList[0].Int())
}
```

## 调用方法

```go
package main

import (
	"fmt"
	"reflect"
)

type MyMath struct {
	Pi float64
}

//普通函数
func (myMath MyMath) Sum(a, b int) int {
	return a + b
}

func (myMath MyMath) Dec(a, b int) int {
	return a - b
}

func main() {

	var myMath = MyMath{Pi:3.14159}

	//获取myMath的值对象
	rValue := reflect.ValueOf(myMath)

	//获取到该结构体有多少个方法
	//numOfMethod := rValue.NumMethod()

	//构造函数参数，传入两个整形值
	paramList := []reflect.Value{reflect.ValueOf(30), reflect.ValueOf(20)}

	//调用结构体的第一个方法Method(0)
	//注意:在反射值对象中方法索引的顺序并不是结构体方法定义的先后顺序
	//而是根据方法的ASCII码值来从小到大排序，所以Dec排在第一个，也就是Method(0)
	result := rValue.Method(0).Call(paramList)

	fmt.Println(result[0].Int())
	
}
```

# 附录

[codegangsta/inject](https://github.com/codegangsta/inject)

[Go语言反射reflect](https://www.cnblogs.com/itbsl/p/10551880.html)