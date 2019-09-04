# Symbol类型

标签（空格分隔）： ES6规范

---

非字符串的属性名

* 唯一、不可更改
* [[Description]]属性为该Symbol的描述

## 内置Symbol

| 名称 | [[Description]] | 说明 |
| --- | --- | --- |
| @@toStringTag | Symbol.toStringTag | 对象的类型，由Object.prototype.toString()方法访问 |
| @@toPrimitive | Symbol.toPrimitive | 对象方法，将对象转换为原始值类型，由ToPrimitive()操作调用 |
| @@iterator | Symbol.iterator | 对象方法，返回默认的迭代器，由for-of语句调用 |
| @@asyncIterator | Symbol.asyncIterator | 对象方法，返回默认的异步迭代器，由for-await-of语句调用 |
| @@unscopables | Symbol.unscopables | 指定不能被with语句访问的属性 |
| @@isConcatSpreadable | Symbol.isConcatSpreadable | 被Array.prototype.concat()方法拼接时是否要展开 |
| @@hasInstance | Symbol.hasInstance | 构造器方法，判断是否是构造器实例，由instanceof操作符调用 |
| @@species | Symbol.species | 衍生对象的构造函数 |
| @@search | Symbol.search | 正则搜索方法，由String.prototype.search()方法调用 |
| @@match | Symbol.match | 正则匹配方法，由String.prototype.match()方法调用 |
| @@replace | Symbol.replace | 正则替换方法，由String.prototype.replace()方法调用 |
| @@split | Symbol.split | 正则分隔方法，由String.prototype.split()方法调用 |
