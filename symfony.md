# symfony

安装symfony

```
下载symfony
 c:\> php -r "file_put_contents('symfony',file_get_contents('https://symfony.com/installer'));" 

安装symfony
php symfony new project_name

进入工程目录

php bin/console server:run   启动symfony    默认路由是127.0.0.1:8000或者localhost:8000

php bin/console generate:bundle  创建一个bundle

php bin/console generate:controller 创建controller

```

symfony写法

```
annotation 注释路由写法：

必须以/**开始    以*/结束 中间为路由 
例：  /** * @Route("/", name="homepage") */ 
@Route()中的第一个值定义了匹配的URL，用来触发后续操作。当你没有在URL中添加程序的域名信息时（比如http://example.com），这些URL始终是相对的，并被称为路径（paths）。
本例中，/路径指代程序的首页。@Route()的第二个参数（比如name=”homepage”）是可选项，设置了该路由的名字。此处这个名字并不是必须项，但以后它会非常有用，因为在链接页面时要用到 

```

twig

```
twig 可以是任意类型的内容文件

{{ ... }} 输出变量内容，或执行表达式给出结果
{% ... %} 控制模板逻辑，比如，它常被用于执行for循环以及if声明等。
{# ... #} 在模板文件中包容注释内容。与HTML注释相反，twig注释不出现在渲染之后的模板中。

$this->render（）调用渲染模板函数

在模板中显示二维数组的结果
$this->render("xxxx.html.twig",array('name' => $user))
{{% for value in name%}}
<li>{{ value.id}} </li>
<li>{{ value.name}}</li>
...
{{ endfor}}

{{include（‘’）}} 嵌套模板
{{ include('ads/banner.html.twig') }}     
```

创建不同链接页面

```
{{ path('name')}}   name为路由名称(和注释路由中的路由名字一致)

 {{ asset('css/blog.css') }}     包容资源asset函数
```

数据库链接

```
配置信息路径   app/config/parameters.yml
链接数据库使用
$conn = $this->get('database_connection');
$users = $conn->fetchAll("SELECT * FROM jieqi_article_article where articleid=$articleid");

```

get  post数据

```
调用Requst类
public function indexAction(Request $request)
获取get数据（第一个参数为名称，第二个为默认值）
$request->query->get('foo'); 
获取post数据 （第一个参数为名称，第二个为默认值） 
$request->request->get('bar', 'default value if bar does not exist');
```

function

```
$this->generateUrl   获取url
```

管理session

```
public function indexAction(Request $request)
$session = $request->getSession()  获取session
// store an attribute for reuse during a later user request
$session->set('foo', 'bar');
// get the attribute set by another controller in another request
$foobar = $session->get('foobar');
// use a default value if the attribute doesn't exist
$filters = $session->get('filters', array());
```

管理cookie

```
   Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。 

，只需要新建一个同名的Cookie，并将maxAge设置为0，并添加到response中覆盖原来的Cookie。注意是0而不是负数。负数代表其他的意义。读者可以通过上例的程序进行验证，设置不同的属性。

注意：修改、删除Cookie时，新建的Cookie除value、maxAge之外的所有属性，例如name、path、domain等，都要与原Cookie完全一样。否则，浏览器将视为两个不同的Cookie不予覆盖，导致修改、删除失败。
```
