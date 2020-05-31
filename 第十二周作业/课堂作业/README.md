#### 查看服务器程序的配置文件
![image]()
#### 了解各种配置的基本信息，与课件对比
ServerRoot：apache的安装路径
Timeout：apache配置的超时时间
KeepAlive：长连接是开启或关闭
MaxKeepAliveRequests：长连接配置的最大连接数，如果KeepAlive是关闭状态的，这项就无效
KeepAliveTimeout：长连接超时时间
Listen：apache对外提供的端口号，默认是80
Include：包含哪些文件进来
DocumentRoot：apache的工程路径，项目部署的路径

设置alias实现目录别名访问
在 apache下面的httpd.conf文件中的末端添加如下内容：    
<IfModule alias_module>     
 Alias /phpmyadmin “E:/server/phpMyAdmin/”    
 <Directory “E:/server/phpMyAdmin/”>    
 AllowOverride  AuthConfig    
 Order allow,deny    
 Allow from all     
 </Directory>    
 </IfModule>    
 “E:/server /phpMyAdmin/”这个就是phpmyadmin路径了
 
 apache 基本vhost配置
 
1，用记事本打开apache目录下httpd文件(如：D:\wamp\bin\apache\apache2.2.8\conf)，找到如下模块

#Virtual hosts
#Include conf/extra/httpd-vhosts.conf
去掉前面的#，这样就开启了httpd-vhosts虚拟主机文件。这时候重启wamp环境，无法打开localhost，需要在httpd-vhosts.conf配置一下。

2，用记事本打开httpd-vhosts文件，配置好localhost虚拟主机，参照httpd-vhosts文件中实例，修改成如下：

<VirtualHost *:80>
ServerAdmin webmaster@dummy-host.localhost
DocumentRoot "D:\wamp\www"
ServerName localhost
ServerAlias localhost
ErrorLog "logs/dummy-host.localhost-error.log"
CustomLog "logs/dummy-host.localhost-access.log" common
</VirtualHost>
修改配置如下：
DocumentRoot 修改为本地wamp环境下的www目录(如：D:\wamp\www)
ServerName改为localhost

3，重启Apache，发现localhost可以正常打开，配置localhost比较简单。
