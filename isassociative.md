##### isAssociative\(\)

返回True表示这是Keyed或者Indexed Collection。

```
isAssociative(maybeAssociative: any): boolean
```

###### 示例

```
const { isAssociative, Map, List, Stack, Set } = require('immutable');
isAssociative([]); // false
isAssociative({}); // false
isAssociative(Map()); // true
isAssociative(List()); // true
isAssociative(Stack()); // true
isAssociative(Set()); // false
```



