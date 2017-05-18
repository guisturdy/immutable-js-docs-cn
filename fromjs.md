## fromJS\(\)

完全地将一个JS对象转或数组转换为不可变的Maps或Lists。

```
fromJS(
    jsValue: any,
    reviver?: (
        key: string | number,
        sequence: Collection.Keyed<string, any> | Collection.Indexed<any>,
        path?: Array<string | number>
    ) => any
): any
```

reviver是可选参数，它将在每个集合（collection）上调用，接受Seq（从最深层到顶层）和当前集合的key，父级JS对象将作为`this`被提供。在最顶层的对象的key为`""` 。reviver将会返回一个新的不可变集合，同时允许自定义转换一个深层次的JS对象。path将会提供从开始到当前值的Key系列。

以下是一个由原始JS数据转换为List和OrderedMap：

```
const { fromJS, isIndexed } = require('immutable')
fromJS({ a: {b: [10, 20, 30]}, c: 40}, function (key, value, path) {
  console.log(key, value, path)
  return isIndexed(value) ? value.toList() : value.toOrderedMap()
})
```

```
> "b", [ 10, 20, 30 ], [ "a", "b" ]
> "a", { b: [10, 20, 30] }, c: 40 }, [ "a" ]
> "", {a: {b: [10, 20, 30]}, c: 40}, []
```

如果reviver未提供，那么默认地，Array会转换为List，Object会转换为Map。

事实上，reviver和[`JSON.parse`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse#Example.3A_Using_the_reviver_parameter)参数一致。

`fromJS`是保守的转换，它只会将能被[`Array.isArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)认证的数组转换为List，只会转换原始的对象（不包含自定义原型）为Map。

注意，当一个JS对象转换为不可变Map时，尽管不可变Map可接受任意类型键值，JS对象的索引永远为字符串，即使你使用简写。

```
let obj = { 1: "one" };
Object.keys(obj); // [ "1" ]
obj["1"]; // "one"
obj[1];   // "one"

let map = Map(obj);
map.get("1"); // "one"
map.get(1);   // undefined
```

当使用任意类型去查询JS对象的索引它将会隐式转换为字符串，但不可变Map使用`get()`去索引时，它的参数将不会被转换。

```
"Using the reviver parameter"
```



