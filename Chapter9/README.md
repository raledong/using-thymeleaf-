# 9.局部变量
Thymeleaf将局部变量定义为只能在模板的特定片段中使用，并且只能在那个片段中进行计算的变量。

我们已经看到的例子是商品列表界面的`prod`单迭代变量。
```
<tr th:each="prod : ${prods}">
    ...
</tr>
```
`prod`变量只能在`<tr>`标签下使用，特别的是：
* 它可用于宿主标签中所有其他 *优先级低于*`th:each`（即在`th:each`之后执行）的`th:*`属性
* 它可用于`<tr>`标签的任何子节点，比如`<td>`。

Thymeleaf为你提供了一个方法，使用`th:with`属性，无需通过迭代即可声明局部变量。它的语法类似于属性的赋值：
```
<div th:with="firstPer=${persons[0]}">
  <p>
    The name of the first person is <span th:text="${firstPer.name}">Julius Caesar</span>.
  </p>
</div>
```
当处理`th:with`属性时，`firstPer`变量被创建为局部变量并添加到上下文的变量表中，这样在`<div>`标签下，它可以和上下文中的别的变量一起被使用。

你还能使用多赋值语法同时定义多个变量:
```
<div th:with="firstPer=${persons[0]},secondPer=${persons[1]}">
  <p>
    The name of the first person is <span th:text="${firstPer.name}">Julius Caesar</span>.
  </p>
  <p>
    But the name of the second person is
    <span th:text="${secondPer.name}">Marcus Antonius</span>.
  </p>
</div>
```
`th:with`属性允许我们重用在同一个属性中定义的变量：
```
<div th:with="company=${user.company + ' Co.'},account=${accounts[company]}">...</div>
```
让我们将它运用到我们的杂货店的主页上！还记得我们用于输出一个格式化日期的代码吗？
```
<p>
  Today is:
  <span th:text="${#calendars.format(today,'dd MMMM yyyy')}">13 february 2011</span>
</p>
```
那如果我们希望`"dd MMMM yyyy"`的值取决于语言环境呢？比如我们可能希望将以下的消息加到`home_en.properties`中：
```
date.format=MMMM dd'','' yyyy
```
以及其等价的内容至`home_es.properties`中：
```
date.format=dd ''de'' MMMM'','' yyyy
```
现在，让我们使用`th:with`来获得本地日期的格式并将其存储为局部变量，然后在`th:text`表达式中使用它：
```
<p th:with="df=#{date.format}">
  Today is: <span th:text="${#calendars.format(today,df)}">13 February 2011</span>
</p>
```
这段代码简单清晰。事实上，考虑到`th:with`的优先级比`th:text`，我们可以直接在`span`标签中完成所有操作:
```
<p>
  Today is:
  <span th:with="df=#{date.format}"
        th:text="${#calendars.format(today,df)}">13 February 2011</span>
</p>
```
你可能会想：啥是优先级？我们还没有说到这里！不用担心，因为下一章就是来介绍这个的。
