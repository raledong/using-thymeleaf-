# 6.2 获取迭代状态
当时用`th:each`属性时，Thymeleaf提供了一个机制用于跟踪迭代的状态：*状态变量*。

状态变量在`th:each`中定义，它包含以下内容：
* `index`属性：当前迭代的下标，从0开始。
* `count`属性：当前迭代的下标，从1开始。
* `size`属性：可迭代变量的总数量。
* `current`属性：每次迭代的单个迭代变量。
* `even/odd`属性：当前的迭代序号为奇数还是偶数。
* `first`布尔属性：当前的迭代是否是第一个迭代。
* `last`布尔属性：当前的迭代是否是最后一个迭代。

让我们看一下如何在之前的例子中使用它：
```
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
  </tr>
  <tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
  </tr>
</table>
```
状态变量（本例中的`iterStat`）在`th:each`中定义，在单个迭代变量后声明，二者用逗号隔开。和单个迭代变量相同，状态变量的作用域也是`th:each`属性所在的标签的代码段之内。

让我们看一看模板的运行结果：
```
<!DOCTYPE html>

<html>

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta content="text/html; charset=UTF-8" http-equiv="Content-Type"/>
    <link rel="stylesheet" type="text/css" media="all" href="/gtvg/css/gtvg.css" />
  </head>

  <body>

    <h1>Product list</h1>

    <table>
      <tr>
        <th>NAME</th>
        <th>PRICE</th>
        <th>IN STOCK</th>
      </tr>
      <tr class="odd">
        <td>Fresh Sweet Basil</td>
        <td>4.99</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>Italian Tomato</td>
        <td>1.25</td>
        <td>no</td>
      </tr>
      <tr class="odd">
        <td>Yellow Bell Pepper</td>
        <td>2.50</td>
        <td>yes</td>
      </tr>
      <tr>
        <td>Old Cheddar</td>
        <td>18.75</td>
        <td>yes</td>
      </tr>
    </table>

    <p>
      <a href="/gtvg/" shape="rect">Return to home</a>
    </p>

  </body>

</html>
```

可以看到我们的状态变量顺利运行，将`odd`CSS类添加到奇数行上。

如果你没有显式的声明一个状态变量，Thymeleaf会自动为你声明，它的名称是单个迭代变量的名称加上`Stat`后缀：
```
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
  </tr>
  <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
  </tr>
</table>
```
