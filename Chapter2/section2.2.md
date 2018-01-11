# 2.2 创建并配置模板引擎
filter中的process(…)方法包含这样的一行代码：
```
ITemplateEngine templateEngine = this.application.getTemplateEngine();
```
这行代码意味着GTVGApplication类负责创建和配置Thymeleaf应用中最重要的对象之一：TemplateEngine实例。`TemplateEngine`是`ITemplateEngine`的实现类。
`org.thymeleaf.TemplateEngine`对象初始化如下：

```
public class GTVGApplication {


    ...
    private final TemplateEngine templateEngine;
    ...


    public GTVGApplication(final ServletContext servletContext) {

        super();

        ServletContextTemplateResolver templateResolver =
                new ServletContextTemplateResolver(servletContext);

        //HTML是默认的模式，但是为了增强对代码的理解我们还是在代码中显式设置了模板模式
        templateResolver.setTemplateMode(TemplateMode.HTML);
        //将"home"路径转化为"/WEB-INF/templates/home.html"
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        // 模板缓存 TTL=1h。如果不显式设置，条目会被一直缓存直到被LRU去除
        templateResolver.setCacheTTLMs(Long.valueOf(3600000L));

        // 缓存被默认设置为true，
        //如果你希望模板在修改后能够自动更新，则将其设置为false
        templateResolver.setCacheable(true);

        this.templateEngine = new TemplateEngine();
        this.templateEngine.setTemplateResolver(templateResolver);

        ...

    }

}
```
配置`TemplateEngine`对象有许多办法，但是目前这几行代码足够我们进行后续步骤。

### 模板解析器（The Template Resolver） ###
---------------------------------------
让我们从模板解析器开始。
```
ServletContextTemplateResolver templateResolver =
        new ServletContextTemplateResolver(servletContext);
```
模板解析器是一个实现了Thymeleaf API中`org.thymeleaf.templateresolver.ITemplateResolver`接口的一个实现类。
```
public interface ITemplateResolver {

    ...

     /*
     * 通过模板的名称(或内容)
     * 和其拥有者的模板(当我们试图解析另一个模板中的一段代码)进行解析
     * 如果模板无法被解析，返回null
     */
    public TemplateResolution resolveTemplate(
            final IEngineConfiguration configuration,
            final String ownerTemplate, final String template,
            final Map<String, Object> templateResolutionAttributes);
}
```
这些对象负责决定如何获取我们的模板。在GTVG程序中，`org.thymeleaf.templateresolver.ServletContextTemplateResolver`意味着我们将从servlet上下文(一个应用级的上下文对象，存在于每一个JAVA WEB应用中)中获取我们的的模板文件，并以web应用的根目录作为起点解析资源。

但这并不是模板解析器的全部功能，它还能用于设置配置参数。
首先，设置模板模式(template mode)
```
templateResolver.setTemplateMode(TemplateMode.HTML);
```
HTML是`ServletContextTemplateResolver`的默认模板模式，但是显式的声明它有助于使代码显式的记录正在进行的工作。
```
templateResolver.setPrefix("/WEB-INF/templates/");
templateResolver.setSuffix(".html");
```
*prefix* 和 *suffix* 修改了传给处理器用于获取真实资源的的模板的名称。
通过这个配置，名为 *“product/list”* 的模板会被解析为：
```
servletContext.getResourceAsStream("/WEB-INF/templates/product/list.html")
```
你还能选择性地配置一个解析后的模板在缓存中存活的时间，通过配置模板解析器的 *cacheTTLMs* 属性：
```
templateResolver.setCacheTTLMs(3600000L);
```
当缓存满了，并且当前的模板是最早进入缓存的，那么这个模板也可能在TTL到达之前被丢出缓存。
>用户可以通过实现`ICacheManager`接口或修改`StandardCacheManager`对象来管理缓存的行为和大小。

关于模板处理器的知识还有许多需要了解，但是现在我们看一下刚刚创建的`Template Engine`对象。

### 模板引擎(The Template Engine)###
模板引擎对象是`org.thymeleaf.ITemplateEngine`接口的实现类。Thymeleaf核心库提供了它的一个实现`org.thymeleaf.TemplateEngine`，我们在这里创建这个对象的一个实例。
```
templateEngine = new TemplateEngine();
templateEngine.setTemplateResolver(templateResolver);
```
是不是很简单呢？我们所要做的一切就是创建一个实例并且设置它的模板解析器。

这里`TemplateEngine`需要的唯一参数就是模板解析器，尽管后面还会介绍许多其他可以设置的参数(消息解析器message resolvers，缓存大小等)。目前我们只需要知道这么多。

我们的模板引擎已经准备就休，现在我们可以开始用Thymeleaf创建页面。
