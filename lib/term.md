# 术语

标签（空格分隔）： ES6规范

---

## Symbol类型

新增的原始数据类型，表示唯一的非字符串属性名

```javascript
let s1 = Symbol()
let s2 = Symbol('foo')
let s3 = Symbol('foo')

s2 === s3 // false
```
