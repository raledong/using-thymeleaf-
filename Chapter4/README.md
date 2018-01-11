# 4.标准表达式语法
我们先暂停一下虚拟杂货店的开发来学习Thymeleaf标准方言的最重要的部分之一：Thymeleaf标准表达式语法(Thymeleaf Standard Expression syntax)。

我们已经见过了两种用这种语法表达的合理的属性值：消息和变量表达式：
```
<p th:utext="#{home.welcome}">Welcome to our grocery store!</p>

<p>Today is: <span th:text="${today}">13 february 2011</span></p>
```
但是表达式的种类远不止这些，而且关于我们已知的这两种还有一些更有趣的细节需要了解。首先，让我们快速的看一下标准表达式的功能。
* 简单表达式 Simple Expressions:
  * 变量表达式 Variable Expressions： `${...}`
  * 选中变量表达式 Selection Variable Expressions: `*{...}`
  * 消息表达式 Message Expressions: `#{...}`
  * 连接URL表达式 Link URL Expressions: `@{...}`
  * 片段表达式 Fragment Expressions: `~{...}`
* 常量 Literals
  * 文本常量 Text literals: `'one text'`, `'Another one!'`,…
  * 数字常量 Number literals: `0`, `34`, `3.0`, `12.3`,…
  * 布尔常量 Boolean literals: `true`, `false`
  * 空常量 Null literal: `null`
  * 常符号 Literal tokens: `one`, `sometext`, `main`,…
* 文本操作 Text operations:
  * 字符串连接 String concatenation: `+`
  * 常量替换 Literal substitutions: `|The name is ${name}|`
* 算数操作 Arithmetic operations:
  * Binary operators: `+`, `-`, `*`, `/`, `%`
  * Minus sign (unary operator): `-`
* 布尔操作 Boolean operations:
  * 布尔操作符 Binary operators: `and`, `or`
  * 布尔否定 一元操作符Boolean negation (unary operator): `!`, `not`
* 比较和相等 Comparisons and equality:
  * 比较符 Comparators: `>`, `<`, `>=`, `<=` (`gt`, `lt`, `ge`, `le`)
  * 相等符 Equality operators: `==`, `!=` (`eq`, `ne`)
* 条件操作符 Conditional operators:
  * If-then: `(if) ? (then)`
  * If-then-else: `(if) ? (then) : (else)`
  * 默认值 Default: `(value) ?: (defaultvalue)`
* 特殊符号 Special tokens:
  * 无操作符 No-Operation: `_`

这些功能可以自由的组合和嵌套:
```
'User is of type ' + (${user.isAdmin()} ? 'Administrator' : (${user.type} ?: 'Unknown'))
```
