## OrderedMap

一种能保证Map内元素的顺序与他们被set()的先后顺序一致的Map类型。

```
class OrderedMap<K, V> extends Map<K, V>
```

OrderedMap的迭代表现与ES6Map和JS对象一致。

注意`OrderedMap`将会比无序`Map`话费跟多的内存。`OrderedMap#set`复杂度大概为O(log32 N)，但不稳定。

###### 构造器

##### OrderedMap()

构造一个新的不可变OrderedMap()。

```
OrderedMap<K, V>(collection: Iterable<[K, V]>): OrderedMap<K, V>
OrderedMap<T>(collection: Iterable<Iterable<T>>): OrderedMap<T, T>
OrderedMap<V>(obj: {[key: string]: V}): OrderedMap<string, V>
OrderedMap<K, V>(): OrderedMap<K, V>
OrderedMap(): OrderedMap<any, any>
```

用提供的Collection.Keyed或者JS对象创建相同的键值集合或者期望的[K, V]元组集合。

键值对的迭代顺序将会按照提供给构造函数的顺序保存在OrderedMap中。

```
let newOrderedMap = OrderedMap({key: "value"})
let newOrderedMap = OrderedMap([["key", "value"]])
```

###### 静态方法

##### OrderedMap.isOrderedMap()

如果提供的值为OrderedMap，那么会返回true。

```
OrderedMap.isOrderedMap(maybeOrderedMap: any): boolean
```

###### 成员

##### size

```
size
```

###### 系列算法

##### concat()

将传入的集合与当前集合合并返回为新的Map。

```
concat<KC, VC>(...collections: Array<Iterable<[KC, VC]>>): Map<K | KC, V | VC>
concat<C>(...collections: Array<{[key: string]: C}>): Map<K | string, V | C>
```

继承

`Collection#concat`

##### map()

使用`mapper`遍历所有值，将其返回的值返回为新的Map。

```
map<M>(mapper: (value: V, key: K, iter: this) => M, context?: any): Map<K, M>
```

继承自

`Collection#map`

例

```
Map({ a: 1, b: 2 }).map(x => 10 * x)
// Map { a: 10, b: 20 }
```

注意：`map()`始终返回一个新的实例，即使它产生的结果与原Map一致。

##### mapEntries()

```
mapEntries<KM, VM>(
mapper: (entry: [K, V], index: number, iter: this) => [KM, VM],
context?: any
): Map<KM, VM>
```

继承自

`Collection.Keyed#mapEntries`

见

Collection.Keyed.mapEntries


##### flatMap()

返回一个新的扁平化的Map。

```
flatMap<KM, VM>(
mapper: (value: V, key: K, iter: this) => Iterable<[KM, VM]>,
context?: any
): Map<KM, VM>
```

继承自

`Collection#flatMap`

与`data.map(...).flatten(true)`效果一致。

##### filter()

返回只有由方法`predicate`返回为true的那些条目组成的新的Map。

```
filter<F>(
predicate: (value: V, key: K, iter: this) => boolean,
context?: any
): Map<K, F>
filter(predicate: (value: V, key: K, iter: this) => any, context?: any): this
```

重载

`Collection#filter`

注意：`filter()`总是返回一个新的实例，即使没有过滤任何值。

##### filterNot()

返回一个同类型的集合，只包含`predicate`方法返回为false的那些值。

```
filterNot(
predicate: (value: V, key: K, iter: this) => boolean,
context?: any
): this
```

继承自

`Collection#filterNot`

例

```
const { Map } = require('immutable')
Map({ a: 1, b: 2, c: 3, d: 4}).filterNot(x => x % 2 === 0)
// Map { "a": 1, "c": 3 }
```

注意：`filterNot()`总是返回一个新的实例，即使没有过滤任何值。

##### reverse()

返回一个同类型逆序的集合。

```
reverse(): this
```

继承自

`Collection#reverse`

##### sort()

返回一个新的同类型包含相同条目由`comparator`排序的新的集合。

```
sort(comparator?: (valueA: V, valueB: V) => number): this
```

继承自

`Collection#sort`

如果`comparator`未提供，默认比较器为`<`和`>`。

`comparator(valueA, valueB)`:

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

继承自

`Collection#sortBy`

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

继承自

`Collection#groupBy`

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

###### 持久化修改

##### set()

返回一个在原Map基础上包含了新的键值对的新Map。如果key在原函数中已经有相等的存在，那么他将会被替换。

```
set(key: K, value: V): this
```

例

```
const { Map } = require('immutable')
const originalMap = Map()
const newerMap = originalMap.set('key', 'value')
const newestMap = newerMap.set('key', 'newer value')

originalMap
// Map {}
newerMap
// Map { "key": "value" }
newestMap
// Map { "key": "newer value" }
```

注意：`set`可以在`withMutations`中使用。

##### delete()

返回一个新的不包含`key`的新Map。

```
delete(key: K): this
```

别名

```
remove()
```

注意：`delete`在IE8中不能安全地使用，提供是为了镜像ES6中集合的API。

```
const { Map } = require('immutable')
const originalMap = Map({
  key: 'value',
  otherKey: 'other value'
})
// Map { "key": "value", "otherKey": "other value" }
originalMap.delete('otherKey')
// Map { "key": "value" }
```

注意：`delete`可以在`withMutations`中使用。

##### deleteAll()

返回一个不包含所有提供的`key`的新Map。

```
deleteAll(keys: Iterable<K>): this
```

别名

```
removeAll()
```

示例

```
const { Map } = require('immutable')
const names = Map({ a: "Aaron", b: "Barry", c: "Connor" })
names.deleteAll([ 'a', 'c' ])
// Map { "b": "Barry" }
```

注意：`deleteAll`可以在`withMutations`中使用。

##### clear()

返回一个不包含任何键或值的新Map。

```
clear(): this
```

示例

```
const { Map } = require('immutable')
Map({ key: 'value' }).clear()
// Map {}
```

注意：`clear`可以在`withMutations`中使用。

##### update()

将`key`对应的值传入`updater`方法，使用此方法返回的值设置返回的新的Map对应位置的值。

```
update(key: K, notSetValue: V, updater: (value: V) => V): this
update(key: K, updater: (value: V) => V): this
update<R>(updater: (value: this) => R): R
```

**重载**

`Collection#update`

与`map.set(key, updater(map.get(key)))`效果类似。

```
const { Map } = require('immutable')
const aMap = Map({ key: 'value' })
const newMap = aMap.update('key', value => value + value)
// Map { "key": "valuevalue" }
```

这是最常用与在结构化的数据集合上的方法。例如，为了能够在一个层叠的`List`上进行`.push()`操作，`update`和`push`会同时使用。

```
const aMap = Map({ nestedList: List([ 1, 2, 3 ]) })
const newMap = aMap.update('nestedList', list => list.push(4))
// Map { "nestedList": List [ 1, 2, 3, 4 ] }
```

当提供了`notSetValue`，当Map中键对应位置未设置值时他会被提供给`updater`方法。

```
const aMap = Map({ key: 'value' })
const newMap = aMap.update('noKey', 'no value', value => value + value)
// Map { "key": "value", "noKey": "no valueno value" }
```

如果`updater`返回了与原数据相同的值，那么Map将不会被改变。即使提供了`notSetValue`也是一样。

```
const aMap = Map({ apples: 10 })
const newMap = aMap.update('oranges', 0, val => val)
// Map { "apples": 10 }
assert(newMap === map);
```

在ES2015或更高的代码环境中，不建议使用`notSetValue`来支持函数变量默认值。这有助于避免与上述功能有任何潜在的混淆。

这是与默认写法不同的写法将出现的结果的例子：

```
const aMap = Map({ apples: 10 })
const newMap = aMap.update('oranges', (val = 0) => val)
// Map { "apples": 10, "oranges": 0 }
```

如果没提供键，则会返回`updater`的返回值。

```
const aMap = Map({ key: 'value' })
const result = aMap.update(aMap => aMap.get('key'))
// "value"
```

这将是一个很有用的用法来将两个普通方法链式调用。RxJS中为"let"，lodash中为"thru"。

```
function sum(collection) {
  return collection.reduce((sum, x) => sum + x, 0)
}

Map({ x: 1, y: 2, z: 3 })
  .map(x => x + 1)
  .filter(x => x % 2 === 0)
  .update(sum)
// 6
```

注意：`update(key)`可以在`withMutations`中使用。

##### merge()

返回一个新的Map由原Map合并了提供的集合（或者JS对象）。换而言之，这个方法将每个集合中的所有元素都设置到了新的Map。

```
merge(...collections: Array<Collection<K, V> | {[key: string]: V}>): this
```

如果提供的用来`merge`的值不是一个集合（`isCollection`返回false）那么在合并之前它将会被`fromJS`深度地转换。然后，如果提供的值是一个集合，但包含了非集合的JS对象或数组，这些嵌套值将会被保留。

```
const { Map } = require('immutable')
const one = Map({ a: 10, b: 20, c: 30 })
const two = Map({ b: 40, a: 50, d: 60 })
one.merge(two) // Map { "a": 50, "b": 40, "c": 30, "d": 60 }
two.merge(one) // Map { "b": 20, "a": 10, "d": 60, "c": 30 }
```

注意：`merge`可以在`withMutations`中使用。

##### mergeWith()

和`mgere()`类似，`mergeWith()`将原集合与提供的集合（或者JS对象）合并返回为新的Map，当它能使用`merge`来处理冲突。

```
mergeWith(
    merger: (oldVal: V, newVal: V, key: K) => V,
    ...collections: Array<Collection<K, V> | {[key: string]: V}>
): this
```

例

```
const { Map } = require('immutable')
const one = Map({ a: 10, b: 20, c: 30 })
const two = Map({ b: 40, a: 50, d: 60 })
one.mergeWith((oldVal, newVal) => oldVal / newVal, two)
// { "a": 0.2, "b": 0.5, "c": 30, "d": 60 }
two.mergeWith((oldVal, newVal) => oldVal / newVal, one)
// { "b": 2, "a": 5, "d": 60, "c": 30 }
```

注意：`mergeWith`可以在`withMutations`中使用。

##### mergeDeep()

和`merge()`类似，但当两个集合冲突时，依然会合并他们，并且能深层地处理嵌套。

```
const { Map } = require('immutable')
const one = Map({ a: Map({ x: 10, y: 10 }), b: Map({ x: 20, y: 50 }) })
const two = Map({ a: Map({ x: 2 }), b: Map({ y: 5 }), c: Map({ z: 3 }) })
one.mergeDeep(two)
// Map {
//   "a": Map { "x": 2, "y": 10 },
//   "b": Map { "x": 20, "y": 5 },
//   "c": Map { "z": 3 }
// }
```

注意：`mergeDeep`可以在`withMutations`中使用。

##### mergeDeepWith()

和`mergeDeep()`类似，但当两个非集合冲突时，使用`merger`来觉定结果。

```
mergeDeepWith(
merger: (oldVal: V, newVal: V, key: K) => V,
...collections: Array<Collection<K, V> | {[key: string]: V}>
): this
```

例

```
const { Map } = require('immutable')
const one = Map({ a: Map({ x: 10, y: 10 }), b: Map({ x: 20, y: 50 }) })
const two = Map({ a: Map({ x: 2 }), b: Map({ y: 5 }), c: Map({ z: 3 }) })
one.mergeDeepWith((oldVal, newVal) => oldVal / newVal, two)
// Map {
//   "a": Map { "x": 5, "y": 10 },
//   "b": Map { "x": 20, "y": 10 },
//   "c": Map { "z": 3 }
// }
```

注意：`mergeDeepWith`可以在`withMutations`中使用。

###### 深度持久化

##### setIn()

在原Map的`keyPath`位置设置值为`value`并返回为新Map。如果`keyPath`位置无值，那么新的不可变Map将会创建此位置的值。

```
setIn(keyPath: Iterable<any>, value: any): this
```

例

```
const { Map } = require('immutable')
const originalMap = Map({
  subObject: Map({
    subKey: 'subvalue',
    subSubObject: Map({
      subSubKey: 'subSubValue'
    })
  })
})

const newMap = originalMap.setIn(['subObject', 'subKey'], 'ha ha!')
// Map {
//   "subObject": Map {
//     "subKey": "ha ha!",
//     "subSubObject": Map { "subSubKey": "subSubValue" }
//   }
// }

const newerMap = originalMap.setIn(
  ['subObject', 'subSubObject', 'subSubKey'],
  'ha ha ha!'
)
// Map {
//   "subObject": Map {
//     "subKey": "ha ha!",
//     "subSubObject": Map { "subSubKey": "ha ha ha!" }
//   }
// }
```

如果指定位置存在值，但没有`.set()`方法（如Map和List），此时将会抛出异常。

注意：`setIn`可以在`withMutations`中使用。

##### deleteIn()

返回一个移除了原Map`keyPath`位置的新Map。如果`keyPath`处无值，那么将不会发生改变。

```
deleteIn(keyPath: Iterable<any>): this
```

别名

`removeIn()`

注意：`removeIn`可以在`withMutations`中使用。

##### updateIn()

在指定索引位置调用`updater`，并返回为新Map。

```
updateIn(
keyPath: Iterable<any>,
notSetValue: any,
updater: (value: any) => any
): this
updateIn(keyPath: Iterable<any>, updater: (value: any) => any): this
```

这是经常会在层叠的数据集合上使用的方法。如，为了在层叠的`List`上进行`push()`操作，`updateIn`和`push`会同时使用：

```
const { Map, List } = require('immutable')
const map = Map({ inMap: Map({ inList: List([ 1, 2, 3 ]) }) })
const newMap = map.updateIn(['inMap', 'inList'], list => list.push(4))
// Map { "inMap": Map { "inList": List [ 1, 2, 3, 4 ] } }
```

如果`keyPath`位置没有值，那么不可变`Map`将会在此位置创建这个键。如果`keyPath`指定位置为定义值，那么`updater`方法将会使用`notSetValue`来调用，如果`notSetValue`未提供，那么将会传入`undefined`。

```
const map = Map({ a: Map({ b: Map({ c: 10 }) }) })
const newMap = map.updateIn(['a', 'b', 'c'], val => val * 2)
// Map { "a": Map { "b": Map { "c": 20 } } }
```

如果`updater`方法返回了和原值一样的值，那么将不会发生改变，即使提供了`notSetValue`。

```
const map = Map({ a: Map({ b: Map({ c: 10 }) }) })
const newMap = map.updateIn(['a', 'b', 'x'], 100, val => val)
// Map { "a": Map { "b": Map { "c": 10 } } }
assert(newMap === map)
```

当处在ES2015或者更高环境下，不推荐使用`notSetValue`来实现函数变量默认值。这有助于避免任何可能导致与上述特性不符的困惑。

这里提供一个设置了函数变量默认值的例子，以展示这种写法将产生的不同表现：

```
const map = Map({ a: Map({ b: Map({ c: 10 }) }) })
const newMap = map.updateIn(['a', 'b', 'x'], (val = 100) => val)
// Map { "a": Map { "b": Map { "c": 10, "x": 100 } } }
```

如果指定位置存在值，但没有`.set()`方法（如Map和List），此时将会抛出异常。

##### mergeIn()

一个`updateIn`和`merge`的结合体，会一个新的Map在指定路径那个点上执行合并操作。换而言之，这两条语句效果相同：

```
map.updateIn(['a', 'b', 'c'], abc => abc.merge(y))
map.mergeIn(['a', 'b', 'c'], y)
```

```
mergeIn(keyPath: Iterable<any>, ...collections: Array<any>): this
```

注意：`mergeIn`可以在`withMutations`中使用。

##### mergeDeepIn()

一个`updateIn`和`mergeDeep`的结合体，会一个新的Map在指定路径那个点上执行合并操作。换而言之，这两条语句效果相同：

```
map.updateIn(['a', 'b', 'c'], abc => abc.mergeDeep(y))
map.mergeDeepIn(['a', 'b', 'c'], y)
```

```
mergeDeepIn(keyPath: Iterable<any>, ...collections: Array<any>): this
```

注意：`mergeDeepIn`可以在`withMutations`中使用。

###### 暂时改变

##### withMutations()

每次你调用以上方法，它都会新建一个不可变Map。如果一个纯函数调用了多个上述方法来产生需要的值，那么产生的这些中间不可变Map将会对性能和内存造成负担。

```
withMutations(mutator: (mutable: this) => any): this
```

继承自

`Map#withMutations`

如果你需要进行一系列变化来产生新的不可变Map，用`withMutations()`对此Map创造一个临时的可变拷贝的方式来进程变化操作，可以极大地提升性能。事实上这是像`merge`这样复杂地操作实现地方式。

以下例子将会创造2个而不是4个新Map：

```
const { Map } = require('immutable')
const map1 = Map()
const map2 = map1.withMutations(map => {
  map.set('a', 1).set('b', 2).set('c', 3)
})
assert(map1.size === 0)
assert(map2.size === 3)
```

注意：不是所有方法都可以在可变的集合或者是在`withMutations`中使用！查看每个方法的文档可以确认他是否能够安全地使用`withMutations`。

##### asMutable()

另外一种避免产生中间值的不可变Map的方式是创建一个这个集合的可变拷贝。可变拷贝*总是*返回`this`，因此不要用他们来进行比较。你的函数不要将这个可变集合返回出去，请只在函数内使用它来构建新的集合。如果可能的话使用`withMutations`，因为他提供了更易用地API。

```
asMutable(): this
```

继承自

`Map#asMutable`

注意：如果一个集合已经是可变的，`asMutable`将会返回他本身。

注意：不是所有方法都可以在可变的集合或者是在`withMutations`中使用！查看每个方法的文档可以确认他是否能够安全地使用`withMutations`。

##### asImmutable()

与`asMutable`谓之阳所对应的阴。因为他作用于可变集合，此操作是*可变地*并返回自身。一旦执行，这个可变的拷贝将会变成不可变的，这样即可将他安全的从方法中返回出去。

```
asImmutable(): this
```

继承自

`Map#asImmutable`

###### 转换为JavaScript类型

##### toJS()

深层地将这个有序的集合转换转换为原生JS数组。

```
toJS(): Array<any>
```

继承自

`Collection.Index#toJS`

##### toJSON()

浅转换这个有序的集合为原生JS数组。

```
toJSON(): Array<any>
```

继承自

`Collection.Index#toJSON`

##### toArray()

浅转换这个有序的集合为原生JS数组并且丢弃key。

```
toArray(): Array<any>
```

继承自

`Collection#toArray`

##### toObject()

浅转换这个有序的集合为原生JS对象。

```
toObject(): {[key: string]: V}
```

继承自

`Collection#toObject`

键会被转换为String。

###### 转换为Seq

##### toSeq()

返回Seq.Indexed。

```
toSeq(): Seq.Indexed<T>
```

继承自

`Collection.Indexed#toSeq`

##### fromEntrySeq()

如果这个集合是由[key, value]这种原组构成的，那么这将返回这些原组的Seq.Keyed。

```
fromEntrySeq(): Seq.Keyed<any, any>
```

继承自

`Collection.Index#fromEntrySeq`

##### toKeyedSeq()

从这个集合返回一个Seq.Keyed，其中索引将视作key。

```
toKeyedSeq(): Seq.Keyed<number, T>
```

继承自

`Collection#toKeyedSeq`

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

继承自

`Collection#toIndexedSeq`

##### toSetSeq()

将这个集合的值丢弃键(key)返回为Seq.Set。

```
toSetSeq(): Seq.Set<T>
```

继承自

`Collection#toSetSeq`

###### 系列函数

##### flip()

返回一个新的同类型Collection.Keyed，它将原Map的键和值进行交换。

```
flip(): this
```

继承自

`Collection.Keyed#flip`

例

```
const { Map } = require('immutable')
Map({ a: 'z', b: 'y' }).flip()
// Map { "z": "a", "y": "b" }
```

##### \[Symbol.iterator\]()

```
[Symbol.iterator](): IterableIterator<[K, V]>
```

继承自

`Collection.Keyed#[Symbol.iterator]`


###### 等值比较

##### equals()

如果当前集合和另一个集合比较为相等，那么返回true，是否相等由`Immutable.is()`定义。

```
equals(other: any): boolean
```

继承自

`Collection#equals`

注意：此方法与`Immutable.is(this, other)`等效，提供此方法是为了方便能够链式地使用。

##### hashCode()

计算并返回这个集合的哈希值。

```
hashCode(): number
```

继承自

`Collection#hashCode`

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

继承自

`Collection#has`

##### includes()

使用`Immutable.is`判断value值是否在`Collection`中。

```
includes(value: T): boolean
```

继承自

`Collection#includes`

##### first()

取得集合第一个值。

```
first(): T | undefined
```

继承自

`Collection#first`

##### last()

取得集合第一个值。

```
last(): T | undefined
```

继承自

`Collection#last`

###### 读取深层数据

##### getIn()

返回根据提供的路径或者索引搜索到的嵌套的值。

```
getIn(searchKeyPath: Iterable<any>, notSetValue?: any): any
```

继承自

`Collection#getIn`

##### hasIn()

根据提供的路径或者索引检测该处是否设置了值。

```
hasIn(searchKeyPath: Iterable<any>): boolean
```

继承自

`Collection#hasIn`

###### 转换为集合

##### toMap()

将此集合转换为Map，如果键不可哈希，则抛弃。

```
toMap(): Map<number, T>
```

继承自

`Collection#toMap`

注意：这和`Map(this.toKeyedSeq())`等效，为了能够方便的进行链式调用而提供。

##### toOrderedMap()

将此集合转换为Map，保留索引的顺序。

```
toOrderedMap(): OrderedMap<number, T>
```

继承自

`Collection#toOrderedMap`

注意：这和`OrderedMap(this.toKeyedSeq())`等效，为了能够方便的进行链式调用而提供。

##### toSet()

将此集合转换为Set，如果值不可哈希，则抛弃。

```
toSet(): Set<T>
```

继承自

`Collection#toSet`

注意：这和`Set(this)`等效，为了能够方便的进行链式调用而提供。

##### toOrderSet()

将此集合转换为Set，保留索引的顺序。

```
toOrderedSet(): OrderedSet<T>
```

继承自

`Collection#toOrderedSet`

注意：这和`OrderedSet(this.valueSeq())`等效，为了能够方便的进行链式调用而提供。

##### toList()

将此集合转换为List，丢弃键值。

```
toList(): List<T>
```

继承自

`Collection#toList`

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

###### 迭代器

##### keys()

一个关于`Collection`键的迭代器。

```
keys(): IterableIterator<number>
```

继承自

`Collection#keys`

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`keySeq`来满足需求。

##### values()

一个关于`Collection`值的迭代器。

```
values(): IterableIterator<T>
```

继承自

`Collection#values`

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`valueSeq`来满足需求。

##### entries()

一个关于`Collection`条目的迭代器，是`[ key, value ]`这样的元组数据。

```
entries(): IterableIterator<[number, T]>
```

继承自

`Collection#entries`

注意：此方法将返回ES6规范的迭代器，并不支持Immutable.js的sequence算法，你可以尝试使用`entrySeq`来满足需求。


###### 集合（Seq）

##### keySeq()

返回一个新的Seq.Indexed，其包含这个集合的键值。

```
keySeq(): Seq.Indexed<number>
```

继承自

`Collection#keySeq`

##### valueSeq()

返回一个新的Seq.Indexed，其包含这个集合的所有值。

```
valueSeq(): Seq.Indexed<T>
```

继承自

`Collection#valueSeq`

##### entrySeq()

返回一个新的Seq.Indexed，其为[key, value]这样的元组。

```
entrySeq(): Seq.Indexed<[number, T]>
```

继承自

`Collection#entrySeq`

###### 副作用

##### forEach()

`sideEffect`将会对集合上每个元素执行。

```
forEach(
    sideEffect: (value: T, key: number, iter: this) => any,
    context?: any
): number
```

继承自 

`Collection#forEach`

与`Array#forEach`不同，任意一个`sideEffect`返回`false`都会停止循环。函数将返回所有参与循环的元素（包括最后一个返回false的那个）。

###### 创建子集

##### slice()

返回一个新的相同类型的相当于原集合指定范围的元素集合，包含开始索引但不包含结束索引位置的值。

```
slice(begin?: number, end?: number): this
```

继承自

`Collection#slice`

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

继承自

`Collection#skip`

##### skipLast()

返回一个不包含原集合从结尾开始`amount`个数元素的新的同类型集合。

```
skipLast(amount: number): this
```

继承自

`Collection#skipLast`

##### skipWhile()

返回一个原集合从`predicate`返回false那个元素开始的新的同类型集合。

```
skipWhile(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

继承自

`Collection#skipWhile`

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

继承自

`Collection#skipUntil`

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

继承自

`Collection#take`

##### takeLast()

返回一个包含从原集合结尾开始的`amount`个元素的新的同类型集合。

```
takeLast(amount: number): this
```

继承自

`Collection#take`

##### takeWhile()

返回一个包含原集合从头开始的`prediacte`返回true的那些元素的新的同类型集合。

```
takeWhile(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): this
```

继承自

`Collection#takeWhile`

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

继承自

`Collection#takeUntil`

例

```
const { List } = require('immutable')
List([ 'dog', 'frog', 'cat', 'hat', 'god' ])
  .takeUntil(x => x.match(/at/))
// List [ "dog", "frog" ]
```

###### 组合


##### flatten()

压平嵌套的集合。

```
flatten(depth?: number): Collection<any, any>
flatten(shallow?: boolean): Collection<any, any>
```

继承自

`Collection#flatten`

默认会深度地经常压平集合操作，返回一个同类型的集合。可以指定`depth`为压平深度或者是否深度压平（为true表示仅进行一层的浅层压平）。如果深度为0（或者shllow:false）将会深层压平。

压平仅会操作其他集合，数组和对象不会进行此操作。

注意：`flatten(true)`操作是在集合上进行，同时返回一个集合。


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

继承自

`Collection#reduce`

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

继承自

`Collection#reduceRight`

注意：与this.reverse().reduce()等效，为了与`Array#reduceRight`看齐而提供。

##### every()

当集合中所有元素`predicate`都判定为true时返回ture。

```
every(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): boolean
```

继承自

`Collection#every`

##### some()

当集合中任意元素`predicate`判定为true时返回ture。

```
some(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): boolean
```

继承自

`Collection#some`

##### join()

将值连接为字符串，并且在每两个值之间插入分割。默认分隔为`","`。

```
join(separator?: string): string
```

继承自

`Collection#join`

##### isEmpty()

当集合不包含值时返回true。

```
isEmpty(): boolean
```

继承自

`Collection#isEmpty`

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

继承自

`Collection#count`

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

继承自

`Collection#find`

##### findLast()

返回集合中最后一个符合与所提供的断言的值。

```
findLast(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): T | undefined
```

继承自

`Collection#findLast`

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

继承自

`Collection#findEntry`

##### findLastEntry()

返回最后一个符合所提供断言的值的[key， value]。

```
findLastEntry(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any,
    notSetValue?: T
): [number, T] | undefined
```

继承自

`Collection#findLastEntry`

注意：`predicate`将会逆序地在每个值上调用。

##### findKey()

返回第一个`predicate`返回为true的键。

```
findKey(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): number | undefined
```

继承自

`Collection#findKey`

##### findLastKey()

返回最后一个`predicate`返回为true的键。

```
findLastKey(
    predicate: (value: T, key: number, iter: this) => boolean,
    context?: any
): number | undefined
```

继承自

`Collection#findLastKey`

注意：`predicate`将会逆序地在每个值上调用。

##### keyOf()

返回与提供的搜索值关联的键，或者undefined。

```
keyOf(searchValue: T): number | undefined
```

继承自

`Collection#keyOf`

##### lastKeyOf()

返回最后一个与提供的搜索值关联的键或者undefined。

```
lastKeyOf(searchValue: T): number | undefined
```

继承自

`Collection#lastKeyOf`

##### max()

返回集合中最大的值。如果有多个值比较为相等，那么将返回第一个。

```
max(comparator?: (valueA: T, valueB: T) => number): T | undefined
```

继承自

`Collection#max`

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

继承自

`Collection#maxBy`

例

```
hitters.maxBy(hitter => hitter.avgHits)
```

##### min()

返回集合中最小的值，如果有多个值比较为相等，将会返回第一个。

```
min(comparator?: (valueA: T, valueB: T) => number): T | undefined
```

继承自

`Collection#min`

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

继承自

`Collection#minBy`

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

继承自

`Collection#isSubset`

##### isSuperset()

如果集合包含`iter`中所有元素则返回true。

```
isSuperset(iter: Iterable<T>): boolean
```

继承自

`Collection#isSuperset`