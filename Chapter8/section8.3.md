# 8.3 灵活的布局：不仅仅是片段插入
多亏 *片段表达式*，我们能够为片段指定非文本，数字或是bean对象，而是markup代码段作为参数。

这允许模板传入markup代码作为参数来丰富片段内容，形成一种非常灵活的模板布局机制。

注意下面的代码段中使用的`title`和`links`变量：
```
<head th:fragment="common_header(title,links)">

  <title th:replace="${title}">The awesome application</title>

  <!-- Common styles and scripts -->
  <link rel="stylesheet" type="text/css" media="all" th:href="@{/css/awesomeapp.css}">
  <link rel="shortcut icon" th:href="@{/images/favicon.ico}">
  <script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></script>

  <!--/* Per-page placeholder for additional links */-->
  <th:block th:replace="${links}" />

</head>
```
我们可以这样调用这个片段：
```
...
<head th:replace="base :: common_header(~{::title},~{::link})">

  <title>Awesome - Main</title>

  <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
  <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">

</head>
...
```
...结果中会使用调用方模板中的`<title>`和`<link>`标签来作为`title`和`links`变量的值，使得片段在插入的过程中完成自定义。
```
...
<head>

  <title>Awesome - Main</title>

  <!-- Common styles and scripts -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

  <link rel="stylesheet" href="/awe/css/bootstrap.min.css">
  <link rel="stylesheet" href="/awe/themes/smoothness/jquery-ui.css">

</head>
...
```

### 使用空片段 ###
---------------------------------------
一个特殊的片段表达式，空片段（`~{}`），能够用于说明无`markup`。让我们用前面的那个例子说明：
```
<head th:replace="base :: common_header(~{::title},~{})">

  <title>Awesome - Main</title>

</head>
...
```
注意，片段的第二个参数（`links`）被设置为空，因此没有内容会被写入`<th:block th:replace="${links}" />`。
```
...
<head>

  <title>Awesome - Main</title>

  <!-- Common styles and scripts -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

</head>
...
```

### 使用无操作标识 ###
---------------------------------------
无操作标识也能作为参数传递给片段，如果我们想让片段使用当前的markup作为默认值。我们再次用`common_header`举例：
```
...
<head th:replace="base :: common_header(_,~{::link})">

  <title>Awesome - Main</title>

  <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
  <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">

</head>
...
```
看到`title`参数（`common_header`片段的第一个参数）被设置为无操作标识（`_`），使这部分片段不执行任何操作（`title`=*no-operation*）。
```
 <title th:replace="${title}">The awesome application</title>
```
所以结果为：
```
...
<head>

  <title>The awesome application</title>

  <!-- Common styles and scripts -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

  <link rel="stylesheet" href="/awe/css/bootstrap.min.css">
  <link rel="stylesheet" href="/awe/themes/smoothness/jquery-ui.css">

</head>
...
```

### 高级条件性片段插入 ###
---------------------------------------
*空片段* 和 *无操作标识* 都允许我们用一种简单而且优雅的方式条件性的插入片段。

例如，我们可以通过这种方法，仅当用户是管理员的时候插入`common :: adminhead`片段，否则不插入任何内容（空片段）：
```
...
<div th:insert="${user.isAdmin()} ? ~{common :: adminhead} : ~{}">...</div>
...
```
而且，我们能够使用无操作标识来实现仅当满足一定条件时插入片段，否则不对markup进行修改。
```
...
<div th:insert="${user.isAdmin()} ? ~{common :: adminhead} : _">
    Welcome [[${user.name}]], click <a th:href="@{/support}">here</a> for help-desk support.
</div>
...
```
除此以外，如果我们已经配置了模板解析器来检查模板资源是否存在 -- 通过`checkExistence`标识 -- 我们可以将片段本身是否存在作为判断条件来执行默认操作：
```
...
<!-- The body of the <div> will be used if the "common :: salutation" fragment  -->
<!-- does not exist (or is empty).                                              -->
<div th:insert="~{common :: salutation} ?: _">
    Welcome [[${user.name}]], click <a th:href="@{/support}">here</a> for help-desk support.
</div>
...
```
