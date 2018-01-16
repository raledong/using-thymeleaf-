# 17.1 逻辑解耦：概念
目前为止我们用通常的的方式创建了杂货店的模板，将逻辑以属性的形式插入模板中。

但是Thymeleaf允许我们将markup和逻辑完全解耦，从而在`HTML`和`XML`模式下创建 **完全没有逻辑的markup模板**。

核心的思想是将模板逻辑放在在一个另一个文件中（确切的说是逻辑资源，因为它不一定是一个文件）。默认情况下，逻辑资源和模板文件位于同样的位置（比如文件夹），拥有相同的名称并带有`.th.xml`后缀：
```
/templates
+->/home.html
+->/home.th.xml
```
因此`home.html`文件可以没有任何逻辑内容，如下：
```
<!DOCTYPE html>
<html>
  <body>
    <table id="usersTable">
      <tr>
        <td class="username">Jeremy Grapefruit</td>
        <td class="usertype">Normal User</td>
      </tr>
      <tr>
        <td class="username">Alice Watermelon</td>
        <td class="usertype">Administrator</td>
      </tr>
    </table>
  </body>
</html>
```
这里显然没有Thymeleaf代码。这是一个没有任何Thymeleaf知识或模板知识的设计师也可以创建，编辑和理解的文件。这也可以是由外部系统提供的一段代码，不包含任何的Thymeleaf连接点。

我们现在将`home.html`模板转化为Thymeleaf模板，通过`home.th.xml`文件：
```
<?xml version="1.0"?>
<thlogic>
  <attr sel="#usersTable" th:remove="all-but-first">
    <attr sel="/tr[0]" th:each="user : ${users}">
      <attr sel="td.username" th:text="${user.name}" />
      <attr sel="td.usertype" th:text="#{|user.type.${user.type}|}" />
    </attr>
  </attr>
</thlogic>
```
我们看到在`thlogic`块下有大量的`<attr>`标签。这些`<attr>`标签将属性插入到原始模板对应的节点上，通过`sel`属性选择对应的节点。`sel`属性包含Thymeleaf选择器（实际上是 *AttoParser markup选择器* ）

还要注意`<attr>`标签允许嵌套，选择器也可以拼接。比如上面的`sel="/tr[0]"`会被解析为`sel="#usersTable/tr[0]"`。而且用于用户名的`<td>`选择器也会被解析为`sel="#usersTable/tr[0]//td.username"`。

所以一旦合并上面两个文件,等价于：
```
<!DOCTYPE html>
<html>
  <body>
    <table id="usersTable" th:remove="all-but-first">
      <tr th:each="user : ${users}">
        <td class="username" th:text="${user.name}">Jeremy Grapefruit</td>
        <td class="usertype" th:text="#{|user.type.${user.type}|}">Normal User</td>
      </tr>
      <tr>
        <td class="username">Alice Watermelon</td>
        <td class="usertype">Administrator</td>
      </tr>
    </table>
  </body>
</html>
```
这看上去更熟悉些，相比于创建两个独立的文件也要简洁很多。但是解耦模板的好处是我们可以将模板完全独立于Thymeleaf，因此从设计的角度来说有更好的可维护性。

当然了，设计师和开发人员之间的契约还是需要的。比如，用户列表`table`需要`id="usersTable"`属性。但是，在很多场景中，一个纯HTML模板是设计团队和开发团队之间更好的交流产品。
