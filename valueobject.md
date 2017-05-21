## ValueObject

```
class ValueObject
```

###### 成员函数

##### equals\(\)

当与传入的集合值相等时返回True，相等比较与[`Immutable.is()`](/is.md)的定义一样。

```
equals(other: any): boolean
```

注意：此方法与`Immutable.is(this,other)`等效。



##### hashCode\(\)

返回当前集合的哈希计算值。

```
hashCode(): number
```

在添加一个元素到Set中或者用key索引Map时，`hashCode`会被用于查明两个集合潜在的相等关系，即使他们没用相同的地址。

```
const a = List([ 1, 2, 3 ]);
const b = List([ 1, 2, 3 ]);
assert(a !== b); // 不是相同地址
const set = Set([ a ]);
assert(set.has(b) === true);
```

当两个值的`hashCode`相同，并[不能完全保证他们是相等的](https://zh.wikipedia.org/wiki/碰撞_%28计算机科学%29)，但当他们的`hashCode`不同，他们一定是不等的。

