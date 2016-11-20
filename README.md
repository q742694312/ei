# ThinkPHP3.2.3 multi user blog

#[http://ei.lenggirl.com](http://ei.lenggirl.com "中小企业智能展示平台")


# 使用说明

1.使用前先将sql导入数据库，改下数据库配置，配置文件在Application/Common/Conf/config.php

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
		                                  
		// 数据库表前缀URL访问模式,可选参数0、1、2、3,代表以下四种模式：
		                                  // 0 (普通模式); 1 (PATHINFO 模式); 2 (REWRITE 模式); 3 (兼容模式) 默认为PATHINFO 模式
		'URL_MODEL' => '2',
		
		'USER_AUTH_KEY' => 'userid', // 用户会话标识
		'NEEDLOGIN' => 1, // 1表示登陆开启，其他表示关闭
		'ADMIN_AUTH_KEY' => array (
				'hunterhug' 
		), // 超级管理员
		//首页展示者
		'HOMEUSER'=>'hunterhug',
		'REGISTER' => 1,
		//'SHOW_PAGE_TRACE'=>true
	//	'DB_2' => 'mysql://root:6833066@localhost:3306/qingmu'
		
)
;
```

2.接着装Nginx,将以下配置放进Nginx配置目录中

进入 /usr/local/nginx/conf

```
# vim nginx.conf  
```

并且nginx.conf最后增加

```
include sites/*.conf;
```

新建sites文件夹，在sites文件夹中放入：

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

Nginx重启

```
/usr/local/nginx# sbin/nginx -s reload
```

3.然后URL改写，在根目录放.htacess(如果是apache服务器的话.htacess是不一样的，nginx的请见根目录下nginx.htacess文件。

nginx重写规则

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

apache重写规则

```
```

服务器安装可能要开启PHP重写等模块，有问题可以star咨询

启动PHP所需模块，注意还有其他如ssl等模块也要开启，百度！

```
/etc/init.d/php5-fpm restart
```

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
