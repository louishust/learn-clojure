# 函数

---------------
* [定义函数](#定义函数)
* [多元函数](#多元函数)
* [变参函数](#变参函数)
* [匿名函数](#匿名函数)
  * [闭包](#闭包)
* [apply函数](#apply函数)
* [闭包函数](#闭包函数)
---------------


## 定义函数

`defn`用于定义普通函数：

```
;;    name   params         body
;;    -----  ------  -------------------
(defn greet  [name]  (str "Hello, " name) )
```

如上定义了一个函数名为`greet`，参数为`name`的函数, 其返回值是`(str "Hello, " name)`生成的字符串。

** 示例 **

```
my-stuff.core=> (defn greet  [name]  (str "Hello, " name) )
#'my-stuff.core/greet
my-stuff.core=> (greet "Clojure!")
"Hello, Clojure!"
```

## 多元函数

类似C和C++里面的同名不同参函数, 一个参数名可以接收不同的参数。

```
(defn messenger
  ([]     (messenger "Hello world!"))
  ([msg]  (println msg)))
```

上面定义了函数名为`messenger`的函数，其有两种调用方式：无参形式和传递一个参数。


** 示例 **
```
my-stuff.core=> (defn messenger
           #_=>   ([]     (messenger "Hello world!"))
           #_=>   ([msg]  (println msg)))
#'my-stuff.core/messenger
my-stuff.core=> (messenger)
Hello world!
nil
my-stuff.core=> (messenger "Hello!")
Hello!
nil
```

## 变参函数

类似`print`函数，可以接收不定数量的参数：

```
my-stuff.core=> (source print)
(defn print
  "Prints the object(s) to the output stream that is the current value
  of *out*.  print and println produce output for human consumption."
  {:added "1.0"
   :static true}
  [& more]
    (binding [*print-readably* nil]
      (apply pr more)))
```

可以看到函数`print`的定义中，参数使用了`& more`进行定义。
注意：`&`和`more`之间需要有空格。


** 示例 **

```
my-stuff.core=> (defn hello [a & b] (print a b))
#'my-stuff.core/hello

my-stuff.core=> (hello "abc" [1 2 3] {:a 10, :b 20})
20abc ([1 2 3] {:b 20, :a 10})nil
```
定义了`hello`函数，除了固定的第一个参数`a`之外，还定义了一个变参参数`b`.

## 匿名函数

通过`fn`定义匿名函数，语法如下：

```
;;    params         body
;;   ---------  -----------------
(fn  [message]  (println message) )
```

和`defn`的区别是没有了`name`函数名的定义。


```
(defn greet [name] (str "Hello, " name))

(def greet (fn [name] (str "Hello, " name)))
```

上面两种方法的定义相同，第一种是直接定义了一个显示函数，
第二种是定义greet变量为匿名函数。


** 示例 **

```
my-stuff.core=> (def greet (fn [name] (str "Hello, " name)))
#'my-stuff.core/greet
my-stuff.core=> (greet "Clojure!")
"Hello, Clojure!"
```

对于`fn`匿名函数，有一个更加简单的，使用`#()`定义方式：

** 示例 **

```
; %取第一个参数
my-stuff.core=> (def greet #(str "Hello, " %))
#'my-stuff.core/greet
my-stuff.core=> (greet "Clojure!")
"Hello, Clojure!"

; %1取第一个参数
my-stuff.core=> (def greet #(str "Hello, " %1))
#'my-stuff.core/greet
my-stuff.core=> (greet "Clojure!")
"Hello, Clojure!"

; 调用多个参数会报错
my-stuff.core=> (greet "Clojure!" "test")

ArityException Wrong number of args (2) passed to: core/greet  clojure.lang.AFn.throwArity (AFn.java:429)

; 变参
my-stuff.core=> (def greet #(str "Hello, " %&))
#'my-stuff.core/greet
my-stuff.core=> (greet "Clojure!" "test")
"Hello, (\"Clojure!\" \"test\")"
```

这里使用`%`来取参数，`%1`和`%`表示取第一个参数, `%&`用于取剩余所有的参数。


### 闭包

匿名函数可以作为闭包使用在其它的函数定义中:

```
(defn messenger-builder [greeting]
  (fn [who] (println greeting who))) ; closes over greeting

;; greeting provided here, then goes out of scope
(def hello-er (messenger-builder "Hello"))
```

匿名函数`fn [who]`作为闭包，可以引用上一层的变量`greeting`.
`messenger-builder`函数接收参数`greeting`,返回匿名函数`fn [who]`,
同时定义`hello-er`变量，对应的是`fn [who]`的返回函数。


** 示例 **

```
my-stuff.core=> (defn messenger-builder [greeting]
           #_=>   (fn [who] (println greeting who))) ; closes over greeting
#'my-stuff.core/messenger-builder
my-stuff.core=> (def hello-er (messenger-builder "Hello"))
#'my-stuff.core/hello-er
my-stuff.core=> (hello-er "Clojure!")
Hello Clojure!
nil
```


## apply

`apply`函数最后一个参数必须是一个list，apply用于将list拆分成具体的参数
传入函数`f`

```
(apply f '(1 2 3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 '(2 3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 2 '(3 4))    ;; same as  (f 1 2 3 4)
(apply f 1 2 3 '(4))    ;; same as  (f 1 2 3 4)
```

** 示例 **

```
my-stuff.core=> (defn messenger
           #_=>   ([msg]  (println msg))
           #_=>   ([msg1 msg2]  (println msg1 msg2)))
#'my-stuff.core/messenger
my-stuff.core=> (messenger '("Hello", "Clojure"))
(Hello Clojure)
nil
my-stuff.core=> (apply messenger '("Hello", "Clojure"))
Hello Clojure
nil
```

`apply`将list拆成了两个参数，调用了函数`[msg1 msg2]`.


