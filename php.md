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

time（）  计算得到当前时间戳

UNIX时间戳转换为日期用函数： date() 
一般形式：date('Y-m-d H:i:s', 1156219870) 

 日期转换为UNIX时间戳用函数：strtotime() 
一般形式：strtotime('2010-03-24 08:15:42') 


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

php处理xml数据为数组

```
xml_parse_into_struct
```

php  抓取数据
```
 $ch = curl_init();    //初始化
 curl_setopt($ch, CURLOPT, $url);
 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
 curl_setopt($ch, CURLOPT_HEADER, 0);
 $data = curl_exex($ch);   //执行操作
 curl_close($ch);
```

