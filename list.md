## List

List是类似于JS中数组的密集型有序集合。

```
class List<T> extends Collection.Indexed<T>
```

List是不可变的\(Immutable\)，修改和读取数据的复杂度为O\(log32 N\)，入栈出栈\(push, pop\)复杂度为O\(1\)。

List实现了队列功能，能高效的在队首\(unshift, shift\)或者队尾\(push, pop\)进行元素的添加和删除。

与JS的数组不同，在List中一个未设置的索引值和设置为undefined的索引值是相同的。List\#forEach会从0到size便利所有元素，无论他是否有明确定义。

###### 构造函数

##### List\(\)

新建一个包含传入元素的不可变List。

```
List(): List<any>
List<T>(): List<T>
List<T>(collection: Iterable<T>): List<T>
```

例

```
const { List, Set } = require('immutable')

const emptyList = List()
// List []

const plainArray = [ 1, 2, 3, 4 ]
const listFromPlainArray = List(plainArray)
// List [ 1, 2, 3, 4 ]

const plainSet = Set([ 1, 2, 3, 4 ])
const listFromPlainSet = List(plainSet)
// List [ 1, 2, 3, 4 ]

const arrayIterator = plainArray[Symbol.iterator]()
const listFromCollectionArray = List(arrayIterator)
// List [ 1, 2, 3, 4 ]

listFromPlainArray.equals(listFromCollectionArray) // true
listFromPlainSet.equals(listFromCollectionArray) // true
listFromPlainSet.equals(listFromPlainArray) // true
```

###### 静态方法

##### List.isList\(\)

判断传入参数是否为List。

```
List.isList(maybeList: any): boolean
```

例

```
List.isList([]); // false
List.isList(List()); // true
```

##### List.of\(\)

使用传入值新建一个List。

```
List.of<T>(...values: Array<T>): List<T>
```

例

```
List.of(1, 2, 3, 4)
// List [ 1, 2, 3, 4 ]
```

注意：所有值不会被改变。

```
List.of({x:1}, 2, [3], 4)
// List [ { x: 1 }, 2, [ 3 ], 4 ]
```

###### 成员变量

##### size

```
size
```

###### 持久化修改

##### set\(\)

返回一个在`index`位置处值为`value`的新List。如果`index`位置已经定义了值，它将会被替换。

```
set(index: number, value: T): List<T>
```

当`index`为负值时，将从List尾部开始索引。`v.set(-1, "value")`设置了List最后一个索引位置的值。

当`index`比原List的`size`大时，返回的新List的`size`将会足够大以包含`index`。

```
const originalList = List([ 0 ]);
// List [ 0 ]
originalList.set(1, 1);
// List [ 0, 1 ]
originalList.set(0, 'overwritten');
// List [ "overwritten" ]
originalList.set(2, 2);
// List [ 0, undefined, 2 ]

List().set(50000, 'value').size;
// 50001
```

注意：`set`可以在`withMutations`中使用。

##### delete\(\)

返回一个不包含原`index`值总长度减一的新List。并且大于原`index`的索引都会减一。

```
delete(index: number): List<T>
```

###### 别名

`remove()`

此方法与`list.splice(index, 1)`是同义的。

`index`可以为负值，表示从末尾开始计算索引。`v.delete(-1)`将会删除List最后一个元素。

注意：`delete`在IE8上使用是不安全的。

```
List([ 0, 1, 2, 3, 4 ]).delete(0);
// List [ 1, 2, 3, 4 ]
```

注意：`delete`_不可_在`withMutations`中使用。

##### insert\(\)

返回一个`index`处值为`value`总长度加一的新List。并且大于原`index`的索引都会加一。

```
insert(index: number, value: T): List<T>
```

此方法与`list.splice(index, 0, value)`同义。

注意：`insert`_不可_在`withMutations`中使用。

##### clear\(\)

返回一个新的长度为0的空List。

```
clear(): List<T>
```

例

```
List([ 1, 2, 3, 4 ]).clear()
// List []
```

注意：`clear`可以在`withMutations`中使用。

##### push\(\)

返回一个新的List为旧List末尾添加一个元素值为所传入`value`。

```
push(...values: Array<T>): List<T>
```

例

```
List([ 1, 2, 3, 4 ]).push(5)
// List [ 1, 2, 3, 4, 5 ]
```

注意：`push`可以在`withMutations`中使用。

##### pop\(\)

返回一个新的List为旧List移除最后一个元素。

```
pop(): List<T>
```

注意：这与[`Array#pop`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)不同，它返回的是新List而不是被移除元素。使用`last()`方法来获取List最后一个元素。

```
List([ 1, 2, 3, 4 ]).pop()
// List[ 1, 2, 3 ]
```

注意：`pop`可以在`withMutations`中使用。

##### unshift\(\)

返回一个新的List为旧List头部插入所提供的`values`。

```
unshift(...values: Array<T>): List<T>
```

例

```
List([ 2, 3, 4]).unshift(1);
// List [ 1, 2, 3, 4 ]
```

注意：`unshift`可以在`withMutations`中使用。

##### shift\(\)

返回一个新的List为旧List移除第一个元素，并且其他元素索引减一。

```
shift(): List<T>
```

注意：这与[`Array#shift`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)不同，它返回的是新List而不是被移除元素。使用`first()`方法来获取List最后一个元素。

```
List([ 0, 1, 2, 3, 4 ]).shift();
// List [ 1, 2, 3, 4 ]
```

注意：`shift`可以在`withMutations`中使用。

##### update\(\)

将会返回一个新List，如果指定`index`位置在原List中存在，那么由所提供函数`updater`更新此位置值，否则该位置值设为所提供的`notSetValue`值。 如果只传入了一个参数`updater`，那么`updater`接受的参数为List本身。

```
update(index: number, notSetValue: T, updater: (value: T) => T): this
update(index: number, updater: (value: T) => T): this
update<R>(updater: (value: this) => R): R
```

**重载**

`Collection#update`

**见**

`Map#update`

`index`可以为负值，表示从尾部开始索引。`v.update(-1)`表示更新最后一个元素。

```
const list = List([ 'a', 'b', 'c' ])
const result = list.update(2, val => val.toUpperCase())
// List [ "a", "b", "C" ]
```

此方法可以很方便地链式调用一系列普通的方法。RxJS称这为"let"，lodash叫做"thru"。

例，在调用map和filter之后计算List的和：

```
function sum(collection) {
  return collection.reduce((sum, x) => sum + x, 0)
}

List([ 1, 2, 3 ])
  .map(x => x + 1)
  .filter(x => x % 2 === 0)
  .update(sum)
// 6
```

注意：`update(index)`可以在`withMutations`中使用。

##### merge\(\)

注意：`merge`可以在`withMutations`中使用。

```
merge(...collections: Array<Collection.Indexed<T> | Array<T>>): this
```

见

`Map#merge`

##### mergeWith\(\)

注意：`mergeWith`可以在`withMutations`中使用。

```
mergeWith(
merger: (oldVal: T, newVal: T, key: number) => T,
...collections: Array<Collection.Indexed<T> | Array<T>>
): this
```

见

`Map#mergeWith`

##### mergeDeep\(\)

注意：`mergeDeep`可以在`withMutations`中使用。

```
mergeDeep(...collections: Array<Collection.Indexed<T> | Array<T>>): this
```

见

`Map#mergeDeep`

##### mergeDeepWith\(\)

注意：`mergeDeepWith`可以在`withMutations`中使用。

```
mergeDeepWith(
merger: (oldVal: T, newVal: T, key: number) => T,
...collections: Array<Collection.Indexed<T> | Array<T>>
): this
```

见

`Map#mergeDeepWith`

##### setSize\(\)

返回一个新的List，长度为指定`size`。如果原长度大于`size`，那么新的list将不包含超`size`的值。如果原长度小于`size`，那么超过部分的值为undefind。

```
setSize(size: number): List<T>
```

在新构建一个LIst时，当已知最终长度，`setSize`可能会和`withMutations`共同使用以提高性能。

###### 深度持久化

##### setIn\(\)

返回一个新的在`ksyPath`指定位置了值`value`的List。如果在`keyPath`位置没有值存在，这个位置将会被构建一个新的不可变Map。

```
setIn(keyPath: Iterable<any>, value: any): this
```

数值索引将被用于描述在List中的路径位置。

```
const { List } = require('immutable');
const list = List([ 0, 1, 2, List([ 3, 4 ])])
list.setIn([3, 0], 999);
// List [ 0, 1, 2, List [ 999, 4 ] ]
```

注意：`setIn`可以在`withMutations`中使用。

##### deleteIn\(\)

返回一个删除了由`keyPath`指定位置值的新List。如果指定位置无值，那么不会发生改变。

```
deleteIn(keyPath: Iterable<any>): this
```

别名

`removeIn()`

例

```
const { List } = require('immutable');
const list = List([ 0, 1, 2, List([ 3, 4 ])])
list.deleteIn([3, 0]);
// List [ 0, 1, 2, List [ 4 ] ]
```

注意：`removeIn`_不可_在`withMutations`中使用。

##### updateIn\(\)

注意：`updateIn`可以在`withMutations`中使用。

```
updateIn(
keyPath: Iterable<any>,
notSetValue: any,
updater: (value: any) => any
): this
updateIn(keyPath: Iterable<any>, updater: (value: any) => any): this
```

见

`Map#updateIn`

##### mergIn\(\)

注意：`mergIn`_不可_在`withMutations`中使用。

```
mergeIn(keyPath: Iterable<any>, ...collections: Array<any>): this
```

见

`Map#mergIn`

##### mergeDeepIn\(\)

注意：`mergeDeepIn`可以在`withMutations`中使用。

```
mergeDeepIn(keyPath: Iterable<any>, ...collections: Array<any>): this
```

见

`Map#mergeDeepIn`

###### 暂时改变

##### withMutaitions\(\)

注意：只有部分方法可以被可变的集合调用或者在`withMutations`中调用！查看文档中各个方法看他是否允许在`withMuataions`中调用。

```
withMutations(mutator: (mutable: this) => any): this
```

见

`Map#withMutations`

##### asMutable\(\)

withMutations\(\)的替代API。

```
asMutable(): this
```

见

`Map#asMutable`

注意：只有部分方法可以被可变的集合调用或者在`withMutations`中调用！查看文档中各个方法看他是否允许在`withMuataions`中调用。

```
withMutations(mutator: (mutable: this) => any): this
```

##### asImmutable\(\)

```
asImmutable(): this
```

见

`Map#asImmutable`

###### 序列算法

##### cancat\(\)

将其他的值或者集合与这个List串联起来返回为一个新List。

```
concat<C>(...valuesOrCollections: Array<Iterable<C> | C>): List<T | C>
```

覆盖

`Collection#concat`

##### map\(\)

返回一个由传入的`mapper`函数处理过值的新List。

```
map<M>(mapper: (value: T, key: number, iter: this) => M, context?: any): List<M>
```

覆盖

`Collection#map`

例

```
List([ 1, 2 ]).map(x => 10 * x)
// List [ 10, 20 ]
```

注意：`map()`总是返回一个新的实例，即使它产出的每一个值都与原始值相同。

##### flatMap\(\)

扁平化这个List为一个新List。

```
flatMap<M>(
mapper: (value: T, key: number, iter: this) => Iterable<M>,
context?: any
): List<M>
```

覆盖

`Collection#flatMap`

与`list.map(...).flatten(true)`相似。

##### filter\(\)

返回一个只有由传入方法`predicate`返回为true的值组成的新LIst。

```
filter<F>(
predicate: (value: T, index: number, iter: this) => boolean,
context?: any
): List<F>
filter(
predicate: (value: T, index: number, iter: this) => any,
context?: any
): this
```

覆盖

`Collection#filter`

注意：`filter()`总是返回一个新的实例，即使它的结果没有过滤掉任何一个值。

##### zip\(\)

将List与所提供集合拉链咬合\(zipped\)。

```
zip(...collections: Array<Collection<any, any>>): List<any>
```

覆盖

`Collection.Index#zip`

与`zipWIth`类似，但这个使用默认的zipper建立[数组](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。

```
const a = List([ 1, 2, 3 ]);
const b = List([ 4, 5, 6 ]);
const c = a.zip(b); // List [ [ 1, 4 ], [ 2, 5 ], [ 3, 6 ] ]
```

##### [Symbol.iterator]

```
[Symbol.iterator](): IterableIterator<T>
```

继承自

`Collection.Indexed#[Symbol.iterator]`

##### filterNot\(\)

返回一个由所提供的`predicate`方法返回false过滤的新的相同类型的集合。

```
filterNot(
predicate: (value: T, key: number, iter: this) => boolean,
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

注意：`filterNot`总是返回一个新的实例，即使它没有过滤掉任何一个值。

##### reverse()

返回为一个逆序的新的List。

```
reverse(): this
```

继承自

`Collection#reverse`

##### sort()

返回一个使用传入的`comparator`重新排序的新List。

```
sort(comparator?: (valueA: T, valueB: T) => number): this
```

继承自

`Collection#sort`

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

###### 组合

##### interpose()

返回一个在原集合每两个元素之间插入提供的`separator`的同类型集合。

```
interpose(separator: T): this
```

继承自

`COllection.Indexed#interpose`

##### interleave()

返回一个原集合与所提供`collections`交叉的痛类型集合。

```
interleave(...collections: Array<Collection<any, T>>): this
```

继承自

`Collection.Indexed#interleave`

返回的集合依次包含第一个集合元素与第二个集合元素。

```
const { List } = require('immutable')
List([ 1, 2, 3 ]).interleave(List([ 'A', 'B', 'C' ]))
// List [ 1, "A", 2, "B", 3, "C"" ]
```

由最短的集合结束交叉。

```
List([ 1, 2, 3 ]).interleave(
  List([ 'A', 'B' ]),
  List([ 'X', 'Y', 'Z' ])
)
// List [ 1, "A", "X", 2, "B", "Y"" ]
```

##### splice()

返回一个由指定值替换了原集合某个范围的值的新的有序集合。如果没提供替换的值，那么会跳过要删除的范围。

```
splice(index: number, removeNum: number, ...values: Array<T>): this
```

继承自

`Collection.Indexed#splice`

`index`可以为负值，表示从集合结尾开始索引。`s.splice(-2)`表示倒数第二个元素开始拼接。

```
const { List } = require('immutable')
List([ 'a', 'b', 'c', 'd' ]).splice(1, 2, 'q', 'r', 's')
// List [ "a", "q", "r", "s", "d" ]
```

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

###### 查找

##### indexOf()

返回集合中第一个与所提供的搜索值匹配的索引，无匹配值则返回-1。

```
indexOf(searchValue: T): number
```

继承自

`Collection.Indexed#indexOf`

##### lastIndexOf()

返回集合中最后一个与所提供的搜索值匹配的索引，无匹配值则返回-1。

```
lastIndexOf(searchValue: T): number
```

继承自

`Collection.Indexed#lastIndexOf`

##### findIndex()

返回集合中第一个符合与所提供的断言的索引，均不符合则返回-1。

```
findIndex(
    predicate: (value: T, index: number, iter: this) => boolean,
    context?: any
): number
```

继承自

`Collection.Indexed#findIndex`

##### findLastIndex()

返回集合中最后一个符合与所提供的断言的索引，均不符合则返回-1。

```
findLastIndex(
    predicate: (value: T, index: number, iter: this) => boolean,
    context?: any
): number
```

继承自

`Collection.Indexed#findLastIndex`

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