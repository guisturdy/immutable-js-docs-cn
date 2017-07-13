## Seq

相当于一系列值，但不能具体表现为某种数据结构。

```
class Seq<K, V> extends Collection<K, V>
```

**Seq是不可变的** — 一旦Seq被创建，他将不可被修改、添加值、重新排序以及其它形式地修改。所以，任何应用于`Seq`的方法都将创建一个新的`Seq`。

**Seq是懒惰的（lazy）** — Seq会为回应任意方法的调用做一些微小的工作。值通常都将在迭代中被创建，包括隐式的在缩减操作(reduce)或者转换为类似`List`或JS数组这类强结构数据时。

例如，以下操作将不会执行，因为生成的Seq的值没有被迭代：

```
const { Seq } = require('immutable')
const oddSquares = Seq([ 1, 2, 3, 4, 5, 6, 7, 8 ])
  .filter(x => x % 2 !== 0)
  .map(x => x * x)
```

当Seq被使用时，也将只有被需要的操作才会执行。在以下样例中，没有临时的数据结构会被创建，filter将只会被调用三次，map则仅会调用一次：

```
oddSquares.get(1); // 9
```

Seq能够进行高效地链式操作，可以有效的表达用其它方式可能非常乏味的逻辑。

```
Seq({ a: 1, b: 1, c: 1})
  .flip()
  .map(key => key.toUpperCase())
  .flip()
// Seq { A: 1, B: 1, C: 1 }
```

同时也可以表达用其它方式可能非常消耗内存或者时间的逻辑：

```
const { Range } = require('immutable')
Range(1, Infinity)
  .skip(1000)
  .map(n => -n)
  .filter(n => n % 2 === 0)
  .take(2)
  .reduce((r, n) => r * n, 1)
// 1006008
```

Seq通常用来为API提供丰富集合的JS对象。

```
Seq({ x: 0, y: 1, z: 2 }).map(v => v * 2).toObject();
// { x: 0, y: 2, z: 4 }
```

###### 构造器

##### Seq()

创建一个Seq。

```
Seq<S>(seq: S): S
Seq<K, V>(collection: Collection.Keyed<K, V>): Seq.Keyed<K, V>
Seq<T>(collection: Collection.Indexed<T>): Seq.Indexed<T>
Seq<T>(collection: Collection.Set<T>): Seq.Set<T>
Seq<T>(collection: Iterable<T>): Seq.Indexed<T>
Seq<V>(obj: {[key: string]: V}): Seq.Keyed<string, V>
Seq(): Seq<any, any>
```

返回一个基于输入的特定类型的Seq。

* 当输入为`Seq`，则返回同样为`Seq`
* 当输入为`Collection`，则返回同类型（Keyed, Indexed 或者 Set）的`Seq`
* 当输入为数组类似时，返回`Seq.Indexed`。
* 当输入为具有迭代器（Iterator）的对象时，返回`Seq.Indexed`。
* 当输入为迭代器（Iterator）时，返回`Seq.Indexed`。
* 当输入为对象时，返回`Seq.Keyed`。

###### 静态方法

##### Seq.isSeq()

如果输入为Seq则返回True，他不会强类型数据结构支持如Map，List或者Set。

```
Seq.isSeq(maybeSeq: any): boolean
```

##### Seq.of()

用输入值创建一个Seq。`Seq.Indexed.of()`的别名。

```
Seq.of<T>(...values: Array<T>): Seq.Indexed<T>
```

##### 类型

`Seq.Keyed`

###### 成员

##### size

```
size
```

