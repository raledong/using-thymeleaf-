# 5.1 设置任意属性的值
假设我们的网站发布了简讯，并且我们希望用户能够订阅这个简讯，于是我们新建了包含一个表格的`/WEB-INF/templates/subscribe.html`页面。

```
<form action="subscribe.html">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" />
  </fieldset>
</form>
```
在Thymeleaf中，这个模板更像是一个静态的原型界面，而不是web应用的模板界面。首先，表单中的`action`属性连接到自身页面，无法进行路径重写。其次，提交按钮的`value`属性中的值为英语，但是我们希望将其国际化。

于是`th:attr`属性隆重登场，它可以改变它设置的标签属性的值。
```
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```
背后的理念很直观：`th:attr`获取一个赋值给属性的表达式并执行这个操作。在创建了响应的控制器和消息文件后，这个文件的处理结果是：
```
<form action="/gtvg/subscribe">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="¡Suscríbe!"/>
  </fieldset>
</form>
```
除了赋予属性新的值，你还会发现，应用的上下文名称被自动的添加至URL的前缀并生成`/gtvg/subscribe`这个路径。

但是如果我们想要一次性设置多个属性怎么办？XML规定不允许在一个标签中对同一个属性多次赋值，所以`th:attr`可以接收以逗号作为分割的多个赋值语句，如下：
```
<img src="../../images/gtvglogo.png"
     th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```
假设存在对应的消息文件，那么生成的结果如下：
```
<img src="/gtgv/images/gtvglogo.png" title="Logo de Good Thymes" alt="Logo de Good Thymes" />

```
