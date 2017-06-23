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

