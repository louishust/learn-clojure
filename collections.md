# 集合类型


-----------------------
* [Vectors](#vectors)
  * [构建vector](#构建vector)
  * [获取vector元素](#获取vector元素)
  * [添加vector元素](#添加vector元素)
  * [count](#count)
* [Lists](#lists)
  * [构建list](#构建list)
  * [获取list元素](#获取list元素)
  * [添加list元素](#添加list元素)
* [Sets](#sets)
  * [构建Set](#构建set)
  * [获取set元素](#获取set元素)
  * [添加set元素](#添加set元素)
  * [删除set元素](#删除set元素)
  * [检查存在](#检查存在)
  * [排序](#排序)
* [Maps](#maps)
  * [构建map](#构建map)
  * [添加map元素](#添加map元素)
  * [删除map元素](#删除map元素)
  * [查找map元素](#查找map元素)
  * [检查key存在](#检查key存在)
-----------------------

## Vectors

### 构建vector

通过`def`定义`v`为包含`1`,`2`,`3`三个元素的vector。
```
my-stuff.core=> (def v [1 2 3])
#'my-stuff.core/v
```

通过`vector`表达式定义
```
my-stuff.core=> (def v (vector 1 2 3))
#'my-stuff.core/v
my-stuff.core=> v
[1 2 3]
```

### 获取vector元素

通过`get`表达式，按小标获取`v`中的元素, 下标从`0`开始。

```
my-stuff.core=> (get v 0)
1
my-stuff.core=> (get v 2)
3
my-stuff.core=> (get v 3)
nil
```

### 添加vector元素

通过`conj`表达式，添加元素，注意，并不改变v本身，而是生成新的`vector`.

```
my-stuff.core=> (conj v 4)
[1 2 3 4]
my-stuff.core=> v
[1 2 3]
```

### count

通过`count`表达式，获取vector的大小。
```
my-stuff.core=> (count v)
3
```


## Lists

### 构建list

通过`'()`定义包含了4个元素的list

```
my-stuff.core=> (def cards '(10 :ace :jack 9))
#'my-stuff.core/cards
```

### 获取list元素

`first`获取第一个元素，`second`获取第二个元素，`rest`获取除第一个元素之外的所有元素

```
my-stuff.core=> (first cards)
10
my-stuff.core=> (second cards)
:ace
my-stuff.core=> (rest cards)
(:ace :jack 9)
```

还可以像`stack`一样使用, `peek` 获取第一个元素，`pop`获取除第一个元素之外的其它元素.

```
my-stuff.core=> (peek cards)
10
my-stuff.core=> (pop cards)
(:ace :jack 9)
my-stuff.core=> (peek (pop cards))
:ace
```

### 添加list元素

和Vector一样，利用`conj`添加新的元素

```
my-stuff.core=> (conj cards :queen)
(:queen 10 :ace :jack 9)
```

## Sets

Sets是无序的，不存在重复元素的集合。

### 构建Set

`#{}` 是构建Set的格式：

```
my-stuff.core=> (def players #{"Alice", "Bob", "Kelly"})
#'my-stuff.core/players
```

### 添加set元素

同样通过`conj`添加元素

```
my-stuff.core=> (conj players "Fred")
#{"Alice" "Kelly" "Fred" "Bob"}
```

### 删除set元素

因为Sets本身存在元素唯一性，所以存在删除单个元素的操作。
通过`disj`删除元素。

```
my-stuff.core=> (disj players "Bob" "Sal")
#{"Alice" "Kelly"}
```

### 检查set存在

通过`contains`表达式，检查某个元素是否存在于Set中。


```
my-stuff.core=> (contains? players "Kelly")
true
my-stuff.core=> (contains? players "Louis")
false
```

### 排序

配合`conj`和`sorted-set`，可以生成一个排序的Set:

```
my-stuff.core=> (conj (sorted-set) "Bravo" "Charlie" "Sigma" "Alpha")
#{"Alpha" "Bravo" "Charlie" "Sigma"}
```

## Maps

### 构建map

通过`{}`进行map定义, 换行符或者`,`作为分割符。

```
my-stuff.core=> (def scores {"Fred"  1400
           #_=>              "Bob"   1240
           #_=>              "Angela" 1024})
#'my-stuff.core/scores
my-stuff.core=> (def scores {"Fred" 1400, "Bob" 1240, "Angela" 1024})
#'my-stuff.core/scores
```

### 添加map元素

```
my-stuff.core=> (assoc scores "Sally" 0)
{"Fred" 1400, "Bob" 1240, "Angela" 1024, "Sally" 0}
my-stuff.core=> (assoc scores "Bob" 0)
{"Fred" 1400, "Bob" 0, "Angela" 1024}
```

通过`assoc`进行添加，如果`key`不存在，则是`insert`方式，否则是`update`方式。

### 删除map元素

通过`dissoc`进行删除元素。

```
my-stuff.core=> (dissoc scores "Bob")
{"Fred" 1400, "Angela" 1024}
```

### 查找map元素

和vector一样, 通过`get`进行获取, 返回`value`, 如果不存在，返回nil。

```
my-stuff.core=> (get scores "Angela")
1024
my-stuff.core=> (get scores "Non")
nil
```

还可以通过`(set :key)`方式获取, `set`不能为空，否则会抛出异常。

```
my-stuff.core=> (scores "Fred")
1400

my-stuff.core=> (def bad-lookup-map nil)
#'my-stuff.core/bad-lookup-map
my-stuff.core=> (bad-lookup-map :foo)
NullPointerException   my-stuff.core/eval1979 (form-init3451734867643668722.clj:1)
```

如果获取的key不存在，可以提供不存在时返回的默认值。

```
my-stuff.core=> (get scores "Sam" 0)
0
```


### 检查key存在

和`set`一样，通过`contains`进行判断。

```
my-stuff.core=> (contains? scores "Fred")
true
```

