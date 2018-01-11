# 3.2 文本和变量之外的内容
### Unescaped Text ###
---------------------------------------
主页的最简单版本似乎已经完成了，但是有一件事情我们似乎忘了...如果我们有如下形式的消息呢？
```
home.welcome=Welcome to our <b>fantastic</b> grocery store!
```
如果我们像之前那样运行模板，我们会得到这样的结果：
```
<p>Welcome to our &lt;b&gt;fantastic&lt;/b&gt; grocery store!</p>
```
这并不是我们所要的结果，因为我们的`<b>`标签已经被转义了，它会在浏览器中展示出来。

这是`th:text`属性的默认行为。如果我们希望Thymeleaf不对HTML标签进行转义，那么我们需要使用一个不同的属性`th:utext`用于不转义的文本。
```
<p th:utext="#{home.welcome}">Welcome to our grocery store!</p>
```
这样产生的文本就如我们所希望的那样：
```
<p>Welcome to our <b>fantastic</b> grocery store!</p>
```

###使用和展示变量###
---------------------------------------
现在让我们给我们的主页添加更多的功能。比如，我们可能希望在欢迎消息之下展示一些数据，如下：
```
Welcome to our fantastic grocery store!

Today is: 12 july 2010
```
首先，修改controller，增加一个日期数据作为上下文变量。
```
public void process(
            final HttpServletRequest request, final HttpServletResponse response,
            final ServletContext servletContext, final ITemplateEngine templateEngine)
            throws Exception {

    SimpleDateFormat dateFormat = new SimpleDateFormat("dd MMMM yyyy");
    Calendar cal = Calendar.getInstance();

    WebContext ctx =
            new WebContext(request, response, servletContext, request.getLocale());
    ctx.setVariable("today", dateFormat.format(cal.getTime()));

    templateEngine.process("home", ctx, response.getWriter());

}
```
我们已经在上下文中添加了`String`类型的变量`today`，现在我们可以在模板中展示它：
```
<body>

  <p th:utext="#{home.welcome}">Welcome to our grocery store!</p>

  <p>Today is: <span th:text="${today}">13 February 2011</span></p>

</body>
```
如你所见，我们还是使用`th:text`属性来完成工作（这是正确的，因为我们想替换标签的内容），但是这里的语法有些许的不同。我们使用`${...}`而不是`#{...}`。这是一个变量表达式，它包含了一个称作`OGNL(Object-Graph Navigation Language)`的表达式，这个表达式可以在上下文变量映射中执行读取变量操作。

`${today}`表达式就是指 *"获得成为today的变量"*，但是这些表达式还可以更复杂（比如`${user.name}`是指 *"获取称为user的变量，并调用`getName()`方法"* ）

属性的值有很多中可能情况：消息，变量表达式...接下来的章节将会展示所有可能的情况。
