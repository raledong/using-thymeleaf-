# 4.1 消息
As we already know, #{...} message expressions allow us to link this:
我们已经知道，`#{...}`消息表达式允许我们连接内容：
```
<p th:utext="#{home.welcome}">Welcome to our grocery store!</p>
```
...至这里：
```
home.welcome=¡Bienvenido a nuestra tienda de comestibles!
```
但是有一个问题我们没有考虑到：如果消息内容不是完全静态的怎么办？比如，如果我们的应用拥有访问网站的用户的信息，并且希望欢迎语句中包含用户的名字怎么办？场景如下：
```
<p>¡Bienvenido a nuestra tienda de comestibles, John Apricot!</p>
```
这意味着我们需要给消息添加一个变量，如下：
```
home.welcome=¡Bienvenido a nuestra tienda de comestibles, {0}!
```
参数由`java.text.MessageFormat`标准语法定义，这意味着我们可以利用API文档中的方法标准化数字和日期。

为了设置参数值，假设在HTTP session中有一个属性称为`user`，那么我们可以：
```
<p th:utext="#{home.welcome(${session.user.name})}">
  Welcome to our grocery store, Sebastian Pepper!
</p>
```
还可以定义多个参数，每个参数之间用逗号`,`隔开。事实上，消息键的内容本身就可以来自于一个变量：
```
<p th:utext="#{${welcomeMsgKey}(${session.user.name})}">
  Welcome to our grocery store, Sebastian Pepper!
</p>
```
