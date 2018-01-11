# 4.3 选中表达式
表达式变量不仅可以位于`${...}`中，还可以位于`*{...}`中。

但是这二者之间有一个重要的区别：星型语法在选中的对象上计算表达式而不是在整个上下文之上。也就是说，如果没有选中的对象，那么`${...}`和`*{...}`执行的操作是相同的。

那么什么是一个被选中的对象？它是指被`th:object`表达式标记的对象。让我们在用户信息页(`userprofile.html`)使用一下这个功能：
```
<div th:object="${session.user}">
  <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
</div>
```
这段代码等价于：
```
<div>
  <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
</div>
```
当然了，这两个符号可以混合使用：
```
<div th:object="${session.user}">
  <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
</div>
```
正如上文所说，如果没有选中的对象，这二者执行的操作是等价的。
```
<div>
  <p>Name: <span th:text="*{session.user.name}">Sebastian</span>.</p>
  <p>Surname: <span th:text="*{session.user.surname}">Pepper</span>.</p>
  <p>Nationality: <span th:text="*{session.user.nationality}">Saturn</span>.</p>
</div>
```
