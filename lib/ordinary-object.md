# 普通对象

标签（空格分隔）： ES6规范

---

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
| [[Get]] | (propertyKey: String &#124; Symbol, receiver: any) => any | 获取对象自身属性或继承属性的属性值，receiver为get访问器的this值 |
| [[Set]] | (propertyKey: String &#124; Symbol, value: any, receiver: any) | 设置对象属性，receiver为set访问器的this值 |
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

对象是否可扩展

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

阻止对象扩展

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

设置对象自身属性的属性描述符

```javascript
/**
 * P应为属性名
 * P不是O的自身属性，添加属性
 *    O不可扩展，直接返回false
 *    O可扩展，根据Desc添加数据属性或访问器属性，返回true
 * P是O的自身属性，修改属性的属性描述符
 *   能修改，用Desc里的属性描述符特性覆盖自身的，返回true
 *   不能修改，直接返回false
 *      自身属性的[[Configurable]]为false时
 *          [[Configurable]]、[[Enumberable]]不能修改
 *          不能由一种属性描述符改为另一种属性描述符
 *          数据属性描述符[[Writable]]为[[false]]时, [[Writable]]和[[Value]]都不能修改
 *          访问器属性描述符的[[Get]]和Set]]不能修改
 */
function OrdinaryDefineOwnProperty (O, P, Desc) {
  let current = O.[[GetOwnProperty]](P)
  let extensible = O.[[IsExtensible]]()
  return ValidateAndApplyPropertyDescriptor(O, P, extensible, Desc, current)
}

function ValidateAndApplyPropertyDescriptor(O, P, extensible, Desc, current) {
  if (O !== undefined) {
    AssertValue(IsPropertyKey(P), true)
  }

  if (current === undefined) {
    if (extensible === false) {
      return false
    }

    if (O !== undefined) {
      if (IsGenericDescriptor(Desc) || IsDataDescriptor(Desc)) {
        CreateOwnProperty(O, P, {
          [[Value]]: IsPresent(Desc.[[Value]]) ? Desc.[[Value]] : undefined,
          [[Writable]]: IsPresent(Desc.[[Writable]]) ? [[Desc.Writable]] : false,
          [[Enumerable]]: IsPresent(Desc.[[Enumerable]]) ? [[Desc.Enumerable]] : false,
          [[Configurable]]: IsPresent(Desc.[[Configurable]]) ? [[Desc.Configurable]] : false
        })
      } else {
        CreateOwnProperty(O, P, {
          [[Get]]: IsPresent(Desc.[[Get]]) ? [[Desc.Get]] : undefined,
          [[Set]]: IsPresent(Desc.[[Set]]) ? [[Desc.Set]] : undefined,
          [[Enumerable]]: IsPresent(Desc.[[Enumerable]]) ? Desc.[[Enumerable]] : false,
          [[Configurable]]: IsPresent(Desc.[[Configurable]]) ? Desc.[[Configurable]] : false
        })
      }
    }
    return true
  }

  if (current.[[Configurable]] === false) {
    let invalid = false
    if (Desc.[[Configurable]] === true) {
      invalid = true
    } else if (IsPresent(Desc.[[Enumerable]]) && Desc.[[Enumerable]] !== current.[[Enumerable]]) {
      invalid = true
    } else if (IsGenericDescriptor(Desc)) {
    } else if (IsDataDescriptor(Desc) !== IsDataDescriptor(current)) {
      invalid = true
    } else if (IsDataDescriptor(current)) {
      if (current.[[Writable]] === false) {
        if (Desc.[[Writable]] === true || (IsPresent(Desc.[[Value]]) && !SameValue(current.[[Value]], Desc.[[Value]]))) {
          invalid = true
        }
      }
    } else {
      if (IsPresent(Desc.[[Get]]) && !SameValue(current.[[Get]], Desc.[[Get]])) {
        invalid = true
      } else if (IsPresent(Desc.[[Set]]) && !SameValue(current.[[Set]], Desc.[[Set]])) {
        invalid = true
      }
    }

    if (invalid) {
      return false
    }
  }

  if (O !== undefined) {
    current.[[Enumerable]] = IsPresent(Desc.[[Enumerable]]) ? Desc.[[Enumerable]] :       current.[[Enumerable]]
    current.[[Configurable]] = IsPresent(Desc.[[Configurable]]) ? Desc.[[Configurable]] : current.[[Configurable]]

    if (IsDataDescriptor(Desc)) {
      if (!IsDataDescriptor(current)) {
        convertToDataDescriptor(current)
      }
      current.[[Value]] = IsPresent(Desc.Value) ? Desc.Value : current.[[Value]]
      current.[[Writable]] = IsPresent(Desc.Writable) ? Desc.Writable : current.[[Writable]]
    } else if (IsAccessorDescriptor(Desc)) {
      if (!IsAccessorDescriptor(current)) {
        convertToAccessorDescriptor(current)
      }
      current.[[Get]] = IsPresent(Desc.Get) ? Desc.Get : current.[[Get]]
      current.[[Set]] = IsPresent(Desc.Set) ? Desc.Set : current.[[Set]]
    }
  }

  return true
}

return OrdinaryDefineOwnProperty(O, P, Desc)
```

示例

```javascript
let o = {
  a: 1
}

Object.defineProperty('p', {
  get () { return 1 },
  enumerable: true,
  configurable: false
})

// 添加属性
// 可扩展
o.[[DefinedOwnProperty]]('b', { // 返回true
  [[Value]]: 2
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
})

// 不可扩展
Object.preventExtensions(o)
o.[[DefinedOwnProperty]]('c', { // 返回false
  [[Value]]: 2
  [[Writable]]: true,
  [[Enumerable]]: true,
  [[Configurable]]: true
})

// 修改属性
// 可修改
o.[[DefineOwnProperty]]('a', { // 返回true
  [[Value]]: 3
})

// 不可修改
o.[[DefineOwnProperty]]('p', { // 返回false
  [[Value]]: 3
})
```

### [[HasProperty]] ( P )

是否是对象自身属性或继承属性

```javascript
/*
 * P应为属性名
 * 获取自身属性P的属性描述符
 *    存在，直接返回true
 *    不存在，沿原型链递归，都不存在时返回false
 */
function OrdinaryHasProperty(O, P) {
  AssertValue(IsPropertyKey(P), true)

  let desc = O.[[GetOwnProperty]](P)
  if (desc !== undefined) {
    return true
  } else {
    let parent = O.[[GetPrototypeOf]]()
    if (parent === null) {
      return false
    } else {
      return parent.[[HasProperty]](P)
    }
  }
}

return OrdinaryHasProperty(O, P)
```

示例

```javascript
let o = {
  a: 1
}

// 属性不存在
o.[[HasProperty]]('b') // false

// 自身属性
o.[[HasProperty]]('a') // true

// 继承属性
o.[[HasProperty]]('toString') // true
```

### [[Get]] ( P, Receiver )

获取对象自身属性或继承属性的属性值

```javascript
/*
 * P应为属性名
 * 获取自身属性P的属性描述符
 * 存在，根据描述符类型返回属性值
 *    数据属性，返回[[Value]]值
 *    访问器属性
 *       [[Get]]不存在，返回undefined
 *       [[Get]]存在，调用[[Get]]方法，this为Receiver，参数为空
 * 不存在，沿原型链递归，都不存在时返回undefined
 */
function OrdinaryGet(O, P, Receiver) {
  AssertValue(IsPropertyKey(P), true)

  let desc = O.[[GetOwnProperty]](P)
  if (desc !== undefined) {
    if (IsDataDescriptor(desc)) {
      return desc.[[Value]]
    } else {
      let getter = desc.[[Get]]
      if (getter === undefined) {
        return undefined
      } else {
        return Call(getter, Receiver)
      }
    }
  } else {
    let parent = O.[[GetPrototypeOf]]()
    if (parent === null) {
      return undefined
    } else {
      return parent.[[Get]](P, Receiver)
    }
  }
}

return OrdinaryGet(O, P, Receiver)
```

示例

```javascript
let o = {
  a: 1,
  get p () { return this.a }
}

// 属性不存在
o.[[Get]]('b', o) // undefined

// 自身属性
o.[[Get]]('a', o) // 1
o.[[Get]]('p', o) // 1

// 继承属性
o.[[Get]]('toString', o) // function toString () { [native code] }
```

### [[Set]] ( P, V, Receiver )

设置对象自身属性的属性描述符

```javascript
/*
 * P应为属性名
 * 获取自身属性P的属性描述符
 * 存在，根据描述符类型设置属性
 *    数据属性
 *       [[Writable]]为false，返回false
 *       [[Writable]]为true，获取Receiver的属性P的属性描述符
 *         不存在，Receiver添加数据属性P，值为V
 *         存在且为可写的数据属性，Receiver设置属性P的[[Value]]为V
 *         其他情况，返回false
 *    访问器属性
 *       [[Set]]不存在，返回false
 *       [[Set]]存在，调用[[Set]]方法，this为Receiver，参数为List{V}
 * 不存在，沿原型链递归，都不存在时为Receiver添加数据属性P，值为V
 */
function OriginarySet (O, P, V, Receiver) {
  AssertValue(IsPropertyKey(P), true)

  let ownDesc = O.[[GetOwnProperty]](P)
  return OriginarySetWithOwnDescriptor(O, P, V, Receiver, ownDesc)
}

function OriginarySetWithOwnDescriptor (O, P, V, Receiver, ownDesc) {
  AssertValue(IsPropertyKey(P), true)

  if (ownDesc === undefined) {
    let parent = O.[[GetPrototypeOf]]()
    if (parent !== null) {
      return parent.[[Set]](P, V, Receiver)
    } else {
      ownDesc = {
        [[Value]]: undefined,
        [[Writable]]: true,
        [[Enumerable]]: true,
        [[Configurable]]: true
      }
    }
  }

  if (IsDataDescriptor(ownDesc)) {
    if (ownDesc.[[Writable]] === false) {
      return false
    }
    if (Type(Receiver) !== Object) {
      return false
    }

    let existingDescriptor = Receiver.[[GetOwnProperty]](P)
    if (existingDescriptor !== undefined) {
      if (IsAccessorDescriptor(existingDescriptor)) {
        return false
      }
      if (existingDescriptor.[[Writable]] === false) {
        return fasle
      }

      let valueDesc = {[[Value]]: V}
      return Receiver.[[DefineOwnProperty]](P, valueDesc)
    } else {
      return CreateDataProperty(Receiver, P, V)
    }
  }

  let setter = ownDesc.[[Set]]
  if (setter === undefined) {
    return false
  } else {
    Call(setter, Receiver, List{V})
    return true
  }
}

return OriginarySet(O, P, V, Receiver)
```

示例

```javascript
let o = {
  a: 1,
  set p (val) { this.a = val}
}

// 属性不存在，添加属性
o.[[Put]]('b', 2, o) // {a: 1, b: 2, set p}

// 自身数据属性，修改属性
o.[[Put]]('a', 3, o) // {a: 3, set p}

// 访问器属性，调用set方法
o.[[Put]]('p', 3, o) // {a: 3, set p}

// 继承的数据属性，添加属性
o.[[Put]]('toString', 5, o) // {a: 1, toString: 5, set p}
```

### [[Delete]] ( P )

删除对象自身属性

```javascript
/*
 * P应为属性名
 * 获取自身属性P的属性描述符
 * 存在，直接返回true
 * 不存在，判断属性是否可配置
 *     可配置，删除属性并返回true
 *     不可配置，返回false
 */
function OriginaryDelete (O, P) {
  AssertValue(IsPropertyKey(P), true)

  let desc = O.[[GetOwnProperty]](P)
  if (desc === undefined) {
    return true
  } else {
    if (desc.[[Configurable]]) {
      RemoveOwnProperty(O, P)
      return true
    }  else {
      return false
    }
  }
}

return OriginaryDelete(O, P)
```

示例

```javascript
let o = {
  a: 1
}
Object.defineProperty(o, 'b', {
  value: 2,
  configurable: false
})

// 非自身属性
o.[[Delete]]('c', true) // true

// 可删除的自身属性
o.[[Delete]]('a', true) // true

// 不可删除的自身属性
o.[[Delete]]('b', true) // TypeError
```

### [[OwnPropertyKeys]] ( )

获取对象自身属性名的列表

```javascript
/*
 * 按数值大小遍历数组索引属性名，添加到属性名列表中
 * 按创建时间遍历String属性名，添加到属性名列表中
 * 按创建时间遍历Symbol属性名，添加到属性名列表中
 */
function OriginaryOwnProperty (O) {
  let keys = new List()

  TraverseArrayIndexPropertySortByNumber(O, P => {
    keys.push(P)
  })

  TraverseStringPropertySortByCreateTime(O, P => {
    keys.push(P)
  })

  TraverseSymbolPropertySortByCreateTime(O, P => {
    keys.push(P)
  })

  return keys
}

return OriginaryOwnPropertyKeys(O)
```

示例

```javascript
let arr = [0, 1, 2]
arr.a = 'a'
arr[Symbol('symbol')] = 'symbol'

arr.[[OwnPropertyKeys]]() // List{0, 1, 2, 'a', Symbol('symbol')}
```

## 函数内部方法

函数特有的内部方法

| 内部属性 | 类型 | 描述 |
| --- | --- | --- |
| [[Call]] | (thisArg: any, argList: List{any}) => any  | 函数调用 |
| [[Construct]] | (argList: List{any}, object: Object) => Object | 对象实例化，object为初始化的对象 |
