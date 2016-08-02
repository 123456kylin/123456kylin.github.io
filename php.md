# PHP
```
this   是指向当前对象的指针
self   是指向当前类的指针
parent 是指向父类的指针

pdo 一个已经封装好的类，访问数据库，不直接使用pdo类，重新新建类包装
php console server:run   启动symfony框架插件
php_sapi_name() 判断php文件是以何种方式运行，命令行或者其他

sha1   计算字符串的散列,用于产生随机字符

 CDATA 指的是不由 XML 解析器进行解析的文本数据。 
 CDATA的形式如下：<![CDATA[文本内容]]>   文本内容不解析

$stmt->errorInfo()   查看数据库语句执行错误

mktime  计算得到时间戳
```

上传文件
```
var_dump($_FILES);
move_uploaded_file($_FILES['fname']['tmp_name'],"F:/".$_FILES['fname']['name']);

```

PDO数据库写法
```
采用预处理占位写法
$sql = <<<EOF
select * from table_name where id=:id
EOF;
$stmt -> prepare($sql);
$stmt -> bindParam(":id", $id);
$stmt -> execute();

```
