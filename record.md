## Record

建立一个继承自Record的新类型。它类似于JS的对象，但需要明确指定可设置的键及其对应的值。

例

```
const { Record } = require('immutable')
const ABRecord = Record({ a: 1, b: 2 })
const myRecord = new ABRecord({ b: 3 })
```

Record中定义的键一定有对应的值。从Record中`reomove`一个键仅仅是为他设置了一个默认值。

```
myRecord.size // 2
myRecord.get('a') // 1
myRecord.get('b') // 3
const myRecordWithoutB = myRecord.remove('b')
myRecordWithoutB.get('b') // 2
myRecordWithoutB.size // 2
```

提供给构造器的值如果在Record类型定义中未出现，那么他将会被忽略。如，在这个情况下，ABRecord提供了"x"这个键而只有"a"和"b"两个键被定义了，那么在这个Record中，"x"的值将会被忽略。

```
const myRecord = new ABRecord({ b: 3, x: 10 })
myRecord.get('x') // undefined
```

因为Record拥有已知的键值集，所以通过属性取值将会正常运行，但设置值将会抛出异常。

注意：IE8不支持按属性取值。如需支持IE8请只使用`get()`。

```
myRecord.b // 3
myRecord.b = 5 // throws Error
```

Record类可被正常地继承，允许在其上自定义方法。这在函数式环境下并不是常见的样例，但出现在很多JS程序中。

```
class ABRecord extends Record({ a: 1, b: 2 }) {
  getAB() {
    return this.a + this.b;
  }
}

var myRecord = new ABRecord({b: 3})
myRecord.getAB() // 4
```

###### 构造器

##### Record()

```
Record<T>(defaultValues: T, name?: string): Record.Class<T>
```

###### 静态方法

##### Record.isRecord()

如果提供的值为Record返回true。

```
Record.isRecord(maybeRecord: any): boolean
```

###### Record.getDescriptiveName()

Record允许通过第二个参数来定义其描述名称，当转换Record为字符串或者在错误信息中展示。任意Record的描述名称可以通过此方法来获取。未定义情况下将返回"Record"。

```
Record.getDescriptiveName(record: Instance<any>): string

```

例

```
const { Record } = require('immutable')
const Person = Record({
  name: null
}, 'Person')

var me = Person({ name: 'My Name' })
me.toString() // "Person { "name": "My Name" }"
Record.getDescriptiveName(me) // "Person"
```

###### 类型

`Record.Class`