不可变数据让纯函数更强大，也让程序开发愈发简单明了，同时使类似于惰性求值的函数式编程成为可能。

为了在javascript上实现这些强大的概念，我们给javascript引擎带来了熟悉的面向对象的API以及Array、Map和Set上一的些镜像方法，让它能简单和高效地与普通javascript对象互相转换。

### 阅前注意

为了更好地介绍Immutable.js多样的API和返回值，本文档将基于静态类型的JS（如[Flow](https://flowtype.org/)、[TypeScript](http://www.typescriptlang.org/)）。你_不必使用这些_类型检查工具来使用Immutable.js，然而使用与它们相似的语法能让你更透彻地理解这些API。

###### 一些例子以及阅读指南

方法上所有的参数声明了它能接受和返回何种类型数据。以下例子表示了一个接受两个数值型参数返回值类型为数值型的方法：

```
sum(first: number, second: number): number
```

某些情况下，参数需要接受多种类型或者方法能返回多种类型数据，这种参数类型被称作_泛型_。例如，一个方法总是返回与他接受的参数同种类型的数据：

```
identity<T>(value: T): T
```

泛型也能被定义在类及其变量上。一个为你保存变量的类应该会是这样：

```
class Box<T> {
  constructor(value: T)
  getValue(): T
}
```

为了修改不可变数据，方法将会返回一个新的同类型集合。`this`这个类型表明返回值类型将参照类的类型。例如，当你在一个List上push一个数据时，他将会返回一个新的同类型List：

```
class List<T> {
  push(value: T): this
}
```

在Immutable.js中，一些方法实现了[迭代](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)协议，`Iterable<string>`表示一系列有序的string。在JS中我们一般使用数组（`[]`）来表示一个可迭代类型，在Immutable.js中集合都是可迭代的。

例如要在数据结构中获得深入的值，我们可以使用`getIn`他需要一个Iterable路径：

```
getIn(path: Iterable<string | number>): any
```

使用这个方法需要通过一个数组：`data.getIn(["key", 2])`。

注意：所有实力都是使用JavaScript的[ES6](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla)规范，在老旧的浏览器上运行可能需要转换为ES3。

例：

```
// ES2015
const mappedFoo = foo.map(x => x * x);
// ES3
var mappedFoo = foo.map(function (x) { return x * x; });
```

### API

##### [fromJS\(\)](/fromjs.md)

完全地将一个JS对象转或数组转换为不可变的Maps或Lists。

##### [is\(\)](/is.md)

和[`Object.is`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is)类似的相等比较方法，比较两个[`Collection`](#collection)是否有相同的值。

##### [hash\(\)](#hash)

hash\(\)方法是Immutable确认两个值是否相等和决定这些值如何存储的重要依据。传入任何数据，它将返回一个31位的整形。

##### [isImmutable\(\)](/isimmutable.md)

返回True表示这是一个不可变数据（Collection或Record）。

##### [isCollection\(\)](/iscollection.md)

返回True表示这是一个集合（Collection）或集合的子类。

##### [isKeyed\(\)](/iskeyed.md)

返回True表示这是Collection.key或其子类。

##### [isIndexed\(\)](/isindexed.md)

返回True表示这是Collection.isIndexed或其子类。

##### [isAssociative\(\)](/isassociative.md)

返回True表示这是Keyed或者Indexed Collection。

##### [isOrdered\(\)](/isordered.md)

返回True表示这是一个Collection同时迭代索引设置正确。Collection.indexed、OrderedMap和OrderedSet会返回True。

##### [isValueObject\(\)](/isvalueobject.md)

返回True表示这是个JS对象并且同时拥有`equals()`和`hashCode()`方法。

#### [ValueObject](/valueobject.md)

##### [List](/list.md)

List是有序的密集型集合，类似于JS的数组（Array）。

##### [Map](/map.md)

不可变Map是无序的可持久化的Collection.Keyed\(key, value\)键值对，存取复杂度为`O(log32 N)`。

##### [OrderedMap](/ordered_map.md)

一种能够保证迭代顺序为元素进入顺序的Map。

##### [Set](/set.md)

一种存取复杂度为`O(log32 N)`的无重复值的集合。

##### [OrderedSet](/ordered_set.md)

一种能够保证迭代顺序为元素添加\([add](#)\)顺序的Set。

##### [Stack](/stack.md)

Stack（栈）是一种支持复杂度为`O(1)`的高效添加和删除数据的集合，在栈顶添加和删除数据使用`unshift(v)`和`shift()`。

##### [Range\(\)](/range.md)

返回由Seq.Indexed指明的从start到end由step指定增量的数值（包含start，不包含end），默认值start为0，step为1，end为无穷大。当start与end相等时，返回一个空范围。

##### [Repeat\(\)](/repeat.md)

返回右Seq.Indexed指明的重复times次数的value，当times未定义时，返回值为无穷Seq的value。

##### [Record](/record.md)

建立一个继承自Record的新类型。它类似于JS的对象，但需要明确指定可设置的键及其对应的值。

##### [Seq](/seq.md)

相当于一系列值，但不能具体表现为某种数据结构。

##### [Collection](/collection.md)

Collection是一组可迭代的键值对集合，它也是所有immutable的基类，确保它们能使用集合的方法（如map，filter）。

