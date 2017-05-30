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



