# 13.1 文本语法
Thymeleaf有三种文本型模板模式：`TEXT`, `JAVASCRIPT`和`CSS`。这将它们和markup模板模式：`HTML`和`XML`区分开来。

二者之间最本质的区别是文本模式中没有标签使我们可以用属性的形式插入逻辑，因此我们需要依赖于别的机制。

内联是首当其要而且也是最基础的方法，在上一章节中我们已经详细描述了。内联语法是文本模板模式下输出表达式结果的最简单方法，所以下面是个完全合法的邮件文本模板：
```
Dear [(${name})],

Please find attached the results of the report you requested
with name "[(${report.name})]".

Sincerely,
  The Reporter.
```
即使没有标签，上面的例子也是一个完整且合法的可以在`文本模板模式`下运行的Thymeleaf模板。

但是为了引入比仅仅是输出表达式结果更复杂的逻辑，我们需要一个全新的不基于标签的语法：
```
[# th:each="item : ${items}"]
  - [(${item})]
[/]
```
这实际上是更详细的浓缩版本：
```
[#th:block th:each="item : ${items}"]
  - [#th:block th:utext="${item}" /]
[/th:block]
```
注意这个新语法是基于声明为`[#element ...]`而不是`<element ...>`的元素（如可处理标签）。元素以`[#element ...]`形式开始，以`[/element]`结尾，而且独立标签可以通过`/`最小化开放元素来声明，这种方式几乎等价于XML标签：`[#element ... /]`。

标准方言只有一个处理器用于这些元素中的一个：已知的`th:block`，但是我们能够在方言中进行扩展并用通常的方式创建新的元素。因此，`th:block`元素（`[#th:block ...] ... [/th:block]`）可以缩写为空字符串（`[# ...] ... [/]`），所以上面的代码块实际上等价于：
```
[# th:each="item : ${items}"]
  - [# th:utext="${item}" /]
[/]
```
考虑到`[# th:utext="${item}" /]`等价于内联无转义表达式，我们可以使用内联减少代码量，于是我们得到了之前看到的第一段代码：
```
[# th:each="item : ${items}"]
  - [(${item})]
[/]
```
要注意 *文本语法要求全部元素平衡（没有未闭合的标签）和加引号的属性值*，这更接近XML风格而不是HTML风格。

让我们看一个更完整的 *文本模板* 的例子，一个纯文本电子邮件模板：
```
Dear [(${customer.name})],

This is the list of our products:

[# th:each="prod : ${products}"]
   - [(${prod.name})]. Price: [(${prod.price})] EUR/kg
[/]

Thanks,
  The Thymeleaf Shop
```
执行后结果如下：
```
Dear Mary Ann Blueberry,

This is the list of our products:

   - Apricots. Price: 1.12 EUR/kg
   - Bananas. Price: 1.78 EUR/kg
   - Apples. Price: 0.85 EUR/kg
   - Watermelon. Price: 1.91 EUR/kg

Thanks,
  The Thymeleaf Shop
```
*JAVASCRIPT* 模板模式中还有一个例子，一个`greeter.js`文件，我们将它当作文本模板处理然后在HTML中调用它的结果。注意这不是HTML模板中的一个`<script>`代码块，而是作为一个`.js`文件模板被独立处理。
```
var greeter = function() {

    var username = [[${session.user.name}]];

    [# th:each="salut : ${salutations}"]    
      alert([[${salut}]] + " " + username);
    [/]

};
```
运行后结果如下：
```
var greeter = function() {

    var username = "Bertrand \"Crunchy\" Pear";

      alert("Hello" + " " + username);
      alert("Ol\u00E1" + " " + username);
      alert("Hola" + " " + username);

};
```

### 转义的元素属性 ###
---------------------------------------
为了避免和模板中可能被别的模式处理的部分进行交互（如位于HTML模板下的内联文本），Thymeleaf3.0允许对元素属性中的文本语法进行转义，所以：
* 文本模板模式下的属性将不会进行HTML转义。
* JavaScript模板模式下的属性不会进行JS转义。
* CSS模板模式下的属性不会进行CSS转义。

所以下面的代码在文本模板模式中是完全可以的（注意`&gt;`）
```
[# th:if="${120&lt;user.age}"]
    Congratulations!
 [/]
```
当然了，`&lt;`在真实的文本模板中没有任何意义，但是如果我们在处理HTML模板时使用`th:inline="text"`块包住上面这段代码，这就是一个好的实践。我们希望确保浏览器在将文件作为原型静态打开时不会将`<user.age`当做一个开标签。
