# 18.附录A 表达式基本对象
### 基本对象 ###
---------------------------------------
* `#ctx`：上下文对象，根据环境（独立或是web）变为`org.thymeleaf.context.IContext`或`org.thymeleaf.context.IWebContext`实现。

注意`#vars`和`#root`和它是同样的对象，但是推荐使用`#ctx`。
```
/*
 * ======================================================================
 * See javadoc API for class org.thymeleaf.context.IContext
 * ======================================================================
 */

${#ctx.locale}
${#ctx.variableNames}

/*
 * ======================================================================
 * See javadoc API for class org.thymeleaf.context.IWebContext
 * ======================================================================
 */

${#ctx.request}
${#ctx.response}
${#ctx.session}
${#ctx.servletContext}
```

* `#locale`：直接获得和当前请求相关联的`java.util.Locale`对象。
```
${#locale}
```

### 请求/session属性等Web上下文命名空间###
---------------------------------------
在web环境中使用Thymeleaf是，我们可以使用一系列快捷方式来获取请求参数，session属性和应用属性：
> 注意，这些不是上下文对象，而是映射到上下文中作为变量的map。所以我们无需通过`#`获取它们。某种程度上，它们更像是命名空间。

* `param`：用于获取请求参数。`${param.foo}`是`String[]`，该数组中包含`foo`请求参数的值。因此`${param.foo[0]}`通常用来获取它的第一个值。
```
/*
 * ============================================================================
 * See javadoc API for class org.thymeleaf.context.WebRequestParamsVariablesMap
 * ============================================================================
 */

${param.foo}              // Retrieves a String[] with the values of request parameter 'foo'
${param.size()}
${param.isEmpty()}
${param.containsKey('foo')}
...
```
* `session`：用于获取session属性
```
/*
 * ======================================================================
 * See javadoc API for class org.thymeleaf.context.WebSessionVariablesMap
 * ======================================================================
 */

${session.foo}                 // Retrieves the session atttribute 'foo'
${session.size()}
${session.isEmpty()}
${session.containsKey('foo')}
...
```
* `application`：用于获取application/servlet上下文属性。
```
/*
 * =============================================================================
 * See javadoc API for class org.thymeleaf.context.WebServletContextVariablesMap
 * =============================================================================
 */

${application.foo}              // Retrieves the ServletContext attribute 'foo'
${application.size()}
${application.isEmpty()}
${application.containsKey('foo')}
...
```
注意获取请求属性（不同于请求参数）无需指定命名空间，因为所有的请求属性会被自动当做变量添加到上下文中。
```
${myRequestAttribute}
```

### Web上下文对象###
---------------------------------------
在web环境下可以直接获取下列对象（注意是对象，而不是maps/namespaces）：
* `#request`：直接获得和当前请求相关的`javax.servlet.http.HttpServletRequest`对象。
```
${#request.getAttribute('foo')}
${#request.getParameter('foo')}
${#request.getContextPath()}
${#request.getRequestName()}
...
```

* `#session`：直接获得和当前请求相关的`javax.servlet.http.HttpSession`对象
```
${#session.getAttribute('foo')}
${#session.id}
${#session.lastAccessedTime}
...
```
* `#servletContext` : 直接获得和当前请求相关的`javax.servlet.ServletContext`对象
```
${#servletContext.getAttribute('foo')}
${#servletContext.contextPath}
...
```
