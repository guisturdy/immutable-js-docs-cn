##### isKeyed\(\)

返回True表示这是Collection.key或其子类。

```
isKeyed(maybeKeyed: any): boolean
```

###### 示例

```
const { isKeyed, Map, List, Stack } = require('immutable');
isKeyed([]); // false
isKeyed({}); // false
isKeyed(Map()); // true
isKeyed(List()); // false
isKeyed(Stack()); // false
```



