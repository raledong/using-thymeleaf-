# 6.3 通过惰性读取数据优化性能
有时我们可能希望优化集合数据的获取（比如从数据库中获取），这样这些集合只有在真正需要时才会被获取。

>其实这个功能可以用于任何数据，但是考虑到内存中集合的大小，仅在需要时获取集合是这种场景下最常见的情况。

为了支持这个功能，Thymeleaf提供了一个方法惰性加载上下文中的变量。实现了`ILazyContextVariable`接口的上下文变量（通常是继承`LazyContextVariable`的默认实现）将会在执行时中获取数据。例如：
```
context.setVariable(
     "users",
     new LazyContextVariable<List<User>>() {
         @Override
         protected List<User> loadValue() {
             return databaseRepository.findAllUsers();
         }
     });
```
使用这个变量时无需知道其是否为惰性加载，如下：
```
<ul>
  <li th:each="u : ${users}" th:text="${u.name}">user name</li>
</ul>
```
但是与此同时，这个变量永远不会被加载（即它的`loadValue()`方法永远不会被调用）如果`condition`结果为`false`:
```
<ul th:if="${condition}">
  <li th:each="u : ${users}" th:text="${u.name}">user name</li>
</ul>
```
