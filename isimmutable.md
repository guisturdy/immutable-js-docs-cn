## isImmutable\(\)

返回True表示这是一个不可变数据（Collection或Record）。

```
isImmutable(maybeImmutable: any): boolean
```

###### 示例

```
const { isImmutable, Map, List, Stack } = require('immutable');
isImmutable([]); // false
isImmutable({}); // false
isImmutable(Map()); // true
isImmutable(List()); // true
isImmutable(Stack()); // true
isImmutable(Map().asMutable()); // false
```



