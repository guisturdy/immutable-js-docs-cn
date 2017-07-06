## Set

一种存取复杂度为`O(log32 N)`的无重复值的集合。

```
class Set<T> extends Collection.Set<T>
```

遍历一个Set它的值将会以(value, value)这种形式便利。便利Set的顺序是不确定但稳定地。同一个Set进行多次遍历，他们遍历的顺序将会是一样的。

Set的值和Map的键一样，可以是任意类型的，包括其他不可变集合，自定义类型及NaN。使用`Immutable.is`来确定相等性，以保证Set中值的唯一性。

###### 构造器

##### Set()

构建一个包含提供的集合的值得新的不可变Set。

```
Set(): Set<any>
Set<T>(): Set<T>
Set<T>(collection: Iterable<T>): Set<T>
```

###### 静态方法

##### Set.isSet()

如果提供的值为Set返回true。

```
Set.isSet(maybeSet: any): boolean
```

##### Set.of()

返回新的包含提供值的Set。

```
Set.of<T>(...values: Array<T>): Set<T>
```

##### Set.fromKeys()


`Set.fromKeys()`使用集合或者JS对象的键创建一个新的不可变Set。

```
Set.fromKeys<T>(iter: Collection<T, any>): Set<T>
Set.fromKeys(obj: {[key: string]: any}): Set<string>
```

##### Set.intersect()

`Set.intersect()`使用与另一个Set共有值创建新的不可变Set。

```
Set.intersect<T>(sets: Iterable<Iterable<T>>): Set<T>
```

例

```
const { Set } = require('immutable')
const intersected = Set.intersect([
  Set([ 'a', 'b', 'c' ])
  Set([ 'c', 'a', 't' ])
])
// Set [ "a", "c"" ]
```

##### Set.union()

`Set.union()`将两个Set结合返回一个新的不可变Set。

```
Set.union<T>(sets: Iterable<Iterable<T>>): Set<T>
```

例

```
* const { Set } = require('immutable')
const unioned = Set.union([
  Set([ 'a', 'b', 'c' ])
  Set([ 'c', 'a', 't' ])
])
// Set [ "a", "b", "c", "t"" ]
```

###### 成员

##### size

```
size
```