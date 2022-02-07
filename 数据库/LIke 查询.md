# LIke 查询

## 反斜线

```jsx
Mysql like语句转义字符

查询一个\，是必须用\\\\。

原文如下：

因为 MySQL 在字符串中使用的是 C 的转义句法(例如 “/n”)， 
所以在 LIKE 字符串中使用的任何一个 “/” 必须被双写。 
例如，为了查找 “/n”，必须以 “//n” 形式指定它。为了查找 “/”，必须指定它为 “////” 
(反斜线被语法分析器剥离一次，另一次在模式匹配时完成，留下一条单独的反斜线被匹配)。
```

## 查询优化

```sql
select benchmark(100000000, 'afoobar' like '%foo%');

SELECT BENCHMARK(100000000,LOCATE('foo','foobar') > 0);

SELECT BENCHMARK(100000000,POSITION('foo' IN 'foobar') > 0);

SELECT BENCHMARK(100000000,INSTR('foobar', 'foo') > 0);

FIND_IN_SET，逗号分割
```

[MySQL LIKE vs LOCATE](https://stackoverflow.com/questions/7499438/mysql-like-vs-locate)

```sql
MySQL手册中find_in_set函数的语法：
FIND_IN_SET(str,strlist)

str 要查询的字符串
strlist 字段名 参数以”,”分隔 如 (1,2,6,8)
查询字段(strlist)中包含(str)的结果，返回结果为null或记录

假如字符串str在由N个子链组成的字符串列表strlist 中，则返回值的范围在 1 到 N 之间。 一个字符串列表就是一个由一些被 ‘,’ 符号分开的子链组成的字符串。如果第一个参数是一个常数字符串，而第二个是type SET列，则FIND_IN_SET() 函数被优化，使用比特计算。 如果str不在strlist 或strlist 为空字符串，则返回值为 0 。如任意一个参数为NULL，则返回值为 NULL。这个函数在第一个参数包含一个逗号(‘,’)时将无法正常运行。
```