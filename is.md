## is\(\)

和[`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)类似的相等比较方法，但其是比较两个[`Collection`](/collection.md)是否有相同的值。

```
is(first: any, second: any): boolean
```

用于比较两个不可变数据是否相等，包括[`Map`](/map.md)的键值对和[`Set`](/set.md)的成员。

```
import { Map, is } from 'immutable'
const map1 = Map({ a: 1, b: 1, c: 1 })
const map2 = Map({ a: 1, b: 1, c: 1 })
assert(map1 !== map2)
assert(Object.is(map1, map2) === false)
assert(is(map1, map2) === true)
```

is\(\)不仅仅能比较原始的字符串、数值和不可变集合比如[`Map`](/map.md)和[`Set`](/set.md)，也能比较实现了包含`equals()`和`hashCode()`两个方法的[`ValueObject`](/valueobject.md) 。

注意：和[`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)不同的是，`Immutable.is`假定`0`和`-0`是相同的，与ES6的Map键值相匹配。

