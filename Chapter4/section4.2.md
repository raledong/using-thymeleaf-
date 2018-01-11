# 4.2 变量
我们之前提到了`${...}`表达式事实上是 **OGNL (Object-Graph Navigation Language) expressions** 作用于上下文中包含的属性映射之上。

>要想了解更多关于OGNL语法和功能，你可以阅读[OGNL语法指南](http://commons.apache.org/ognl/)

>在Spring MVC支持的应用中，OGNL会被SpringEL替换，但是二者的语法非常相近（事实上，在大多数场景下都完全相同）

根据OGNL语法，我们知道下面的表达式：
```
<p>Today is: <span th:text="${today}">13 february 2011</span>.</p>
```
...实际上等价于这个：
```
ctx.getVariable("today");
```
但是OGNL允许我们创建更加强大的表达式，如下：
```
<p th:utext="#{home.welcome(${session.user.name})}">
  Welcome to our grocery store, Sebastian Pepper!
</p>
```
...相当于通过执行如下语句获取了用户名：
```
((User) ctx.getVariable("session").get("user")).getName();
```
调用getter方法只是OGNL的功能之一，让我们了解更多功能：
```
/*
 * 使用点（.）来获取属性，等价于调用属性的getter方法
 */
${person.father.name}

/*
 * 使用中括号（[]）来获取属性，将属性的内容写在中括号之间用单括号包起来
 */
${person['father']['name']}

/*
 * 如果对象是一个字典序，点和中括号都等价于调用get(...)方法
 */
${countriesByCode.ES}
${personsByName['Stephen Zucchini'].age}

/*
 * 可以使用中括号([])通过下标访问数组或集合
 * 下标不要用单括号包围
 */
${personsArray[0].name}

/*
 * 方法可以被直接调用，甚至可以传入参数
 */
${person.createCompleteName()}
${person.createCompleteNameWithSeparator('-')}
```

### 表达式基本对象 Expression Basic Objects ###
当在上下文变量中计算OGNL表达式时，为了更高的灵活性，一些对象被创建以供表达式使用。这些对象通过`#`为开头的标识被引用。
* `#ctx`: 上下文
* `#vars`: 上下文所有变量
* `#locale`: 上下文语言环境
* `#request`: (只适用于web上下文) HttpServletRequest对象
* `#response`: (只适用于web上下文) HttpServletResponse对象
* `#session`: (只适用于web上下文) HttpSession对象
* `#servletContext`: (只适用于web上下文) ServletContext对象

所以我们可以这么操作：
```
Established locale country: <span th:text="${#locale.country}">US</span>.
```
你可以在附录A中查看完整的引用。

### 表达式工具对象 Expression Utility Objects###
除了基本对象，Thymeleaf还提供了一组工具对象来帮助我们在表达式中执行一般的任务。
* `#execInfo`: 被处理的模板的信息.
* `#messages`: 等价于#{...}
* `#uris`: 转义部分URLs/URIs
* `#conversions`: 执行预定义的转换功能
* `#dates`: 用于`java.util.Date`对象的方法: 格式化, 成分提取等
* `#calendars`: 类似于`#dates`,但是用于`java.util.Calendar`对象
* `#numbers`: 格式化数字对象的方法
* `#strings`: 用于String对象的方法：contains, startsWith, prepending/appending等
* `#objects`: 用于一般object对象上的方法
* `#bools`: 用于布尔值计算的方法
* `#arrays`: 用于数组的方法m
* `#lists`: 用于列表的方法
* `#sets`: 用于堆sets的方法
* `#maps`: 用于map的方法
* `#aggregates`: 用于在数组和集合上进行合计操作的方法
* `#ids`: 用于处理可能会存在重复的id属性的方法

你可以在附录B中查看每个工具对象提供的功能。

### 再次格式化我们主页中的日期内容###
现在我们知道了工具对象，我们可以通过他们换一种方式在主页中展示日期，之前我们在`HomeController`中的代码如下：
```
SimpleDateFormat dateFormat = new SimpleDateFormat("dd MMMM yyyy");
Calendar cal = Calendar.getInstance();

WebContext ctx = new WebContext(request, servletContext, request.getLocale());
ctx.setVariable("today", dateFormat.format(cal.getTime()));

templateEngine.process("home", ctx, response.getWriter());
```
现在我们只需这么写：
```
WebContext ctx =
    new WebContext(request, response, servletContext, request.getLocale());
ctx.setVariable("today", Calendar.getInstance());

templateEngine.process("home", ctx, response.getWriter());
```
然后在视图层执行日期的格式化：
```
<p>
  Today is: <span th:text="${#calendars.format(today,'dd MMMM yyyy')}">13 May 2011</span>
</p>
```
