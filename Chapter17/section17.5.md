# 17.5 逻辑解耦的解决方法
Thymeleaf解析每个模板对应的逻辑解耦资源的方式是可以由用户进行配置的。它由一个扩展点决定，`org.thymeleaf.templateparser.markup.decoupled.IDecoupledTemplateLogicResolver`。提供的默认实现为`StandardDecoupledTemplateLogicResolver`。

这个标准的实现做了什么？

首先，它将一个前缀和后缀添加至模板资源的基础名称上（通过`ITemplateResolver#getBaseName()`方法）。前缀和后缀都可以配置，而且，默认情况下前缀为空，后缀为`.th.xml`。

接着，它请求模板资源解析由`ITemplateResolver#relative`方法计算的资源的名称。

`IDecoupledTemplateLogicResolver`的详细实现可以在`TemplateEngine`中配置。
```
final StandardDecoupledTemplateLogicResolver decoupledresolver =
        new StandardDecoupledTemplateLogicResolver();
decoupledResolver.setPrefix("../viewlogic/");
...
templateEngine.setDecoupledTemplateLogicResolver(decoupledResolver);
```
