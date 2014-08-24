[官网地址](https://getcomposer.org/)

Composer 是PHP中用来管理依赖(dependency)关系的工具。你可以在自己的项目中声明所依赖的外部工具库(libraries)，Composer会帮你安装这些依赖的库文件。

###系统需求
PHP5.3.2+ 以上的环境来运行。有几个敏感的PHP设置和编译标志也是必需的，但安装程序会发出警告当存在任何不兼容的情况。

###安装
curl -sS https://getcomposer.org/installer|php [-- --install-dir=/DIR]

###使用
php /DIR/composer.phar

###实例：安装slim
Slim is a micro framework for PHP 5 that helps you quickly write simple yet powerful web applications and APIs.[中文文档](http://minimee.org/php/slim)

*在项目根目录中建立名为 composer.json 的文件

		{
		    "require": {
		        "slim/slim": "2.*"
		    }
		}

*在项目根目录通过composer进行安装

		php /DIR/composer.phar install
