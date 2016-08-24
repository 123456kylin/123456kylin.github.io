##composer
php的一个依赖工具

依赖包查询网址
```
https://packagist.org/
```
composer中文文档
```
http://docs.phpcomposer.com
```
安装
下载composer.phar
```
curl -sS https://getcomposer.org/installer | php
```
检查composer是否正常工作 只需要通过php来执行PHAR：
```
php composer.phar
```

####项目安装

第一件事情（并且往往只需要做这一件事），你需要在 composer.json 文件中指定 require key 的值。你只需要简单的告诉 Composer 你的项目需要依赖哪些包。 
```
{ 
    "require": {
         "monolog/monolog": "1.0.*" 
    }
}
```

安装依赖包
```
php composer.phar install
```
执行install命令之后，会创建一个composer.lock文件到项目的根目录中
因为 install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。 

更新
```
php composer.phar update
```

####packagist
packagist 是 Composer 的主要资源库。 一个 Composer 的库基本上是一个包的源：记录了可以得到包的地方。Packagist 的目标是成为大家使用库资源的中央存储平台。这意味着你可以 require 那里的任何包。 




