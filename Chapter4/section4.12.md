# 4.12 默认表达式（Elvis operator）
*默认表达式* 是一个没有*然后then*部分的特殊的条件表达式。它等价于一些语言中的*Elvis操作符*，比如Groovy。它让你定义两个表达式：当第一个表达式不等于null时，则返回第一个表达式的结果，否则返回第二个表达式的结果。

让我们看看它在用户信息页面上的使用：
```
<div th:object="${session.user}">
  ...
  <p>Age: <span th:text="*{age}?: '(no age specified)'">27</span>.</p>
</div>
```
如你所见，`?:`操作符在这里为`age`设置了一个默认值（一个常量）。当且仅当`*{age}`等于null的时候被设置为默认值。它等价于：
```
<p>Age: <span th:text="*{age != null}? *{age} : '(no age specified)'">27</span>.</p>
```

和条件表达式一样，他们也可以通过括号包含嵌套的表达式：
```
<p>
  Name:
  <span th:text="*{firstName}?: (*{admin}? 'Admin' : #{default.username})">Sebastian</span>
</p>
```
