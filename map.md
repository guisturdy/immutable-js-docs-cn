## Map

不可变Map是无序的Collection.keyed的（key, value）键值对，具有`O(log32 N)`读取复杂度和`O(log32 N)`持久化复杂度。

```
class Map<K, V> extends Collection.Keyed<K, V>
```

Map的迭代顺序的不确定的，但是是稳定的。多次迭代同一个Map，迭代顺序将会相同。

Map的键可以为任意类型，使用`Immutable.is`来确定相等性。这将允许使用任意值（包括NaN）来作为键。

由于`Immutable.is`是确定对比值语义上的相等性，不可变集合也会被对待为值，所以任何不可变集合都可以作为键来使用。

```
const { Map, List } = require('immutable');
Map().set(List([ 1 ]), 'listofone').get(List([ 1 ]));
// 'listofone'
```

JS对象也可以作为值，但是将会使用严格地相等来对比键的相等性。所以两个看起来一样的对象将会是两个不同的键。

###### 构造器

##### Map()

创建一个新的不可变Map。

```
Map<K, V>(collection: Iterable<[K, V]>): Map<K, V>
Map<T>(collection: Iterable<Iterable<T>>): Map<T, T>
Map<V>(obj: {[key: string]: V}): Map<string, V>
Map<K, V>(): Map<K, V>
Map(): Map<any, any>
```

用提供的Collection.Keyed或者JS对象创建相同的键值集合或者期望的[K, V]元组集合。

```
const { Map } = require('immutable')
Map({ key: "value" })
Map([ [ "key", "value" ] ])
```

记住，当使用JS对象来创建不可变Map时，尽管不可变数组允许任意类型值作为键，即使使用无引号的缩写方式，JS对象的属性将会对待为字符串。

```
let obj = { 1: "one" }
Object.keys(obj) // [ "1" ]
obj["1"] // "one"
obj[1]   // "one"

let map = Map(obj)
map.get("1") // "one"
map.get(1)   // undefined
```

JS对象的属性键值将首先被装换为字符串，但由于不可变数组的键可以为任意类型，所以`get()`的参数将不会改变。

###### 静态方法

##### Map.isMap()

但提供的值为Map时返回true。

```
Map.isMap(maybeMap: any): boolean
```

例

```
const { Map } = require('immutable')
Map.isMap({}) // false
Map.isMap(Map()) // true
```

###### 成员

##### size

```
size
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