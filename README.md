# 2016.4
# 请查看PPT文件!
使用先将sql导入数据库，改下数据库配置

接着装Nginx,将配置放进去

然后URL改写

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
<p>黄页是一种以提供最直接联系信息的方式，是沟通企业与消费者的媒体。19世纪末诞生于美国，由于电话号簿白色印刷纸张不够，而临时采用黄色，至此沿用。信息时代的来临，以及20世纪末万维网的诞生，出现了相关的互联网黄页。网络传播的的方便性和快捷性无疑为企业带来了福音，然而参差不齐的互联网展示方式，或者依托第三方平台，或者自主建立网站，都是一种费钱费力费时的方式。于是，一个新的想法出现了：新型互联网黄页——企业智能展示平台。</p>
<p>互联网+口号的喊出，以及政府加大对传统企业的信息化建设力度，一个依托互联网的新型企业展示平台很有必要出现。更多的中小传统企业必要也必须加大信息化建设，更亲密的和互联网进行融合与互动，借助网络平台扩大影响力，转型升级。与其花大量金钱和时间外包建站，不如统一设计和建造一个平台。
</p>
<p>通过设计和建造统一的互联网展示平台，使得中小企业只需注册用户，然后进行简单的相关配置，就能通过网络进行自身展示，避免了自主建设网站产生的各种不必要麻烦。
</p>
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem1.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem2.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem3.jpg' />
<img src='https://raw.githubusercontent.com/hunterhug/ei/master/seem4.jpg' />
#[http://www.lenggirl.com](http://www.lenggirl.com "中小企业智能展示平台")
