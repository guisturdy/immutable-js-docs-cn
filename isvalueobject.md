##### isValueObject\(\)

返回True表示这是个JS对象并且同时拥有`equals()`和`hashCode()`方法。

```
isValueObject(maybeValue: any): boolean
```

任意两个_值对象（value object）_都可以使用[`Immutable.is()`](/is.md)来比较值是否相等或用于[`Map`](/map.md)的键和[`Set`](/set.md)的成员。



