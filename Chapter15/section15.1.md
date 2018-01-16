# 15.1 模板解析器(Template Resolvers)
在Thymes虚拟杂货店上，我们选择了`ITemplateResolver`接口的实现类`ServletContextTemplateResolver`从Servlet上下文中获取资源。

Thymeleaf除了让我们通过实现`ITemplateResolver`接口来自定义模板解析器，它还自带了四种实现：
* `org.thymeleaf.templateresolver.ClassLoaderTemplateResolver`，它将模板解析为类加载器资源：
```
return Thread.currentThread().getContextClassLoader().getResourceAsStream(template);
```
* `org.thymeleaf.templateresolver.FileTemplateResolver`，将模板解析为文件系统的文件：
```
return new FileInputStream(new File(template));
```
* `org.thymeleaf.templateresolver.UrlTemplateResolver`，将模板解析为URL（包括非本地路径）：
```
return (new URL(template)).openStream();
```
* `org.thymeleaf.templateresolver.StringTemplateResolver`，将模板直接解析为String：
```
return new StringReader(templateName);
```
所有的`ITemplateResolver`预实现都允许相同的一组配置参数，包括：
* 前缀和后缀（之前已经看到了）
```
templateResolver.setPrefix("/WEB-INF/templates/");
templateResolver.setSuffix(".html");
```
* `模板别名`允许使用不是直接对应于文件名的模板名称。如果前缀/后缀和别名同时存在，别名会在前缀/后缀之前生效：
```
templateResolver.addTemplateAlias("adminHome","profiles/admin/home");
templateResolver.setTemplateAliases(aliasesMap);
```
* 读取模板所用的编码：
```
templateResolver.setEncoding("UTF-8");
```
* 要使用的模板模式：
```
// 默认为HTML
templateResolver.setTemplateMode("XML");
```
* 模板的默认缓存模式，以及特定模板是否进行缓存：
```
// 默认为true
templateResolver.setCacheable(false);
templateResolver.getCacheablePatternSpec().addPattern("/users/*");
```
* 模板解析器中解析后的模板缓存的过期时间。如果没有设置，删除缓存的唯一方法为LRU（达到缓存上限且条目为最久远的）。
```
//默认为无TTL（只有通过LRU删除条目）
templateResolver.setCacheTTLMs(60000L);
```
> Thymeleaf + Spring的继承包提供了`SpringResourceTemplateResolver`实现，它使用Spring的框架来读取应用中的资源，在开启Spring的应用中推荐使用这个实现。

### 连接模板解析器 ###
除此以外，一个模板引擎能够定义多个模板解析器，且可以定义各个模板解析器的处理顺序，这样的话，如果第一个模板解析器无法解析模板，则可以询问第二个解析器，依次往下：
```
ClassLoaderTemplateResolver classLoaderTemplateResolver = new ClassLoaderTemplateResolver();
classLoaderTemplateResolver.setOrder(Integer.valueOf(1));

ServletContextTemplateResolver servletContextTemplateResolver =
        new ServletContextTemplateResolver(servletContext);
servletContextTemplateResolver.setOrder(Integer.valueOf(2));

templateEngine.addTemplateResolver(classLoaderTemplateResolver);
templateEngine.addTemplateResolver(servletContextTemplateResolver);
```
当应用多个模板处理器时，建议为每个模板处理器都定义独特的模板模式，这样Thymeleaf能够快速的抛弃不属于该模板处理器解析的模板，从而提高性能。并非强制这么做，但是推荐使用：
```
ClassLoaderTemplateResolver classLoaderTemplateResolver = new ClassLoaderTemplateResolver();
classLoaderTemplateResolver.setOrder(Integer.valueOf(1));
// This classloader will not be even asked for any templates not matching these patterns
classLoaderTemplateResolver.getResolvablePatternSpec().addPattern("/layout/*.html");
classLoaderTemplateResolver.getResolvablePatternSpec().addPattern("/menu/*.html");

ServletContextTemplateResolver servletContextTemplateResolver =
        new ServletContextTemplateResolver(servletContext);
servletContextTemplateResolver.setOrder(Integer.valueOf(2));
```
如果没有指定 *可解析的路径风格*， 我们将会依赖于使用的`ITemplateResolver`实现类的特殊能力。注意不是所有的实现都能够在解析模板前判断该模板是否存在，因此它可能会认为该模板不可解析然后终止了解析链（也不允许别的解析器检查该模板），然后无法读取真实资源。

在核心Thymeleaf中的所有`ITemplateResolver`实现都允许在认定资源可解析之前判断该资源是否存在。这就是`checkExistence`标识，运行如下：
```
ClassLoaderTemplateResolver classLoaderTemplateResolver = new ClassLoaderTemplateResolver();
classLoaderTemplateResolver.setOrder(Integer.valueOf(1));
classLoaderTempalteResolver.setCheckExistence(true);
```
`checkExistence`标识强制解析器在解析时对资源的存在性进行检查（并且在存在性检查结果为false时允许调用后序的解析器）。尽管这听上去在所有场景中都很合适，但是在大多数场景下它意味着需要对资源进行两次访问（一次检查存在与否，另一次读取），在某些情景中存在潜在的性能问题，比如，基于URL的远程模板资源。但是这个性能问题可以通过使用模板缓存大幅度减轻（通过缓存，只需要在第一次解析时获取模板）。
