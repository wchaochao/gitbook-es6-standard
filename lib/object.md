# 对象

标签（空格分隔）： ES6规范

---

属性的集合

* `ordinary object`: 普通对象，有通用的属性和方法
* `exotic object`: 异常对象，某些属性和方法与普通对象表现不同

## 常见内置对象

### global

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %eval% | eval | eval方法 |
| %isNaN% | isNaN | isNaN方法 |
| %isFinite% | isFinite | isFinite方法 |
| %parseInt% | parseInt | parseInt方法 |
| %parseFloat% | parseFloat | parseFloat方法 |
| %encodeURI% | encodeURI | encodeURI方法 |
| %encodeURIComponent% | encodeURIComponent | encodeURIComponent方法 |
| %decodeURI% | decodeURI | decodeURI方法 |
| %decodeURIComponent% | decodeURIComponent | decodeURIComponent方法 |

### Object

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Object% | Object | Object构造器 |
| %ObjectPrototype% | Object.prototype | Object原型 |
| %ObjProto_toString% | Object.prototype.toString | Object原型的toString方法 |
| %ObjProto_valueOf% | Object.prototype.valueOf | Object原型的valueOf方法 |

### Function

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Function% | Function | Function构造器 |
| %FunctionPrototype% | Function.prototype | Function原型 |
| %ThrowTypeFunction% | 无 | ThrowTypeError函数 |

### Array

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Array% | Array | Array构造器 |
| %ArrayPrototype% | Array.prototype | Array原型 |
| %ArrayProto_forEach% | Array.prototype.forEach | Array原型的forEach方法 |
| %ArrayProto_keys% | Array.prototype.keys | Array原型的keys方法 |
| %ArrayProto_values% | Array.prototype.values | Array原型的values方法 |
| %ArrayProto_entries% | Array.prototype.entries | Array原型的entries方法 |

### Number

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Number% | Number | Number构造器 |
| %NumberPrototype% | Number.prototype | Number原型 |

### String

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %String% | String | String构造器 |
| %StringPrototype% | String.prototype | String原型 |

### Boolean

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Boolean% | Boolean | Boolean构造器 |
| %BooleanPrototype% | Boolean.prototype | Boolean原型 |

### Date

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Date% | Date | Date构造器 |
| %DatePrototype% | Date.prototype | Date原型 |

### RegExp

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %RegExp% | RegExp | RegExp构造器 |
| %RegExpPrototype% | RegExp.prototype | RegExp原型 |

### Error

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Error% | Error | Error构造器 |
| %ErrorPrototype% | Error.prototype | Error原型 |
| %SyntaxError% | SyntaxError | SyntaxError构造器 |
| %SyntaxErrorPrototype% | SyntaxError.prototype | SyntaxError原型 |
| %ReferenceError% | ReferenceError | ReferenceError构造器 |
| %ReferenceErrorPrototype% | ReferenceError.prototype | ReferenceError原型 |
| %RangeError% | RangeError | RangeError构造器 |
| %RangeErrorPrototype% | RangeError.prototype | RangeError原型 |
| %TypeError% | TypeError | TypeError构造器 |
| %TypeErrorPrototype% | TypeError.prototype | TypeError原型 |
| %URIError% | URIError | URIError构造器 |
| %URIErrorPrototype% | URIError.prototype | URIError原型 |
| %EvalError% | EvalError | EvalError构造器 |
| %EvalErrorPrototype% | EvalError.prototype | EvalError原型 |

### Math

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Math% | Math | Math对象 |

### JSON

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %JSON% | JSON | JSON对象 |
| %JSONParse% | JSON.parse | JSON的parse方法 |
| %JSONStringify% | JSON.stringify | JSON的stringify方法 |

### Symbol

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Symbol% | Symbol | Symbol构造器 |
| %SymbolPrototype% | Symbol.prototype | Symbol原型 |

### Set

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Set% | Set | Set构造器 |
| %SetPrototype% | Set.prototype | Set原型 |
| %WeekSet% | WeekSet | WeekSet构造器 |
| %WeekSetPrototype% | WeekSet.prototype | WeekSet原型 |

### Map

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Map% | Map | Map构造器 |
| %MapPrototype% | Map.prototype | Map原型 |
| %WeekMap% | WeekMap | WeekMap构造器 |
| %WeekMapPrototype% | WeekMap.prototype | WeekMap原型 |

### ArrayBuffer

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %ArrayBuffer% | ArrayBuffer | ArrayBuffer构造器 |
| %ArrayBufferPrototype% | ArrayBuffer.prototype | ArrayBuffer原型 |
| %SharedArrayBuffer% | SharedArrayBuffer | SharedArrayBuffer构造器 |
| %SharedArrayBufferPrototype% | SharedArrayBuffer.prototype | SharedArrayBuffer原型 |
| %TypedArray% | 无 | TypedArray构造器 |
| %TypedArrayPrototype% | 无 | TypedArray原型 |
| %Uint8Array% | Uint8Array | Uint8Array构造器 |
| %Uint8ArrayPrototype% | Uint8Array.prototype | Uint8Array原型 |
| %Uint8ClampedArray% | Uint8ClampedArray | Uint8ClampedArray构造器 |
| %Uint8ClampedArrayPrototype% | Uint8ClampedArray.prototype | Uint8ClampedArray原型 |
| %Uint16Array% | Uint16Array | Uint16Array构造器 |
| %Uint16ArrayPrototype% | Uint16Array.prototype | Uint16Array原型 |
| %Uint32Array% | Uint32Array | Uint32Array构造器 |
| %Uint32ArrayPrototype% | Uint32Array.prototype | Uint32Array原型 |
| %Int8Array% | Int8Array | Int8Array构造器 |
| %Int8ArrayPrototype% | Int8Array.prototype | Int8Array原型 |
| %Int16Array% | Int16Array | Int16Array构造器 |
| %Int16ArrayPrototype% | Int16Array.prototype | Int16Array原型 |
| %Int32Array% | Int32Array | Int32Array构造器 |
| %Int32ArrayPrototype% | Int32Array.prototype | Int32Array原型 |
| %Float32Array% | Float32Array | Float32Array构造器 |
| %Float32ArrayPrototype% | Float32Array.prototype | Float32Array原型 |
| %Float64Array% | Float64Array | Float64Array构造器 |
| %Float64ArrayPrototype% | Float64Array.prototype | Float64Array原型 |
| %DateView% | DateView | DateView构造器 |
| %DateViewPrototype% | DateView.prototype | DateView原型 |

### Promise

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Promise% | Promise | Promise构造器 |
| %PromisePrototype% | Promise.prototype | Promise原型 |
| %Promise_resolve% | Promise.resolve | Promise的resolve方法 |
| %Promise_reject% | Promise.reject | Promise的reject方法 |
| %Promise_all% | Promise.all | Promise的all方法 |
| %PromiseProto_then% | Promise.prototype.then | Promise原型的then方法 |

### Async

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %AsyncFunction% | 无 | AsyncFunction构造器 |
| %AsyncFunctionPrototype% | 无 | AsyncFunction原型 |

### Iterator

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %IteratorPrototype% | 无 | Iterator原型 |
| %ArrayIteratorPrototype% | 无 | ArrayIterator原型 |
| %StringIteratorPrototype% | 无 | StringIterator原型 |
| %MapIteratorPrototype% | 无 | MapIterator原型 |
| %SetIteratorPrototype% | 无 | SetIterator原型 |
| %AsyncIteratorPrototype% | 无 | AsyncIterator原型 |
| %AsyncFromSyncIteratorPrototype% | 无 | AsyncFromSyncIterator原型 |

### Generator

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %GeneratorFunction% | 无 | Generator构造器 |
| %Generator% | 无 | Generator原型 |
| %GeneratorPropertype% | 无 | Generator原型的原型 |
| %AsyncGeneratorFunction% | 无 | 异步Generator构造器 |
| %AsyncGenerator% | 无 | 异步Generator原型 |
| %AsyncGeneratorPropertype% | 无 | 异步Generator原型的原型 |

### Proxy

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Proxy% | Proxy | Proxy对象 |

### Reflect

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Reflect% | Reflect | Reflect对象 |

### Atomics

| 内部名称 | 全局名称 | 说明 |
| --- | --- | --- |
| %Atomics% | Atomics | Atomics对象 |
