## List

List是类似于JS中数组的密集型有序集合。

```
class List<T> extends Collection.Indexed<T>
```

List是不可变的\(Immutable\)，无限长的，设置和读取的复杂度为O\(log32 N\)，入栈出栈\(push, pop\)复杂度为O\(1\)。

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



