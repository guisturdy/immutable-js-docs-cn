## OrderedMap

一种能保证Map内元素的顺序与他们被set()的先后顺序一致的Map类型。

```
class OrderedMap<K, V> extends Map<K, V>
```

OrderedMap的迭代表现与ES6Map和JS对象一致。

注意`OrderedMap`将会比无序`Map`话费跟多的内存。`OrderedMap#set`复杂度大概为O(log32 N)，但不稳定。

