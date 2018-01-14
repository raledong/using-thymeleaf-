# 4.14 预处理
除了处理表达式的所有功能，Thymeleaf还有 *预处理* 表达式功能。

预处理是指在普通表达式执行之前完成的表达式执行，允许修改将最终执行的表达式。

预处理表达式和普通的表达式完全相同，只不过他们使用双下划线标注包围（形如：`__${expression}__`）。

假设我们有一个i18n的名为`Messages_fr.properties`的文件，其值为一个调用具体语言的静态方法的OGNL表达式，如下：
```
article.text=@myapp.translator.Translator@translateToFrench({0})
```
而这个文件的西班牙语版`Messages_es.properties`为：
```
article.text=@myapp.translator.Translator@translateToSpanish({0})
```
我们可以根据访问地域来创建一个markup片段决定到底使用哪个表达式。为此，我们首先选择表达式（通过预处理）然后让Thymeleaf执行这个表达式：
```
<p th:text="${__#{article.text('textVar')}__}">Some text here...</p>
```
注意在法语地区这段代码将会转化为：
```
<p th:text="${@myapp.translator.Translator@translateToFrench(textVar)}">Some text here...</p>
```
预处理符号中的双下划线`__`可以用`\_\_`转义。
