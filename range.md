## Range()

返回由Seq.Indexed指明的从start到end由step指定增量的数值（包含start，不包含end），默认值start为0，step为1，end为无穷大。当start与end相等时，返回一个空范围。

```
Range(start?: number, end?: number, step?: number): Seq.Indexed<number>
```

例

```
const { Range } = require('immutable')
Range() // [ 0, 1, 2, 3, ... ]
Range(10) // [ 10, 11, 12, 13, ... ]
Range(10, 15) // [ 10, 11, 12, 13, 14 ]
Range(10, 30, 5) // [ 10, 15, 20, 25 ]
Range(30, 10, 5) // [ 30, 25, 20, 15 ]
Range(30, 30, 5) // []
```