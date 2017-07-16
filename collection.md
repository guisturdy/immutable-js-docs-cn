## Collection

Collection是一组可迭代的键值对集合，它也是所有immutable的基类，确保它们能使用集合的方法（如map，filter）。

```
class Collection<K, V> extends ValueObject
```

注意：一个集合的迭代顺序将是固定的，尽管这个顺序不总是能定义，如`Map`和`Set`那样。

Collection对于实际的数据结构是抽象的基类，他不能被直接的实例化。

实例化他需要扩展以下任意一个子类，`Collection.Keyed`, `Collection.Indexed`或者 `Collection.Set`.

###### 构造器

##### Collection()

构造一个Collection。

```
Collection<I>(collection: I): I
Collection<T>(collection: Iterable<T>): Collection.Indexed<T>
Collection<V>(obj: {[key: string]: V}): Collection.Keyed<string, V>
```

生成的集合类型将取决于输入。

* 当输入为`Collection`，则返回同样为`Collection`
* 当输入为`Collection`，则返回同类型（Keyed, Indexed 或者 Set）的`Collection`
* 当输入为数组类似时，返回`Collection.Indexed`。
* 当输入为具有迭代器（Iterator）的对象时，返回`Collection.Indexed`。
* 当输入为迭代器（Iterator）时，返回`Collection.Indexed`。
* 当输入为对象时，返回`Collection.Keyed`。

此方法强制将对象和字符串转换为collection，如果你希望确保返回值为Collectin，请使用`Seq.of`。

##### 类型

`Collection.Keyed`

###### 等值比较

##### equals()
如果当前集合和另一个集合比较为相等，那么返回true，是否相等由`Immutable.is()`定义。

```
equals(other: any): boolean
```

覆盖

`ValueObject#equals`

注意：此方法与`Immutable.is(this, other)`等效，提供此方法是为了方便能够链式地使用。

##### hashCode()

计算并返回这个集合的哈希值。

```
hashCode(): number
```

覆盖

`ValueObject#hashCode`

集合的`hashCode`用于确定两个集合的相等性，在添加到`Set`或者被作为`Map`的键值时用于检测两个实例是否相等而会被使用到。

```
const a = List([ 1, 2, 3 ]);
const b = List([ 1, 2, 3 ]);
assert(a !== b); // different instances
const set = Set([ a ]);
assert(set.has(b) === true);
```

当两个值的`hashCode`相等时，并[不能完全保证他们是相等的](https://zh.wikipedia.org/wiki/碰撞_%28计算机科学%29)，但当他们的`hashCode`不同时，他们一定是不等的。

###### 等值比较

##### equals()
如果当前集合和另一个集合比较为相等，那么返回true，是否相等由`Immutable.is()`定义。

```
equals(other: any): boolean
```

注意：此方法与`Immutable.is(this, other)`等效，提供此方法是为了方便能够链式地使用。

##### hashCode()

计算并返回这个集合的哈希值。

```
hashCode(): number
```

集合的`hashCode`用于确定两个集合的相等性，在添加到`Set`或者被作为`Map`的键值时用于检测两个实例是否相等而会被使用到。

```
const a = List([ 1, 2, 3 ]);
const b = List([ 1, 2, 3 ]);
assert(a !== b); // different instances
const set = Set([ a ]);
assert(set.has(b) === true);
```

当两个值的`hashCode`相等时，并[不能完全保证他们是相等的](https://zh.wikipedia.org/wiki/碰撞_%28计算机科学%29)，但当他们的`hashCode`不同时，他们一定是不等的。

###### 读值

##### get()

返回提供的索引位置关联的值，或者当提供的索引越界时返回所提供的notSetValue。

```
get<NSV>(index: number, notSetValue: NSV): T | NSV
get(index: number): T | undefined
```

注意：某个键对于的值为`undefined`是可能的，说以如果`notSetValue`没被提供时将会返回`undefined`，不能用这个来确认是否为键值不存在。

##### has()

使用`Immutable.is`判断key值是否在`Collection`中。

```
has(key: number): boolean
```

##### includes()

使用`Immutable.is`判断value值是否在`Collection`中。

```
includes(value: T): boolean
```

别名

`contains()`

##### first()

取得集合第一个值。

```
first(): T | undefined
```

##### last()

取得集合第一个值。

```
last(): T | undefined
```

###### 等值比较

##### equals()
如果当前集合和另一个集合比较为相等，那么返回true，是否相等由`Immutable.is()`定义。

```
equals(other: any): boolean
```

注意：此方法与`Immutable.is(this, other)`等效，提供此方法是为了方便能够链式地使用。

##### hashCode()

计算并返回这个集合的哈希值。

```
hashCode(): number
```

集合的`hashCode`用于确定两个集合的相等性，在添加到`Set`或者被作为`Map`的键值时用于检测两个实例是否相等而会被使用到。

```
const a = List([ 1, 2, 3 ]);
const b = List([ 1, 2, 3 ]);
assert(a !== b); // different instances
const set = Set([ a ]);
assert(set.has(b) === true);
```

当两个值的`hashCode`相等时，并[不能完全保证他们是相等的](https://zh.wikipedia.org/wiki/碰撞_%28计算机科学%29)，但当他们的`hashCode`不同时，他们一定是不等的。

###### 读值

##### get()

返回提供的索引位置关联的值，或者当提供的索引越界时返回所提供的notSetValue。

```
get<NSV>(index: number, notSetValue: NSV): T | NSV
get(index: number): T | undefined
```

继承自

`Collection.Indexed#get`

`index`可以为负值，表示从集合尾部开始索引。`s.get(-1)`取得集合最后一个元素。

##### has()

使用`Immutable.is`判断key值是否在`Collection`中。

```
has(key: number): boolean
```

##### includes()

使用`Immutable.is`判断value值是否在`Collection`中。

```
includes(value: T): boolean
```

##### first()

取得集合第一个值。

```
first(): T | undefined
```

##### last()

取得集合第一个值。

```
last(): T | undefined
```

###### 读取深层数据

##### getIn()

返回根据提供的路径或者索引搜索到的嵌套的值。

```
getIn(searchKeyPath: Iterable<any>, notSetValue?: any): any
```

##### hasIn()

根据提供的路径或者索引检测该处是否设置了值。

```
hasIn(searchKeyPath: Iterable<any>): boolean
```

###### 修改持久化

##### update()

这将是一个很有用的方法来将两个普通方法进行链式调用。RxJS中为"let"，lodash中为"thru"。

```
update<R>(updater: (value: this) => R): R
```
例如，在进行map和filter操作后计算总和操作：

```
const { Seq } = require('immutable')

function sum(collection) {
  return collection.reduce((sum, x) => sum + x, 0)
}

Map({ x: 1, y: 2, z: 3 })
  .map(x => x + 1)
  .filter(x => x % 2 === 0)
  .update(sum)
// 6
```

###### 转换为JavaScript类型

##### toJS()

深层地将这个有序的集合转换转换为原生JS数组或对象。

```
toJS(): Array<any>
```

`Collection.Index`与`Collection.Set`转换为`Array`,而`Collection.Keyed`转换为`Object`键将会转换为字符串。

##### toJSON()

浅转换这个有序的集合为原生JS数组或对象。

```
toJSON(): Array<any>
```

`Collection.Index`与`Collection.Set`转换为`Array`,而`Collection.Keyed`转换为`Object`键将会转换为字符串。

##### toArray()

浅转换这个有序的集合为原生JS数组并且丢弃key。

```
toArray(): Array<any>
```

##### toObject()

浅转换这个有序的集合为原生JS对象。

```
toObject(): {[key: string]: V}
```

###### 转换为集合

##### toMap()

将此集合转换为Map，如果键不可哈希，则抛弃。

```
toMap(): Map<number, T>
```

注意：这和`Map(this.toKeyedSeq())`等效，为了能够方便的进行链式调用而提供。

##### toOrderedMap()

将此集合转换为Map，保留索引的顺序。

```
toOrderedMap(): OrderedMap<number, T>
```

注意：这和`OrderedMap(this.toKeyedSeq())`等效，为了能够方便的进行链式调用而提供。

##### toSet()

将此集合转换为Set，如果值不可哈希，则抛弃。

```
toSet(): Set<T>
```

注意：这和`Set(this)`等效，为了能够方便的进行链式调用而提供。

##### toOrderSet()

将此集合转换为Set，保留索引的顺序。

```
toOrderedSet(): OrderedSet<T>
```

注意：这和`OrderedSet(this.valueSeq())`等效，为了能够方便的进行链式调用而提供。

##### toList()

将此集合转换为List，丢弃键值。

```
toList(): List<T>
```

此方法和`List(collection)`类似，为了能够方便的进行链式调用而提供。然而，当在`Map`或者其他有键的集合上调用时，`collection.toList()`会丢弃键值，同时创建一个只有值的list，而`List(collection)`使用传入的元组创建list。

```
const { Map, List } = require('immutable')
var myMap = Map({ a: 'Apple', b: 'Banana' })
List(myMap) // List [ [ "a", "Apple" ], [ "b", "Banana" ] ]
myMap.toList() // List [ "Apple", "Banana" ]
```

##### toStack()

将此集合转换为Stack，丢弃键值，抛弃不可哈希的值。

```
toStack(): Stack<T>
```

注意：这和`Stack(this)`等效，为了能够方便的进行链式调用而提供。

###### 转换为Seq

##### toSeq()

返回Seq.Indexed。

```
toSeq(): Seq.Indexed<T>
```

##### toKeyedSeq()

从这个集合返回一个Seq.Keyed，其中索引将视作key。

```
toKeyedSeq(): Seq.Keyed<number, T>
```

如果你想对Collection.Indexed操作返回一组[index, value]对，这将十分有用。

返回的Seq将与Colleciont有相同的索引顺序。

```
const { Seq } = require('immutable')
const indexedSeq = Seq([ 'A', 'B', 'C' ])
// Seq [ "A", "B", "C" ]
indexedSeq.filter(v => v === 'B')
// Seq [ "B" ]
const keyedSeq = indexedSeq.toKeyedSeq()
// Seq { 0: "A", 1: "B", 2: "C" }
keyedSeq.filter(v => v === 'B')
// Seq { 1: "B" }
```

##### toIndexedSeq()

将这个集合的值丢弃键(key)返回为Seq.Indexed。

```
toIndexedSeq(): Seq.Indexed<T>
```

##### toSetSeq()

将这个集合的值丢弃键(key)返回为Seq.Set。

```
toSetSeq(): Seq.Set<T>
```

###### 迭代器

##### keys()

一个关于`Collection`键的迭代器。

```
keys(): IterableIterator<number>
```

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`keySeq`来满足需求。

##### values()

一个关于`Collection`值的迭代器。

```
values(): IterableIterator<T>
```

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`valueSeq`来满足需求。

##### entries()

一个关于`Collection`条目的迭代器，是`[ key, value ]`这样的元组数据。

```
entries(): IterableIterator<[number, T]>
```

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`entrySeq`来满足需求。

###### 集合（Seq）

##### keySeq()

返回一个新的Seq.Indexed，其包含这个集合的键值。

```
keySeq(): Seq.Indexed<number>
```

##### valueSeq()

返回一个新的Seq.Indexed，其包含这个集合的所有值。

```
valueSeq(): Seq.Indexed<T>
```

##### entrySeq()

返回一个新的Seq.Indexed，其为[key, value]这样的元组。

```
entrySeq(): Seq.Indexed<[number, T]>
```

###### 序列算法

##### map\(\)

返回一个由传入的`mapper`函数处理过值的新Set。

```
map<M>(mapper: (value: T, key: number, iter: this) => M, context?: any): Set<M>
```

例

```
Set([ 1, 2 ]).map(x => 10 * x)
// Set [ 10, 20 ]
```

注意：`map()`总是返回一个新的实例，即使它产出的每一个值都与原始值相同。

##### filter\(\)

返回一个只有由传入方法`predicate`返回为true的值组成的新Set。

```
filter<F>(
predicate: (value: V, key: K, iter: this) => boolean,
context?: any
): Collection<K, F>
filter(predicate: (value: V, key: K, iter: this) => any, context?: any): this
```

例

```
const { Map } = require('immutable')
Map({ a: 1, b: 2, c: 3, d: 4}).filter(x => x % 2 === 0)
// Map { "b": 2, "d": 4 }
```

注意：`filter()`总是返回一个新的实例，即使它的结果没有过滤掉任何一个值。

##### filterNot\(\)

返回一个由所提供的`predicate`方法返回false过滤的新的相同类型的集合。

```
filterNot(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

例

```
const { Map } = require('immutable')
Map({ a: 1, b: 2, c: 3, d: 4}).filterNot(x => x % 2 === 0)
// Map { "a": 1, "c": 3 }
```

注意：`filterNot`总是返回一个新的实例，即使它没有过滤掉任何一个值。

##### reverse()

返回为一个逆序的新的同类型集合。

```
reverse(): this
```

##### sort()

返回一个使用传入的`comparator`重新排序的新同类型集合。

```
sort(comparator?: (valueA: T, valueB: T) => number): this
```

如果没有提供`comparator`方法，那么默认的比较将使用`<`和`>`。

`comparator(valueA, valueB):`

* 返回值为`0`这个元素将不会被交换。
* 返回值为`-1`(或者任意负数)`valueA`将会移到`valueB`之前。 
* 返回值为`1`(或者任意正数)`valueA`将会移到`valueB`之后。 
* 为空，这将会返回相同的值和顺序。

当被排序的集合没有定义顺序，那么将会返回同等的有序集合。比如`map.sort()`将返回OrderedMap。

```
const { Map } = require('immutable')
Map({ "c": 3, "a": 1, "b": 2 }).sort((a, b) => {
  if (a < b) { return -1; }
  if (a > b) { return 1; }
  if (a === b) { return 0; }
});
// OrderedMap { "a": 1, "b": 2, "c": 3 }
```

注意：`sort()`总是返回一个新的实例，即使它没有改变排序。

##### sort()

返回一个使用传入的`comparator`重新排序的新同类型集合。

```
sort(comparator?: (valueA: T, valueB: T) => number): this
```

如果没有提供`comparator`方法，那么默认的比较将使用`<`和`>`。

`comparator(valueA, valueB):`

* 返回值为`0`这个元素将不会被交换。
* 返回值为`-1`(或者任意负数)`valueA`将会移到`valueB`之前。 
* 返回值为`1`(或者任意正数)`valueA`将会移到`valueB`之后。 
* 为空，这将会返回相同的值和顺序。

当被排序的集合没有定义顺序，那么将会返回同等的有序集合。比如`map.sort()`将返回OrderedMap。

```
const { Map } = require('immutable')
Map({ "c": 3, "a": 1, "b": 2 }).sort((a, b) => {
  if (a < b) { return -1; }
  if (a > b) { return 1; }
  if (a === b) { return 0; }
});
// OrderedMap { "a": 1, "b": 2, "c": 3 }
```

注意：`sort()`总是返回一个新的实例，即使它没有改变排序。

##### sortBy()

与`sort`类似，但能接受一个`comparatorValueMapper`方法，它允许通过更复杂的方式进行排序：

```
sortBy<C>(
    comparatorValueMapper: (value: T, key: number, iter: this) => C,
    comparator?: (valueA: C, valueB: C) => number
): this
```

例

```
hitters.sortBy(hitter => hitter.avgHits)
```

注意：`sortBy()`总是返回一个新的实例，即使它没有改变排序。

##### groupBy()

返回一个`Collection.Keyeds`的`Collection.keyed`，由传入的`grouper`方法分组。

```
groupBy<G>(
    grouper: (value: T, key: number, iter: this) => G,
    context?: any
): Seq.Keyed<G, Collection<number, T>>
```

注意：这不总是一个立即地操作。

```
const { List, Map } = require('immutable')
const listOfMaps = List([
  Map({ v: 0 }),
  Map({ v: 1 }),
  Map({ v: 1 }),
  Map({ v: 0 }),
  Map({ v: 2 })
])
const groupsOfMaps = listOfMaps.groupBy(x => x.get('v'))
// Map {
//   0: List [ Map{ "v": 0 }, Map { "v": 0 } ],
//   1: List [ Map{ "v": 1 }, Map { "v": 1 } ],
//   2: List [ Map{ "v": 2 } ],
// }
```

###### 副作用

##### forEach()

`sideEffect`将会对集合上每个元素执行。

```
forEach(
    sideEffect: (value: T, key: number, iter: this) => any,
    context?: any
): number
```

与`Array#forEach`不同，任意一个`sideEffect`返回`false`都会停止循环。函数将返回所有参与循环的元素（包括最后一个返回false的那个）。

###### 创建子集

##### slice()

返回一个新的相同类型的相当于原集合指定范围的元素集合，包含开始索引但不包含结束索引位置的值。

```
slice(begin?: number, end?: number): this
```

如果起始值为负，那么表示从集合结束开始查找。例如`slice(-2)`返回集合最后两个元素。如果没有提供，那么新的集合将会从最开始那个元素开始。

如果终止值为负，表示从集合结束开始查找。例如`silice(0, -1)`返回除集合最后一个元素外所有元素。如果没提供，新的集合将会包含到原集合最后一个元素。

如果请求的子集与原集合相等，那么将会返回原集合。

##### rest()

返回一个不包含原集合第一个元素的新的同类型的集合。

```
rest(): this
```

继承自 

`Collection#rest`

##### butLast()

返回一个不包含原集合最后一个元素的新的同类型的集合。

```
butLast(): this
```

继承自 

`Collection#butLast`

##### skip()

返回一个不包含原集合从头开始`amount`个数元素的新的同类型集合。

```
skip(amount: number): this
```

##### skipLast()

返回一个不包含原集合从结尾开始`amount`个数元素的新的同类型集合。

```
skipLast(amount: number): this
```

##### skipWhile()

返回一个原集合从`predicate`返回false那个元素开始的新的同类型集合。

```
skipWhile(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

例

```
const { List } = require('immutable')
List([ 'dog', 'frog', 'cat', 'hat', 'god' ])
  .skipWhile(x => x.match(/g/))
// List [ "cat", "hat", "god" ]
```

##### skipUntil()

返回一个原集合从`predicate`返回true那个元素开始的新的同类型集合。

```
skipUntil(
predicate: (value: T, key: number, iter: this) => boolean,
context?: any
): this
```

例

```
const { List } = require('immutable')
List([ 'dog', 'frog', 'cat', 'hat', 'god' ])
  .skipUntil(x => x.match(/hat/))
// List [ "hat", "god"" ]
```

###### take()

返回一个包含原集合从头开始的`amount`个元素的新的同类型集合。

```
take(amount: number): this
```

##### takeLast()

返回一个包含从原集合结尾开始的`amount`个元素的新的同类型集合。

```
takeLast(amount: number): this
```

##### takeWhile()

返回一个包含原集合从头开始的`prediacte`返回true的那些元素的新的同类型集合。

```
takeWhile(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

例

```
const { List } = require('immutable')
List([ 'dog', 'frog', 'cat', 'hat', 'god' ])
  .takeWhile(x => x.match(/o/))
// List [ "dog", "frog" ]
```

##### takeUntil()

返回一个包含原集合从头开始的`prediacte`返回false的那些元素的新的同类型集合。

```
takeUntil(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

例

```
const { List } = require('immutable')
List([ 'dog', 'frog', 'cat', 'hat', 'god' ])
  .takeUntil(x => x.match(/at/))
// List [ "dog", "frog" ]
```

###### 组合

##### cancat\(\)

将其他的值或者集合与这个Set串联起来返回为一个新Set。

```
concat<C>(...valuesOrCollections: Array<Iterable<C> | C>): Set<T | C>
```

##### flatten()

压平嵌套的集合。

```
flatten(depth?: number): Collection<any, any>
flatten(shallow?: boolean): Collection<any, any>
```

默认会深度地经常压平集合操作，返回一个同类型的集合。可以指定`depth`为压平深度或者是否深度压平（为true表示仅进行一层的浅层压平）。如果深度为0（或者shllow:false）将会深层压平。

压平仅会操作其他集合，数组和对象不会进行此操作。

注意：`flatten(true)`操作是在集合上进行，同时返回一个集合。

##### flatMap\(\)

扁平化这个Set为一个新Set。

```
flatMap<M>(
mapper: (value: T, key: number, iter: this) => Iterable<M>,
context?: any
): Set<M>
```

与`set.map(...).flatten(true)`相似。

###### 减少值

##### reduce()

将传入的方法`reducer`在集合每个元素上调用并传递缩减值，以此来缩减集合的值。

```
reduce<R>(
    reducer: (reduction: R, value: T, key: number, iter: this) => R,
    initialReduction: R,
    context?: any
): R
reduce<R>(reducer: (reduction: T | R, value: T, key: number, iter: this) => R): R
```

见

`Array#reduce`

如果`initialReduction`未提供，那么将会使用集合第一个元素。

##### reduceRight()

逆向地缩减集合的值（从结尾开始）。

```
reduceRight<R>(
    reducer: (reduction: R, value: T, key: number, iter: this) => R,
    initialReduction: R,
    context?: any
): R
reduceRight<R>(
    reducer: (reduction: T | R, value: T, key: number, iter: this) => R
): R
```

注意：与this.reverse().reduce()等效，为了与`Array#reduceRight`看齐而提供。

##### every()

当集合中所有元素`predicate`都判定为true时返回ture。

```
every(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): boolean
```

##### some()

当集合中任意元素`predicate`判定为true时返回ture。

```
some(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): boolean
```

##### join()

将值连接为字符串，并且在每两个值之间插入分割。默认分隔为`","`。

```
join(separator?: string): string
```

##### isEmpty()

当集合不包含值时返回true。

```
isEmpty(): boolean
```

对于惰性`Seq`，isEmpty会对他经常迭代来确定是否为空。至少会迭代一次。

##### count()

返回集合的大小。

```
count(): number
count(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): number
```

不管此集合是否惰性地确定大小（某些Seq不能），这个方法将总是返回正确的大小。如果必要，他将会评估一个惰性的Seq。

如果`predicate`提供了，方法返回的数量将是集合中`predicate`返回true的元素个数。

##### countBy()

返回`Seq.Keyed`的数量，由`grouper`方法将值分组。

```
countBy<G>(
    grouper: (value: T, key: number, iter: this) => G,
    context?: any
): Map<G, number>
```

注意：这不是一个惰性操作。

###### 查找

##### find()

返回集合中第一个符合与所提供的断言的值。

```
find(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): T | undefined
```

##### findLast()

返回集合中最后一个符合与所提供的断言的值。

```
findLast(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): T | undefined
```

注意：`predicate`将会逆序地在每个值上调用。

##### findEntry()

返回第一个符合所提供断言的值的[key， value]。

```
findEntry(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): [number, T] | undefined
```

##### findLastEntry()

返回最后一个符合所提供断言的值的[key， value]。

```
findLastEntry(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): [number, T] | undefined
```

注意：`predicate`将会逆序地在每个值上调用。

##### findKey()

返回第一个`predicate`返回为true的键。

```
findKey(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): number | undefined
```

##### findLastKey()

返回最后一个`predicate`返回为true的键。

```
findLastKey(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): number | undefined
```

注意：`predicate`将会逆序地在每个值上调用。

##### keyOf()

返回与提供的搜索值关联的键，或者undefined。

```
keyOf(searchValue: T): number | undefined
```

##### lastKeyOf()

返回最后一个与提供的搜索值关联的键或者undefined。

```
lastKeyOf(searchValue: T): number | undefined
```

##### max()

返回集合中最大的值。如果有多个值比较为相等，那么将返回第一个。

```
max(comparator?: (valueA: T, valueB: T) => number): T | undefined
```

`comparator`的使用方法与`Collection#sort`是一样的，如果未提供那么默认的比较为`>`。

当两个值比较为相等时，第一个遇见的值将会被返回。另一方面，如果`comparator`是可交换的，那么`max`将会独立于输入的顺序。默认的比较器`>`*只有*在类型不一致时才可交换。

如果`comparator`返回0或者值为NaN、undefined或者null，这个值将会被返回。

##### maxBy()

和`max`类似，但还能接受一个`comparatorValueMapper`来实现更复杂的比较。

```
maxBy<C>(
    comparatorValueMapper: (value: T, key: number, iter: this) => C,
    comparator?: (valueA: C, valueB: C) => number
): T | undefined
```

例

```
hitters.maxBy(hitter => hitter.avgHits)
```

##### min()

返回集合中最小的值，如果有多个值比较为相等，将会返回第一个。

```
min(comparator?: (valueA: T, valueB: T) => number): T | undefined
```

当两个值比较为相等时，第一个遇见的值将会被返回。另一方面，如果`comparator`是可交换的，那么`min`将会独立于输入的顺序。默认的比较器`<`*只有*在类型不一致时才可交换。

如果`comparator`返回0或者值为NaN、undefined或者null，这个值将会被返回。

##### minBy()

和`min`类似，但还能接受一个`comparatorValueMapper`来实现更复杂的比较。

```
minBy<C>(
    comparatorValueMapper: (value: T, key: number, iter: this) => C,
    comparator?: (valueA: C, valueB: C) => number
): T | undefined
```

例

```
hitters.minBy(hitter => hitter.avgHits)
```

###### 对比

##### isSubset()

如果`iter`包含集合中所有元素则返回true。

```
isSubset(iter: Iterable<T>): boolean
```

##### isSuperset()

如果集合包含`iter`中所有元素则返回true。

```
isSuperset(iter: Iterable<T>): boolean
```