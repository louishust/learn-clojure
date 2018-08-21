# 异常

-----------------------
* [异常处理](#异常处理)
* [抛出异常](#抛出异常)
* [异常堆栈](#异常堆栈)
-----------------------

## 异常处理

异常处理和`Java`基本一致, 分为`try`, `catch`和`finally`, 返回值在`finally`之后.

```
my-stuff.core=> (try
           #_=>   (/ 2 1)
           #_=>   (catch ArithmeticException e
           #_=>     "divide by zero")
           #_=>   (finally
           #_=>     (println "cleanup")))
cleanup
2
```

## 抛出异常

通过`throw`抛出异常


```
my-stuff.core=> (try
           #_=>   (throw (Exception. "something went wrong"))

           #_=>   (catch Exception e (.getMessage e)))
"something went wrong"
```

## 异常堆栈

通过`(clojure.repl/pst)`可以打印上一次异常的堆栈。

```
my-stuff.core=> (throw (Exception. "something went wrong"))

Exception something went wrong  my-stuff.core/eval1892 (form-init3451734867643668722.clj:1)
my-stuff.core=> (clojure.repl/pst)
Exception something went wrong
	my-stuff.core/eval1892 (form-init3451734867643668722.clj:1)
	my-stuff.core/eval1892 (form-init3451734867643668722.clj:1)
	clojure.lang.Compiler.eval (Compiler.java:7062)
	clojure.lang.Compiler.eval (Compiler.java:7025)
	clojure.core/eval (core.clj:3206)
	clojure.core/eval (core.clj:3202)
	clojure.main/repl/read-eval-print--8572/fn--8575 (main.clj:243)
	clojure.main/repl/read-eval-print--8572 (main.clj:243)
	clojure.main/repl/fn--8581 (main.clj:261)
	clojure.main/repl (main.clj:261)
	clojure.main/repl (main.clj:177)
	clojure.tools.nrepl.middleware.interruptible-eval/evaluate/fn--795 (interruptible_eval.clj:43)
nil
```

