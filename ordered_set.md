## OrderedSet

一种能够保证迭代顺序为元素添加\([add](#)\)顺序的Set。

###### 构造器

##### OrderedSet()

构建一个新的不可变OrderedSet包含所提供类集合结构的值。

```
OrderedSet(): OrderedSet<any>
OrderedSet<T>(): OrderedSet<T>
OrderedSet<T>(collection: Iterable<T>): OrderedSet<T>
```

###### 静态方法

##### OrderedSet.isOrderedSet()

如果提供的值为OrderedSet则返回true。

```
OrderedSet.isOrderedSet(maybeOrderedSet: any): boolean
```

##### OrderedSet.of()

构建一个新的不可变OrderedSet包含所提供`values`。

```
OrderedSet.of<T>(...values: Array<T>): OrderedSet<T>
```

##### OrderedSet.fromKeys()

`OrderedSet.fromKeys()`使用集合或者JS对象的键创建一个新的不可变OrderedSet。

###### 成员

##### size

```
size
```