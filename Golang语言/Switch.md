# Switch

# ExprSwitch

## default case

当没有其他 case 匹配时，将执行 default 语句。

## 包含多个表达式的 case

## 没有表达式的 switch

switch 中的表达式是可选的，可以省略。如果省略表达式，则相当于 switch true，这种情况下会将每一个 case 的表达式的求值结果与 true 做比较，如果相等，则执行相应的代码。

## fallthrough

在 Go 中执行完一个 case 之后会立即退出 switch 语句。fallthrough语句用于标明执行完当前 case 语句之后按顺序执行下一个case 语句。

fallthrough 必须是 case 语句块中的最后一条语句。如果它出现在语句块的中间，编译器将会报错：fallthrough statement out of place

# TypeSwitch

```go
var i interface{}
 
 
// 这里的 v 是类型的值
switch v := i.(type) {
case T:
    // 值 v 的类型为 T
case S:
    // 值 v 的类型为 S
default:
    // 没有匹配，v 的类型与 i 的底层值类型相同，只是没有确定具体是哪个类型
}
```