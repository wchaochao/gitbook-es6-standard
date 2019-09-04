# 约定

标签（空格分隔）： ES6规范

---

## 语法约定

* 非终结符可继续向下推演
* 终结符到此为止

示例

```
WhileStatement:
  while (Expression) Statement
```

### [empty]

匹配空

### one of

匹配可选项

```
NonZeroDigit::one of
  1 2 3 4 5 6 7 8 9

// 等价于

NonZeroDigit::
  1
  2
  3
  4
  5
  6
  7
  8
  9
```

### but not

除...之外

```
Identifier::
  IdentifierName but not ReservedWord
```

### [no LineTerminator here]

不为行结束符

```
ThrowStatement:
  throw [no LineTerminator here] Expression;
```

### 描述语

```
SourceCharacter::
  any Unicode code point
```

### lookahead

紧跟着匹配或不匹配

```
LookaheadExample::
  n [lookahead ∉ { 1, 3, 5, 7, 9 }] DecimalDigits
  DecimalDigit [lookahead ∉ DecimalDigit]
```

### opt

可选项

```
VariableDeclaration:
  BindingIdentifier Initializer(opt)

// 等价于

VariableDeclaration:
  BindingIdentifier
  BindingIdentifier Initializer
```

### 后缀[parameters]

下划线拼接参数

```
StatementList[Return, In]:
  ReturnStatement
  ExpressionStatement

// 等价于

StatementList:
  ReturnStatement
  ExpressionStatement

StatementList_Return:
  ReturnStatement
  ExpressionStatement

StatementList_In:
  ReturnStatement
  ExpressionStatement

StatementList_Return_In:
  ReturnStatement
  ExpressionStatement
```

### 后缀[+parameters]

有参数

```
StatementList:
  ReturnStatement
  ExpressionStatement[+In]

// 等价于

StatementList:
  ReturnStatement
  ExpressionStatement_In
```

### 后缀[~parameters]

无参数

```
StatementList:
  ReturnStatement
  ExpressionStatement[~In]

// 等价于

StatementList:
  ReturnStatement
  ExpressionStatement
```

### 后缀[?parameters]

左侧有参数时参数存在

```
VariableDeclaration[In]:
  BindingIdentifierInitializer[?In]

// 等价于

VariableDeclaration:
  BindingIdentifierInitializer

VariableDeclaration_In:
  BindingIdentifierInitializer_In
```

### 前缀[+parameters]

左侧有参数时表达式存在

```
StatementList[Return]:
  [+Return]ReturnStatement
  ExpressionStatement

// 等价于

StatementList:
  ExpressionStatement

StatementList_Return:
  ReturnStatement
  ExpressionStatement
```

### 前缀[~parameters]

左侧无参数时表达式存在

```
StatementList[Return]:
  [~Return]ReturnStatement
  ExpressionStatement

// 等价于

StatementList:
  ReturnStatement
  ExpressionStatement

StatementList_Return:
  ExpressionStatement
```

## 算法约定

### Assert

断言

```
Assert(completionRecord, IsCompletion)
```

### ReturnIfAbrupt(arg)

为中断Completion时直接返回

```javascript
if (IsAbruptCompletion(arg)) {
  return arg
} else if (Type(arg) === Completion) {
  arg = arg.[[Value]]
}
```

### ?OperationName()

等价于ReturnIfAbrupt

```javascript
let arg = OperationName()
ReturnIfAbrupt(arg)
```

### !OperationName()

不为Abrupt Completion

```javascript
let arg = OperationName()
Assert(arg, IsNotAbruptCompletion)
if (Type(arg) === Completion) {
  arg = arg.[[Value]]
}
```
