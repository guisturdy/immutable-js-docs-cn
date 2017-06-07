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

##### asImmutable\(\)

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