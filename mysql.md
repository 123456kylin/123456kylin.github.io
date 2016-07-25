# MySQL

```
启动MySQL：进入MySQL路径，首先启动mysqld.exe;然后启动mysql.exe ,分别打开不同的命令行。
show warnings   查看警告
mysql -u root -p -h host
```

 like 
```
select * from tablename where name like '%g' 选出所有名字以g结尾信息
```
通配符
```
%  替代一个或多个字符
_   替代一个字符
```
in  只选出符合条件的行
```
SELECT \* FROM Persons WHERE LastName IN \('Adams','Carter'\)  选出lastname为adams和carter的人

```

between and 选出介于两个值之间的数据
```
select * from persons where age between 15 and 30
```

join  根据两个或多个表查询数据
```
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons, Orders WHERE Persons.Id_P = Orders.Id_P      查询谁订购了产品，订购了什么产品
```
