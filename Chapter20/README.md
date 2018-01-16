# 20.附录C Markup选择器语法
thymeleaf的markup选择器直接借用于Thymeleaf的解析库：[AttoParser](http://www.attoparser.org/)

该选择器的语法和XPath，CSS和jQuery的选择器非常相似，对于大多数用户来说易于使用。你可以看一下[AttoParser文档](http://www.attoparser.org/apidocs/attoparser/2.0.4.RELEASE/org/attoparser/select/package-summary.html)中完整的语法介绍。

比如，下面的选择器会选中markup中所有class值包含`content`的`<div>`标签（注意这并不是很简洁，继续阅读来了解原因）：
```
<div th:insert="mytemplate :: //div[@class='content']">...</div>
```
基本的语法包括：
* `/x`指当前节点下名为`x`的直接子节点
* `//x`指当前节点下名为`x`的任何深度的子节点
* `x[@z="v"]`指`x`节点且有值为"v"的`z`属性
* `x[@z1="v1" and @z2="v2"]`指`x`节点且有值为`v1`和`v2`的属性`z1`和`z2`
* `x[i]`指在兄弟节点中位于第`i`位的名为`x`的节点
* `x[@z="v"][i]`值`x`节点且其在兄弟节点中位于第`i`为且其`z`属性的值为`v`

还可以使用更简洁的语法：
* `x`等价于`//x`(寻找任意深度的包含引用`x`的元素，引用是指`th:ref`或`th:fragment`属性的值)
* 选择器也可以没有元素名称/引用，只指定其参数。因此`[@class='oneclass']`是一个合法的选择器，它查找任何`class`值包含“oneclass”的元素（标签）。

高级属性选择功能：
* 除了`=`（相等），其它的比较也是合法的：`!=` (不等), `^=` (以...开头) 和`$=` (以...结尾)。比如，`x[@class^='section']`指元素`x`且其class属性有一个值的开头为`section`。
* 属性可以用`@`指定（XPath风格），也可以不用（jQuery风格）。所以`x[z='v']`等价于`x[@z='v']`
* 多属性修改器可以用XPath风格和JQuery风格实现。因此`x[@z1='v1' and @z2='v2']`等价于`x[@z1='v1'][@z2='v2']`（和`x[z1='v1'][z2='v2']`）

类似jQuery的选择器：
* `x.oneclass`等价于`x[class='oneclass']`
* `.oneclass`等价于`[class='oneclass']`
* `x#oneid`等价于`x[id='oneid']`
* `#oneid` 等价于`[id='oneid']`
* `x%oneref`指有`th:ref="oneref"`或`th:fragment="oneref"`属性的`<x>`标签
* `%oneref`指有`th:ref="oneref"`或`th:fragment="oneref"`属性的任何标签。注意这实际上等价于`oneref`因为引用可以替代元素名称。
* 直接选择器和属性选择器可以混合使用，如`a.external[@href^='https']`

所以上面的markup选择表达式：
```
<div th:insert="mytemplate :: //div[@class='content']">...</div>
```
可以写成：
```
<div th:insert="mytemplate :: div.content">...</div>
```
验证另一个例子，如下：
```
<div th:replace="mytemplate :: myfrag">...</div>
```
它会寻找`th:fragment="myfrag"`片段（或是`th:ref`引用）。但是它也会寻找名为`myfrag`的标签（但是HTML中没有）。注意和下面的区别：
```
<div th:replace="mytemplate :: .myfrag">...</div>
```
它会寻找任何带有`class="myfrag"`属性的元素，不会关心`th:fragment`（或是`th:ref`）。

### 多类匹配 ###
---------------------------------------
markup选择器知道类属性可以为多个值，因此允许选择器作用于有多个值的class属性。

比如`div.two`可以匹配`<div class="one two three" />`
