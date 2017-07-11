## Repeat()

返回右Seq.Indexed指明的重复times次数的value，当times未定义时，返回值为无穷Seq的value。

```
Repeat<T>(value: T, times?: number): Seq.Indexed<T>
```

```
const { Repeat } = require('immutable')
Repeat('foo') // [ 'foo', 'foo', 'foo', ... ]
Repeat('bar', 4) // [ 'bar', 'bar', 'bar', 'bar' ]
```