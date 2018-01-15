# 7.2 switch语句
还有一种条件性展示内容的方法，它等价于Java中的 *switch* 语法：`th:switch / th:case`属性集。
```
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
</div>
```
只要有一个`th:case`属性被评估为`true`，那么同一个switch上下文中的其它`th:case`都会被评估为`false`。

默认选项为`th:case="*"`:
```
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```
