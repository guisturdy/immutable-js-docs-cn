# isCollection\(\)

返回True表示这是一个集合（Collection）或集合的子类。

```
isCollection(maybeCollection: any): boolean
```

###### 示例

```
const { isCollection, Map, List, Stack } = require('immutable');
isCollection([]); // false
isCollection({}); // false
isCollection(Map()); // true
isCollection(List()); // true
isCollection(Stack()); // true
```



