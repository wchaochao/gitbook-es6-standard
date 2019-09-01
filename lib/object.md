# 对象

标签（空格分隔）： ES6规范

---

属性的集合

* `ordinary object`: 普通对象，有通用的属性和方法
* `exotic object`: 异常对象，某些属性和方法与普通对象表现不同

## 通用内部属性

所有对象都有的内部属性

| 内部属性 | 类型 | 描述 |
| --- | --- | --- |
| [[Prototype]] | Object &#124; Null | 对象的原型<br/>原型的数据属性会被继承，用于[[Get]]<br/>原型的访问器属性会被继承，用于[[Get]]和[[Set]] |
| [[Extensible]] | Boolean | 对象是否可扩展，为false时不可添加属性、不可修改[[Class]]、[[Prototype]] |
| [[GetPrototypeOf]] | () => Object &#124; Null  | 获取对象的原型 |
| [[SetPrototypeOf]] | (prototype: Object &#124; Null) => Boolean | 设置对象的原型 |
| [[IsExtensible]] | () => Boolean | 对象是否可扩展 |
| [[PreventExtensions]] | () => Boolean | 阻止对象扩展 |
| [[GetOwnProperty]] | (propertyKey: String &#124; Symbol) => Property Descriptor &#124; Undefined | 获取对象自身属性的属性描述符 |
| [[DefineOwnProperty]] | (propertyKey: String &#124; Symbol, propertyDescriptor: Property Descriptor) => Boolean | 设置对象自身属性的属性描述符 |
| [[HasProperty]] | (propertyKey: String &#124; Symbol) => Boolean | 是否是对象自身属性或继承属性 |
| [[Get]] | (propertyKey: String &#124; Symbol, receiver: any) => any | 获取对象自身属性或继承属性的属性值，receiver为this值 |
| [[Set]] | (propertyKey: String &#124; Symbol, value: any, receiver: any) | 设置对象属性，receiver为this值 |
| [[Delete]] | (propertyKey: String &#124; Symbol) => Boolean | 删除对象自身属性 |
| [[OwnPropertyKeys]] | () => List{String &#124; Symbol} | 获取对象自身属性名的列表 |

### [[GetPrototypeOf]] ( )

获取对象的原型

```javascript
/**
 * 返回O的[[Prototype]]属性
 */
function OrdinaryGetPrototypeOf (O) {
  return O.[[Prototype]]
}

return OrdinaryGetPrototypeOf(O)
```

示例

```javascript
let o = {}
o.[[GetPrototypeOf]]() // Object.prototype
```

### [[SetPrototypeOf]] ( V )

设置对象的原型

```javascript
/**
 * V应为Object或Null类型
 * V与O的原型相同时，直接返回true
 * O不可扩展时，直接返回false
 * O与V构成循环原型链时且原型链的[[GetPrototypeOf]]方法都为[[GetPrototypeOf]]方法时，直接返回false
 * 其他情况设置O的原型为V，返回true
 */
function OrdinarySetPrototypeOf (O, V) {
  AssertType(V, Object | Null)

  let current = O.[[Prototype]]
  if (SameValue(current, V)) {
    return true
  }

  let extensible = O.[[Extensible]]
  if (extensible === false) {
    return false
  }

  let p = V
  while (true) {
    if (p === null) {
      break
    } else if (SameValue(p, O)) {
      return false
    } else {
      if (SameValue(p.[[GetPrototypeOf]], [[GetPrototypeOf]])) {
        p = p.[[Prototype]]
      } else {
        break
      }
    }
  }

  O.[[Prototype]] = V
  return true
}

return OrdinarySetPrototypeOf(O, V)
```

示例

```javascript
let o1 = {}
o1.[[SetPrototypeOf]](1) // 抛出TypeError
o1.[[SetPrototypeOf]](o1) // true，原型不变

let o2 = {}
o2.[[PreventExtensions]]()
o2.[[SetPrototypeOf]]({}) // false，原型不变

let o3 = {}
o1.[[SetPrototypeOf]](o3) // true，原型为o3
o3.[[SetPrototypeOf]](o1) // false，原型不变
```

### [[IsExtensible]] ( )

```javascript
/**
 * 返回O的[[Extensible]]属性
 */
function OridinaryIsExtensible (O) {
  return O.[[Extensible]]
}

return OrdinaryIsExtensible(O)
```

示例

```javascript
let o = {}
o.[[IsExtensible]]() // true

o.[[PreventExtensions]]()
o.[[IsExtensible]]() // false
```

### [[PreventExtensions]] ( )

```javascript
/**
 * 设置O的[[Extensible]]属性为false，返回true
 */
function OrdinaryPreventExtensions (O) {
  O.[[Extensible]] = false
  return true
}

return OrdinaryPreventExtensions(O)
```

示例

```javascript
let o = {}

o.[[PreventExtensions]]() // true
o.[[IsExtensible]]() // false
o.[[SetPrototypeOf]]({}) // fasle
```

### [[GetOwnProperty]] ( P )

获取对象自身属性的属性描述符

```javascript
/**
 * P应为属性名
 * P不是O的自身属性，返回undefined
 * P是O的自身属性，返回相应的数据属性描述符或访问器属性描述符
 */
function OrdinaryGetOwnProperty (O, P) {
  AssertValue(IsPropertyKey(P), true)

  if (!HasOwnProperty(O, P)) {
    return undefined
  }

  let D = new Property Descriptor()
  let X = GetOwnPropertyDescriptor(O, P)
  if (HasOwnProperty(X, 'value')) {
    D.[[Value]] = X.value
    D.[[Writable]] = X.writable
  } else {
    D.[[Get]] = X.get
    D.[[Set]] = X.set
  }
  D.[[Enumerable]] = X.enumerable
  D.[[Configurable]] = X.configurable

  return D
}

return OrdinaryGetOwnProperty(O, P)
```

示例

```javascript
let o = {
  a: 1,
  get p () { return 2 }
}

// 非自身属性
o.[[GetOwnProperty]]('b') // undefined

// 数据属性
o.[[GetOwnProperty]]('a')
Property Descriptor {
  [[Value]]: 1,
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
}

// 访问器属性
o.[[GetOwnProperty]]('p')
Property Descriptor {
  [[Get]]: getter,
  [[Set]]: undefined,
  [[Enumerable]]: true,
  [[Configurable]]: true
```

### [[DefineOwnProperty]] ( P, Desc )

```javascript
function OrdinaryDefineOwnProperty (O, P, Desc) {
  let current = O.[[GetOwnProperty]](P)
  let extensible = O.[[IsExtensible]]()
  return ValidateAndApplyPropertyDescriptor(O, P, extensible, Desc, current)
}

function ValidateAndApplyPropertyDescriptor(O, P, extensible, Desc, current) {
  if (O !== undefined) {
    AssertValue(IsPropertyKey(P), true)
  }
}

return OrdinaryDefineOwnProperty(O, P, Desc)
```

## 函数内部方法

函数特有的内部方法

| 内部属性 | 类型 | 描述 |
| --- | --- | --- |
| [[Call]] | (thisArg: any, argList: List{any}) => any  | 函数调用 |
| [[Construct]] | (argList: List{any}, object: Object) => Object | 对象实例化，object为初始化的对象 |
