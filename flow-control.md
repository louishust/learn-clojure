# 控制流


-----------------------
* [控制流表达式](#控制流表达式)
  * [Truth](#truth)
  * [if](#if)
  * [do](#do)
  * [when](#when)
  * [cond](#cond)
  * [case](#case)
* [迭代器](#迭代器)
  * [dotimes](#dotimes)
  * [doseq](#doseq)
  * [for](#for)
  * [loop和recur](#loop和recur)
  * [defn和recur](#defn和recur)
  * [recur使用限制](#recur使用限制)

-----------------------

## 控制流表达式

在`java`, `c`中，执行流程是通过控制语句(control statement)进行控制。
比如`if`, `while`, `for`等语句。

Clojure中一切均是表达式，包括控制流。

> In Clojure, however, everything is an expression! Everything returns a value, and a block of multiple expressions returns the last value. Expressions that exclusively perform side-effects return nil


### Truth

Clojure中，所有的值都可以进行`true`或`false`的判定，`false`的值有两种: `false`和nil。其它的值都是true。**0也是true, 这一点和其它语言完全不同**


```
my-stuff.core=> (if 0 "true" "false")
"true"
```

### if

`if`文档如下：

```
my-stuff.core=> (doc if)
-------------------------
if
  (if test then else?)
Special Form
  Evaluates test. If not the singular values nil or false,
  evaluates and yields then, otherwise, evaluates and yields else. If
  else is not supplied it defaults to nil.

  Please see http://clojure.org/special_forms#if
```

`if`表达式计算中，`else`是可选的，`test`用来进行判定，如果为`true`，
则执行`then`.


**示例**

```
my-stuff.core=> (if true "true" "false")
"true"
my-stuff.core=> (if false "true")
nil
```


### do

`do`类似大括号的作用，构建一个大的代码块。

**示例**

```
my-stuff.core=> (if (even? 5)
           #_=>   (do
           #_=>      (println "even")
           #_=>      (println "true")
           #_=>   )
           #_=>   (do
           #_=>      (println "odd")
           #_=>      (println "false")
           #_=>   )
           #_=> )
odd
false
```

如上，在每个`do`内分别构建了两个表达式。

### when

`when`文档如下：

```
my-stuff.core=> (doc when)
-------------------------
clojure.core/when
([test & body])
Macro
  Evaluates test. If logical true, evaluates body in an implicit do.
```

`when`相当于没有`else`的`if`表达式.

**示例**

```
my-stuff.core=> (when (neg? -10) (println "-10 is neg"))
-10 is neg
```

### cond

`cond`类似于`C`中的`switch...case`语句。

**示例**

```
my-stuff.core=> (def a 10)
my-stuff.core=> (cond
           #_=>   (< a 2) "a < 2"
           #_=>   (< a 100) "2<= a<100"
           #_=>   (= 10) "a=10"
           #_=>   :else "a > 100"
           #_=>  )
```

`cond`采用短路的方式顺序执行，如果找到符合的判定条件，就会跳过其它判定表达式，类似于默认添加了`break`，`fallthrough`真的不是一个好的编程习惯。

**:else**是关键字，所有的关键字表达式都是true,因为false只有两种情况：`false`和`nil`·

### case

```
my-stuff.core=> (doc case)
-------------------------
clojure.core/case
([e & clauses])
Macro
  Takes an expression, and a set of clauses.

  Each clause can take the form of either:

  test-constant result-expr

  (test-constant1 ... test-constantN)  result-expr
```

`case`和`cond`类似，只不过效率更高, 因为`test-constant`必须是常量.


**示例**

```
my-stuff.core=> (defn foo [x]
           #_=>          (case x
           #_=>            5 "x is 5"
           #_=>            10 "x is 10"
           #_=>            "x isn't 5 or 10"))
#'my-stuff.core/foo
my-stuff.core=> (foo 10)
"x is 10"
my-stuff.core=> (foo 100)
"x isn't 5 or 10"
```

最后一个单独的表达式，用来表示`else`情况。

## 迭代器

### dotimes

```
my-stuff.core=> (doc dotimes)
-------------------------
clojure.core/dotimes
([bindings & body])
Macro
  bindings => name n

  Repeatedly executes body (presumably for side-effects) with name
  bound to integers from 0 through n-1.
```

从`0`到`n-1`, 重复执行`body`表达式。


**示例**

```
my-stuff.core=> (dotimes [i 3]
           #_=>          (println i))
0
1
2
```

`i`从0递增到2进行循环。

### doseq

遍历给定的序列对象。


**示例**

```
my-stuff.core=> (doseq [i [1 2 4]] (println i))
1
2
4

my-stuff.core=> (doseq [letter [:a :b] number (range 3)] (println [letter number]))
[:a 0]
[:a 1]
[:a 2]
[:b 0]
[:b 1]
[:b 2]
```

第一个是一个变量`i`绑定到`vector[1 2 4]`中。

第二个是变量`letter`绑定到`[:a :b]`, 变量`number`绑定到`(range 3)`.

###  for

这个并不是其它语言中的`for`控制语句不一样，而是类似`doseq`
用于将不同的序列，重新组件成一个新的list。

**示例**

```
my-stuff.core=> (for [letter [:a :b]
           #_=>              number (range 3)]
           #_=>          [letter number])
([:a 0] [:a 1] [:a 2] [:b 0] [:b 1] [:b 2])
```

### loop和recur

`loop`和`recur`有点类似`label`和`goto`.


```
my-stuff.core=> (loop [i 0]
           #_=>   (if (< i 10)
           #_=>     (recur (inc i))
           #_=>     ))
10
```
`recur`会跳到`loop`重新执行。


### defn和recur

如果需要写递归函数，这时候就会用到`defn`和`recur`的组合。
可以将`defn`看成是隐含一个`loop [parameter]`.

```
(defn increase [i]
  (if (< i 10)
    (recur (inc i))
    i))
```

### recur使用限制

* `recur`必须在`tail position`，即一个分支执行流程的最后。
* 必须按照位置提供对应的symbols
  * loop bindings
  * defn/fn arguments
