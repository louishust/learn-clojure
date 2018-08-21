# 语法

---------------
* [基本类型](#基本类型)
  * [数字类型](#数字类型)
  * [字符类型](#字符类型)
  * [集合类型](#集合类型)
  * [其它类型](#其它类型)
* [表达式计算](#表达式计算)
* [REPL](#repl)
* [变量定义](#变量定义)
* [参考文献](#参考文献)
---------------

## 基本类型

### 数字类型

```
42              ; Long - 64-bit integer (from -2^63 to 2^63-1)
6.022e23        ; Double - double-precision 64-bit floating point
42N             ; BigInt - arbitrary precision integer
1.0M            ; BigDecimal - arbitrary precision fixed-point decimal
22/7            ; Ratio
```

### 字符类型

```
"hello"         ; String
\e              ; Character
```


### 集合类型


```
'(1 2 3)     ; list
[1 2 3]      ; vector
#{1 2 3}     ; set
{:a 1, :b 2} ; map
```

### 其它类型

```
nil             ; null value
true            ; Boolean (also, false)
#"[0-9]+"       ; Regular expression
:alpha          ; Keyword
:release/alpha  ; Keyword with namespace
map             ; Symbol
+               ; Symbol - most punctuation allowed
clojure.core/+  ; Namespaced symbol
```

## 表达式计算


```
user=> (+ 3 4)
7
```

`(+ 3 4)`是一个典型的**Clojure表达式**, `(`用于表示开始计算，`+`是符号，表示后续计算是`+`函数计算，
`3`和`4`是常量表达式，作为`+`函数的参数传入进去。

Clojure的语法起源于Lisp语言，语言风格与传统的语言比如`c`, `java`完全不同，`(`前置表示唤起计算。
这点对于习惯了传统语言风格的人来说，看起来有点不太适应，连`+`这种运算符也要通过`(`方式计算，
导致了大量的括号。有人吐槽Clojure语言括号太多了。。。


## REPL

Read-Eval-Print-Loop, 交互式开发环境，适合自己测试、学习、实验。


* *1 (上一个表达式的结果)
* *2 (倒数第2个表达式的结果)
* *3 (倒数第3个表达式的结果)

```
my-stuff.core=> 1
1
my-stuff.core=> 2
2
my-stuff.core=> 3
3
my-stuff.core=> (println *1 *2 *3)
3 2 1
```

## 变量定义

通过`def`特殊的定义方式，将数据绑定到变量上。  
其它强类型语言的变量定义后，变量的类型不能改变，
但是在REPL下，变量的类型是可变的。

```
my-stuff.core=> (def x 7)
#'my-stuff.core/x
my-stuff.core=> (println x)
7
my-stuff.core=> (def x "aaa")
#'my-stuff.core/x
```

## 参考文献

1. [syntax](https://clojure.org/guides/learn/syntax)
