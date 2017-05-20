##### isIndexed\(\)

返回True表示这是Collection.isIndexed或其子类。

```
isIndexed(maybeIndexed: any): boolean
```

###### 示例

```
const { isIndexed, Map, List, Stack, Set } = require('immutable');
isIndexed([]); // false
isIndexed({}); // false
isIndexed(Map()); // false
isIndexed(List()); // true
isIndexed(Stack()); // true
isIndexed(Set()); // false
```



