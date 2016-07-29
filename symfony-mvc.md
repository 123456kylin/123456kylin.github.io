symfony对request的response
```
$this->reponse
```

symfony路由
```
路由模式： annotations  注释路由模式
写法：
/**
 * @Route("/blog/{slug}", name="blog_show", defaults = {"slug" = 1}, requirements={"slug":"正则表达式"})
 */
Route 路由路径   
{slug} 参数名称
name  function的逻辑名字   它遵循规则指向一个特定的php类和方法,/blog下逻辑名称为blog_show的方法。
defaults 给占位符{slug} 赋默认值
requirements 参数的正则表达式
method  提交请求方式
例：
/**
 * @Route("/blog/{slug}", name="blog_show
 *@Method({"GET","POST"})
 */
public function showAction($slug)
{
// ...
}

路由在选填参数时，不会匹配带斜杠的请求（如/blog/不匹配，/blog匹配正确）。 



特殊路由参数
_controller
正如你所看到的，这个参数是用来匹配执行控制器的。
_format
用于设置请求格式。
_locale
用于设置请求域。

```