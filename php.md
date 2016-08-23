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

php对小数处理
intval()   丢弃小数部分，保留整数部分
ceil()     向上取整，有小数就向上加1
round()    四舍五入
floor()    向下取整

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
simplexml_load_string()  把xml字符串载入对象
例：
 <?php
       $data=<<<EOF
       <data>
       <para><note>simple note</note></para>
       <molecule>
               <name>Alanine</name>
               <symbol>ala</symbol>
               <code>A</code>
               <type>hydrophobic</type>
      </molecule>
      </data>
      EOF; 
      $a = simplexml_load_string($data, 'SimpleXMLElement', LIBXML_NOCDATA);

      var_dump(json_decode(json_encode($a),true));

输出结果：
array(2) {
  ["para"]=>
  array(1) {
    ["note"]=>
    string(11) "simple note"
  }
  ["molecule"]=>
  array(4) {
    ["name"]=>
    string(7) "Alanine"
    ["symbol"]=>
    string(3) "ala"
    ["code"]=>
    string(1) "A"
    ["type"]=>
    string(11) "hydrophobic"
  }
}

```

###curl get
```
 $ch = curl_init();    //初始化
 curl_setopt($ch, CURLOPT, $url);
 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
 curl_setopt($ch, CURLOPT_HEADER, 0);
 $data = curl_exex($ch);   //执行操作
 curl_close($ch);
```

###curl post
```
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $BookUrl);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_REFERER, "http://www.sina.com.cn");
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Expect:'));
curl_setopt($ch, CURLOPT_ENCODING, 'UTF-8');
curl_setopt($ch, CURLOPT_POST, 1);
// 把post的变量加上
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
$output = curl_exec($ch);
curl_close($ch);

```

####编码类型转换
```
mb_convert_encoding($str,$to_encoding,$from_encoding)
参数：
str   要编码的string
to_encoding    str要转换成都编码
from_encoding   str转换前的编码
```


