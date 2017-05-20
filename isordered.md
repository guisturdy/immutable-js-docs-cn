##### isOrdered\(\)

返回True表示这是一个Collection同时迭代索引设置正确。Collection.indexed、OrderedMap和OrderedSet会返回True。

```
isOrdered(maybeOrdered: any): boolean
```

###### 示例

```
const { isOrdered, Map, OrderedMap, List, Set } = require('immutable');
isOrdered([]); // false
isOrdered({}); // false
isOrdered(Map()); // false
isOrdered(OrderedMap()); // true
isOrdered(List()); // true
isOrdered(Set()); // false
```



