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

注意：`delete`不可在`withMutations`中使用。

##### insert\(\)

返回一个`index`处值为`value`总长度加一的新List。并且大于原`index`的索引都会加一。

```
insert(index: number, value: T): List<T>
```

此方法与`list.splice(index, 0, value)`同义。

注意：`insert`不可在`withMutations`中使用。



