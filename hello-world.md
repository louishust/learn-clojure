# 你好，Clojure

-----------------------
* [安装Clojure](#安装clojure)
  * [Mac安装](#mac安装)
  * [Linux安装](#linux安装)
  * [在线使用](#在线使用)
* [你好,Clojure](#你好clojure)
* [lein使用](#lein使用)
  * [安装lein](#安装lein)
  * [创建项目](#创建项目)
  * [运行项目](#运行项目)
  * [运行测例](#运行测例)
* [小技巧](#小技巧)
  * [查看文档](#查看文档)
  * [查看源码](#查看源码)
* [参考文献](#参考文献)
-----------------------

## 安装Clojure

### Mac安装

```bash
brew install clojure
```

### Linux安装

```bash
curl -O https://download.clojure.org/install/linux-install-1.9.0.391.sh
chmod +x linux-install-1.9.0.391.sh
sudo ./linux-install-1.9.0.391.sh
```

### 在线使用

没有环境安装，可以尝试[Online Cloujre](https://repl.it/languages/clojure)


## 你好,Clojure

```bash
$  clj
Clojure 1.9.0
user=> (print "Hello, Clojure!")
"Hello, Clojure!"nil
```

`print`是函数表达式，`"Hello, Clojure!"`作为print的参数。

这里说明下，Clojure中一切皆是表达式, 包括控制流，比如`if`等。

## lein使用

lein用于管理clojure项目，lein的作用有点类似于管理java项目的maven.

### 安装lein

```bash
$ mkdir -p ~/bin
$ cd ~/bin
$ curl -O https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
$ chmod a+x lein
```

### 创建项目


创建一个基于`app`模板的项目`my-stuff`
```
$ lein new app my-stuff

Generating a project called my-stuff based on the 'app' template.
```


**项目目录结构**

```bash
$ tree
.
├── LICENSE
├── README.md
├── doc
│   └── intro.md
├── project.clj
├── resources
├── src
│   └── my_stuff
│       └── core.clj
└── test
    └── my_stuff
        └── core_test.clj

6 directories, 6 files
```

`src/`目录是源码目录，`test/`是测例目录，`project.clj`描述项目，
`src/my_stuff/core.clj`对应`my-stuff.core`命名空间。


修改项目文件`project.clj`，修改`description`，并添加`dependencies`.

```
(defproject my-stuff "0.1.0-SNAPSHOT"
  :description "my stuff"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.9.0"] [clj-http "2.0.0"]]
  :main ^:skip-aot my-stuff.core
  :target-path "target/%s"
  :profiles {:uberjar {:aot :all}})
```

这里添加了`[clj-http "2.0.0]`, 同时修改了默认的`org.clojure/clojure`版本，提升为`1.9.0`,因为后续需要使用到`spec`库。


### 运行项目


#### REPL交互式

使用REPL(read-eval-print loop)方式启动.

```
$ lein repl
nREPL server started on port 63973 on host 127.0.0.1 - nrepl://127.0.0.1:63973
REPL-y 0.3.2, nREPL 0.2.3
Clojure 1.6.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_60-b27
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e
```

REPL类似python，是交互式的。


#### Run

```
$ lein run
Hello, World!
```

### 运行测例

```
$ lein test

lein test my-stuff.core-test

lein test :only my-stuff.core-test/a-test

FAIL in (a-test) (core_test.clj:7)
FIXME, I fail.
expected: (= 0 1)
  actual: (not (= 0 1))

Ran 1 tests containing 1 assertions.
1 failures, 0 errors.
Tests failed.
```
默认生成的测例是失败的，这里提示断言错误。


## 小技巧

### 查看文档

```
my-stuff.core=> (doc sorted-set)
-------------------------
clojure.core/sorted-set
([& keys])
  Returns a new sorted set with supplied keys.  Any equal keys are
  handled as if by repeated uses of conj.
nil
my-stuff.core=> (apropos "sorted-set")
(sorted-set sorted-set-by)
my-stuff.core=> (find-doc "sorted-set")
-------------------------
clojure.core/sorted-set
([& keys])
  Returns a new sorted set with supplied keys.  Any equal keys are
  handled as if by repeated uses of conj.
-------------------------
clojure.core/sorted-set-by
([comparator & keys])
  Returns a new sorted set with supplied keys, using the supplied
  comparator.  Any equal keys are handled as if by repeated uses of
  conj.
nil
```

`doc`用于精确查找文档，`apropos`用于模糊查找定义，`find-doc`用于模糊查找文档

### 查看源码

```
my-stuff.core=> (source sorted-set)
(defn sorted-set
  "Returns a new sorted set with supplied keys.  Any equal keys are
  handled as if by repeated uses of conj."
  {:added "1.0"
   :static true}
  ([& keys]
   (clojure.lang.PersistentTreeSet/create keys)))
nil
```

Clojure这点非常好用，`source`直接查看某个的源码实现。


## 参考文献

1. [Getting Started](https://clojure.org/guides/getting_started)
2. [leiningen](https://leiningen.org/)
3. [Clojure from the ground up: welcome](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome)
4. [lein tutorial](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md)


