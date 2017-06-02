# MULTI USER BLOG (ThinkPHP)


[http://ei.lenggirl.com](http://ei.lenggirl.com "中小企业智能展示平台")


# 使用说明

## 1.使用前先安装MYSQL(百度一堆)

a. Ubuntu安装

敲入以下命令按提示操作

```
sudo apt-get install mysql-server mysql-client
```

b. Windows安装


[https://yun.baidu.com/s/1hrF0QC8](https://yun.baidu.com/s/1hrF0QC8) 找到mysql文件夹下面的5.6.17.0.msi根据说明安装.


## 2.导入sql数据库

数据库 文件在根目录:lenggirl.sql

```
C:\Users\huterhug>mysql -uroot -p
Enter password: *********

mysql> create database tuzi;
mysql> use tuzi
mysql> source init.sql

```

### 3.改网站配置
改下数据库配置，配置文件在`Application/Common/Conf/config.php`,只需改以下

```
<?php
return array (
		// '配置项'=>'配置值'
		/* 1.数据库设置 */
		'DB_TYPE' => 'mysql', // 数据库类型
		'DB_HOST' => localhost, // 服务器地址
		'DB_NAME' => 'lenggirl', // 数据库名
		'DB_PREFIX' => 'think_',
		'DB_USER' => root, // 用户名
		'DB_PWD' => 6833066, // 密码
		'DB_PORT' => 3306, // 端口
		                                  

		'ADMIN_AUTH_KEY' => array (
				'hunterhug' 
		), // 超级管理员

		//首页展示者
		'HOMEUSER'=>'hunterhug',
		
)
;
```

### 4.装Apache+PHP模式（有问题）

仅仅支持Linux(ubuntu16.04示例）,Windows大法请自行安装

```
sudo apt-get install apache2
sudo apt-get install php7.0-cli
sudo apt-get install libapache2-mod-php7.0
sudo /etc/init.d/apache2 restart
```

Apache配置在`/etc/apache2`下。


进行配置，使其能找到网站的文件位置

```
sudo /etc/apache2/
sudo cd sites-available
sudo cp 000-default.conf ei.conf
sudo vim ei.conf
```

```
<VirtualHost ei.lenggirl.com:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /home/jinhan/book/ei

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/eierror.log
        CustomLog ${APACHE_LOG_DIR}/eiaccess.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

```

其中`/home/jinhan/book/ei`是这个项目文件位置，本机测试将`ei.lenggirl.com:80`改为`127.0.0.1:81`

启动：

```
sudo a2ensite ei.conf 
sudo service apache2 reload
```

启动后apache要重写规则。在项目根目录下的`.htaccess`即是(可忽略)。

```
<IfModule mod_rewrite.c>
  Options +FollowSymlinks
  RewriteEngine On

  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  #
  RewriteRule ^u/(.+)/page/(.+)/pageno/(.+)$ index.php/Ei/User/page?username=$1&id=$2&pageno=$3 [NC,QSA,PT,L]
  RewriteRule ^u/(.+)/page/(.+)$ index.php/Ei/User/page?username=$1&id=$2 [NC,QSA,PT,L]
  RewriteRule ^u/(.+)/article/(.+)$ index.php/Ei/User/article?username=$1&id=$2 [NC,QSA,PT,L]
  RewriteRule ^u/(.+)/product$ index.php/Ei/User/product?username=$1 [NC,QSA,PT,L]
  RewriteRule ^u/(.+)/product/(.*)/pageno/(.+) index.php/Ei/User/product?username=$1&id=$2&pageno=$3 [NC,QSA,PT,L]
  RewriteRule ^u/(.+)/product/(.+)$ index.php/Ei/User/product?username=$1&id=$2 [NC,QSA,PT,L] 
  RewriteRule ^u/(.+)/i/(.+)$ index.php/Ei/User/pc?username=$1&id=$2 [NC,QSA,PT,L] 
  RewriteRule ^u/(.+)/about/(.+)$ index.php/Ei/User/about?username=$1&id=$2 [NC,QSA,PT,L] 
  RewriteRule ^u/(.+)$ index.php/Ei/User/index?username=$1 [NC,QSA,PT,L]
  
  RewriteRule ^page/(.+)/pageno/(.+)$ index.php/Home/Index/page?id=$1&pageno=$2 [NC,QSA,PT,L]
  RewriteRule ^page/(.+)$ index.php/Home/Index/page?id=$1 [NC,QSA,PT,L]
  RewriteRule ^article/(.+)$ index.php/Home/Index/article?id=$1 [NC,QSA,PT,L]
  RewriteRule ^about/(.+)$ index.php/Home/Index/about?id=$1 [NC,QSA,PT,L]
  RewriteRule ^(Home|Admin|Ei)/?(.*)$ index.php/$1/$2 [NC,QSA,PT,L]
</IfModule>
```


### 5.装Nginx+PHP模式（有问题）
 
安装NGINX

进入 /usr/local/nginx/conf

```
vim nginx.conf
```

并且nginx.conf最后增加

```
include sites/*.conf;
```

新建sites文件夹，在sites文件夹中放入该项目下`ei-nginx.conf`文件：

```
server{
	listen 80;
	server_name ei.lenggirl.com;
	charset utf-8;
	access_log /data/logs/nginx/ei.lenggirl.com.log;
	#error_log /data/logs/nginx/www.lenggirl.com.err;
	root /data/www/php/ei;
	include /data/www/php/ei/ngnix.htaccess;
	location / {
        	#root   /data/www/php/ei;
		index index.html index.php;
		#访问路径的文件不存在则重写URL转交给ThinkPHP处理
		if (!-e $request_filename) {
			rewrite  ^/(.*)$  /index.php/$1  last;
			break;
		}
	}
	
	## Images and static content is treated different
	location ~* ^.+.(jpg|jpeg|gif|css|png|js|ico|xml)$ {
	
		access_log        off;
		expires           30d;
		#root /data/www/php/ei;
	 }

	location ~\.php/?.*$ {
   	##root        /data/www/php/ei;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        #加载Nginx默认"服务器环境变量"配置
        include        fastcgi.conf;
        
        #设置PATH_INFO并改写SCRIPT_FILENAME,SCRIPT_NAME服务器环境变量
        set $fastcgi_script_name2 $fastcgi_script_name;
        if ($fastcgi_script_name ~ "^(.+\.php)(/.+)$") {
            set $fastcgi_script_name2 $1;
            set $path_info $2;
        }
        fastcgi_param   PATH_INFO $path_info;
        fastcgi_param   SCRIPT_FILENAME   $document_root$fastcgi_script_name2;
        fastcgi_param   SCRIPT_NAME   $fastcgi_script_name2;        
	}

}
```


本机测试将`ei.lenggirl.com`改为`127.0.0.1`,`80`改为`81`

启动后apache要重写规则。在项目根目录下的`ngnix.htaccess`即是(可忽略)。

```
if (!-d $request_filename){
	set $rule_0 1$rule_0;
}
if (!-f $request_filename){
	set $rule_0 2$rule_0;
}
if ($rule_0 = "21"){
	rewrite ^/u/(.+)/page/(.+)/pageno/(.+)$ /index.php/Ei/User/page?username=$1&id=$2&pageno=$3 last;
}
	rewrite ^/u/(.+)/page/(.+)$ /index.php/Ei/User/page?username=$1&id=$2 last;
	rewrite ^/u/(.+)/article/(.+)$ /index.php/Ei/User/article?username=$1&id=$2 last;
	rewrite ^/u/(.+)/product$ /index.php/Ei/User/product?username=$1 last;
	rewrite ^/u/(.+)/product/(.*)/pageno/(.+) /index.php/Ei/User/product?username=$1&id=$2&pageno=$3 last;
	rewrite ^/u/(.+)/product/(.+)$ /index.php/Ei/User/product?username=$1&id=$2 last;
	rewrite ^/u/(.+)/i/(.+)$ /index.php/Ei/User/pc?username=$1&id=$2 last;
	rewrite ^/u/(.+)/about/(.+)$ /index.php/Ei/User/about?username=$1&id=$2 last;
	rewrite ^/u/(.+)$ /index.php/Ei/User/index?username=$1 last;
	rewrite ^/page/(.+)/pageno/(.+)$ /index.php/Home/Index/page?id=$1&pageno=$2 last;
	rewrite ^/page/(.+)$ /index.php/Home/Index/page?id=$1 last;
	rewrite ^/article/(.+)$ /index.php/Home/Index/article?id=$1 last;
	rewrite ^/about/(.+)$ /index.php/Home/Index/about?id=$1 last;
	rewrite ^/(Home|Admin|Ei)/?(.*)$ /index.php/$1/$2 last;

```

Nginx重启

```
/usr/local/nginx# sbin/nginx -s reload
```

### 6.打开浏览器

http://127.0.0.1:81

http://ei.lengirl.com

后台http://ei.lenggirl.com/admin/public/login.html

用户:hunterhug

密码:

# 其他

<p>黄页是一种以提供最直接联系信息的方式，是沟通企业与消费者的媒体。19世纪末诞生于美国，由于电话号簿白色印刷纸张不够，而临时采用黄色，至此沿用。信息时代的来临，以及20世纪末万维网的诞生，出现了相关的互联网黄页。网络传播的的方便性和快捷性无疑为企业带来了福音，然而参差不齐的互联网展示方式，或者依托第三方平台，或者自主建立网站，都是一种费钱费力费时的方式。于是，一个新的想法出现了：新型互联网黄页——企业智能展示平台。</p>
<p>互联网+口号的喊出，以及政府加大对传统企业的信息化建设力度，一个依托互联网的新型企业展示平台很有必要出现。更多的中小传统企业必要也必须加大信息化建设，更亲密的和互联网进行融合与互动，借助网络平台扩大影响力，转型升级。与其花大量金钱和时间外包建站，不如统一设计和建造一个平台。
</p>
<p>通过设计和建造统一的互联网展示平台，使得中小企业只需注册用户，然后进行简单的相关配置，就能通过网络进行自身展示，避免了自主建设网站产生的各种不必要麻烦。
</p>
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem1.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem2.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem3.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem4.jpg' />
