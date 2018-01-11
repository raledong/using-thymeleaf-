# 2.1 一个杂货铺网页

为了更好的解释用Thymeleaf处理模板过程中设计的概念，这个教程将会使用一个范例程序，你可以从项目网站上下载这个程序的源代码。

这是一个想象中的虚拟杂货店的网页程序，这个程序中会包含很多场景来展示Thymeleaf的功能。

首先，我们的程序需要一组简单的实体：通过创建`订单Orders`销售给`顾客Customers`的`商品Products`。我们还需要管理这些商品的`评论Comments`。*实体关系图*如下：

![实体关系图](http://www.thymeleaf.org/doc/tutorials/3.0/images/usingthymeleaf/gtvg-model.png)

我们的程序还将会包含一个非常简单的逻辑层(service layer)，逻辑层的对象包含的方法如下：
```
public class ProductService {

    ...

    public List<Product> findAll() {
        return ProductRepository.getInstance().findAll();
    }

    public Product findById(Integer id) {
        return ProductRepository.getInstance().findById(id);
    }

}
```

在web层，我们的应用程序将有一个过滤器，根据请求URL将执行的任务委托给启用了Thymeleaf的命令。

```
private boolean process(HttpServletRequest request, HttpServletResponse response)
        throws ServletException {

    try {
        //防止访问系统资源的URL触发Thymeleaf引擎
        if (request.getRequestURI().startsWith("/css") ||
                request.getRequestURI().startsWith("/images") ||
                request.getRequestURI().startsWith("/favicon")) {
            return false;
        }


        /*
         * 查询控制器/URL映射并获取处理请求的控制器。
         * 如果没有可用的controller
         * 返回false并且让其他的filters/servlets处理请求
         */
        IGTVGController controller = this.application.resolveControllerForRequest(request);
        if (controller == null) {
            return false;
        }

        /*
         * 获取TemplateEngine实例
         */
        ITemplateEngine templateEngine = this.application.getTemplateEngine();

        /*
         * 写响应头
         */
        response.setContentType("text/html;charset=UTF-8");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("Cache-Control", "no-cache");
        response.setDateHeader("Expires", 0);

        /*
         * 运行controller并处理试图模板，
         * 将结果写入响应
         */
        controller.process(
                request, response, this.servletContext, templateEngine);

        return true;

    } catch (Exception e) {
        try {
            response.sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR);
        } catch (final IOException ignored) {
            // Just ignore this
        }
        throw new ServletException(e);
    }

}
```
下面是`IGTVGController`接口。
```
public interface IGTVGController {

    public void process(
            HttpServletRequest request, HttpServletResponse response,
            ServletContext servletContext, ITemplateEngine templateEngine);    

}
```
我们所要做的就是编写`IGTVGController`接口的实现，从services中获取数据并用`ITemplateEngine`处理模板。
这个程序的主页如下：
![程序主页](http://www.thymeleaf.org/doc/tutorials/3.0/images/usingthymeleaf/gtvg-view.png)

首先，让我们看一看模板引擎是如何初始化的。
