### PHP中提供了哪些内置数组，各代表什么意思？自己编码实验
#### $_GET
通过 URL 参数传递给当前脚本的变量的数组。
$HTTP_GET_VARS 包含相同的信息， 但它不是一个超全局变量。 (注意 $HTTP_GET_VARS 和 $_GET 是不同的变量，PHP 处理它们的方式不同)
```
<?php
echo 'Hello ' . htmlspecialchars($_GET["name"]) . '!';
?>
```

#### $_POST
当 HTTP POST 请求的 Content-Type 是 application/x-www-form-urlencoded 或 multipart/form-data 时，会将变量以关联数组形式传入当前脚本。
```
<?php
echo 'Hello ' . htmlspecialchars($_POST["name"]) . '!';
?>
```

#### $_SERVER
$_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。不能保证每个服务器都提供全部项目；服务器可能会忽略一些，或者提供一些没有在这里列举出来的项目。
```
<?php
echo $_SERVER['SERVER_NAME'];
?>
```

#### $_SESSION
当前脚本可用 SESSION 变量的数组。
```
<?php
session_start();
/*session is started if you don't write this line can't use $_Session  global variable*/
$_SESSION["newsession"]=$value;
/*session created*/
echo $_SESSION["newsession"];
/*session was getting*/
?>
```

#### $_COOKIE
通过 HTTP Cookies 方式传递给当前脚本的变量的数组。
```
<?php
echo 'Hello ' . htmlspecialchars($_COOKIE["name"]) . '!';
?>
```

#### $_FILES
通过 HTTP POST 方式上传到当前脚本的项目的数组。
```
<html>
<head></head>
<body></body>
<form enctype="multipart/form-data" action="file.php" method="POST">
    Send this file: <input name="userfile" type="file" />
    <input type="submit" value="Send File" />
</form>
</html>
<?php
echo "<pre>";
print_r($_FILES);
?>
```

#### $_ENV
通过环境方式传递给当前脚本的变量的数组。
这些变量被从 PHP 解析器的运行环境导入到 PHP 的全局命名空间。很多是由支持 PHP 运行的 Shell 提供的，并且不同的系统很可能运行着不同种类的 Shell，所以不可能有一份确定的列表。请查看你的 Shell 文档来获取定义的环境变量列表。
其他环境变量包含了 CGI 变量，而不管 PHP 是以服务器模块还是 CGI 处理器的方式运行。
```
<?php
echo 'My username is ' .$_ENV["USER"] . '!';
?>
```

#### $_REQUEST
默认情况下包含了 $_GET，$_POST 和 $_COOKIE 的数组。
```
<?php
    error_reporting(0);
    setcookie("value", "cookie", time()+3600);
    $_REQ = array_merge($_GET, $_POST);
    echo "===GET===<br />";
    echo $_GET['value'] . "<br />";
    echo "===POST===<br />";
    echo $_POST['value'] . "<br />";
    echo "===COOKIE===<br />";
    echo $_COOKIE['value'] . "<br />";
    echo "===REQUEST===<br />";
    echo $_REQ['value'] . "<br />";
?>
<form method="post" action="?value=get">
  <input type="text" name="value" value="post"/>
  <input type="submit" value="submit"/>
</form>
```
#### $GLOBALS
一个包含了全部变量的全局组合数组。变量的名字就是数组的键。
```
<?php
function test() {
    $foo = "local variable";

    echo '$foo in global scope: ' . $GLOBALS["foo"] . "\n";
    echo '$foo in current scope: ' . $foo . "\n";
}

$foo = "Example content";
test();
?>
```

### PHP中实现网页间数据传递，可以使用哪几种方法？
1.SESSION方式：
通过$_SESSION['name'] = value 赋值，就可以在session的生命周期内一直调用该session值。
需要注意的是‘name’需要唯一，不能重复。
session的一个生命周期：从用户访问页面开始，到断开与网站的链接为止。
用session存储变量的问题是在之前已经打开页面展示seesion数据的情况下，若新打开一个页面，则之前那个页面的session相关赋值会同步更新，
这在一些情况下并不是想要达到的效果。不过也因此SESSION适合用来记录用户的在线时间。
2.GET方式
传递的参数链接在目标url的后面，这边需要注意的是url的两种形式----路由形式和路径形式get传值的不同：
路径形式的get传值方式：path/index.php/?id=7878&a=234234
路由形式的get传值方式：path?r=index.php?id =11&b = 88
在目标页面用$_GET就能获取所有的get参数
之前一直只知道路由形式的方式，路径形式方式不知道是啥样，然后在想用其他三种方式传值，虽然都能实现但有些繁琐。然后一筹莫展之时看到了一个url就是路径形式GET传值方式。。。。看来还是书读得少。。。。
3.POST方式
form表单提交action指定跳转url;
js的
document.formname.action = 'url';
document.formname.submit(); //formname为表单名字
4.数据表方式
最繁琐的方式，将页面内容存入数据表，再在另外一个页面从表中将特定记录取出。  

### PHP中Cookie和Session都可以实现用户登陆，他们有什么区别？
COOKIE和SESSION的区别
        （1）存储位置：Cookie存储在客户端浏览器中，相对不安全；Session内容所在文件存储在服务器中，一般在根目录下的tmp文件夹中，相对更安全。
        （2）数量和大小限制：Cookie存储的数据在不同的浏览器会有不同的限制，一般在同一个域名下，Cookie变量数量控制在20个以内，每个cookie值的大小控制在4kb以内。session值没有大小和数量限制，但如果数量过多，会增大服务器的压力。
        （3）内容区别：cookie保存的内容是字符串，而服务器中的session保存的数据是对象。
        （4）路径区别：session不能区分路径，同一个用户在访问一个网站期间，所有的session在任何一个地方都可以访问到；而cookie中如果设置了路径参数，那么同一个网站中不同路径下的cookie互相是访问不到的。
```
<?php
	// 增加 Cookie
	// 设置Cookie函数法 setCookie();
	setCookie("test1","test1");
	// 设置Cookie赋值法
	$_COOKIE['test2'] = "test2";
	// 修改Cookie
	// 修改Cookie setCookie()
	setCookie("test1","test1->test3");
	// 修改Cookie赋值法
	$_COOKIE["test2"] = "test2->test4";
	// 查询所有Cookie值
	var_dump($_COOKIE);
	// 查询键值为test2的值
	var_dump($_COOKIE['test2']);
	// 删除键值为test1的值
	setCookie("test1","",time()-1);
	// 设置test2值为空
	$_COOKIE['test2'] = "";
	var_dump($_COOKIE);
  ?>
  ```
  ```
  <?php
	// 开启session
	session_start();
	// 增加session
	$_SESSION['test'] = "session增加内容";
	$_SESSION['test1'] = "session增加内容";
	// var_dump($_SESSION['test']);
	// 修改session
	$_SESSION['test'] = "session修改内容";
	// var_dump($_SESSION['test']);
	// 查看全部session
	var_dump($_SESSION);
	// 单独查看键值为test的Session值
	var_dump($_SESSION['test']);
	// 删除session
	// 删除session的变量值(但保留数据类型)
	// $_SESSION['test'] = array();
	// 删除所有的SESSION（包括文件）一般用于退出操作
	// 1. 使客户端COOKIE中的SESSIONID过期
	// $session_id = Session_name();
	// setcookie($session_id,"",time()-1);
	// 2. 释放当前在内存中已经创建的所有$_SESSION变量，但不删除session文件以及不释放对应的session_id
	// session_unset();
	// 3. 删除当前用户对应的session文件以及释放session_id，但内存中的$_SESSION变量内容依然保留
	session_destroy();
	var_dump($_SESSION);
?>
```

### PHP中我们可以通过内置数组获取哪些服务器信息，如何获取？
获取系统类型及版本号：    php_uname()     (例：Windows NT COMPUTER 5.1 build 2600)
只获取系统类型：          php_uname('s')       (或：PHP_OS，例：Windows NT)
只获取系统版本号：        php_uname('r')       
获取PHP运行方式：      php_sapi_name()       (PHP run mode：apache2handler)
获取前进程用户名：        Get_Current_User()
获取PHP版本：          PHP_VERSION
获取Zend版本：          Zend_Version()
获取PHP安装路径：      DEFAULT_INCLUDE_PATH
获取当前文件绝对路径：    __FILE__
获取Http请求中Host值：    $_SERVER["HTTP_HOST"]          (返回值为域名或IP)
获取服务器IP：            GetHostByName($_SERVER['SERVER_NAME'])
接受请求的服务器IP：      $_SERVER["SERVER_ADDR"]  
 (有时候获取不到，推荐用：GetHostByName($_SERVER['SERVER_NAME']))
获取客户端IP：            $_SERVER['REMOTE_ADDR']
获取服务器解译引擎：      $_SERVER['SERVER_SOFTWARE']
获取服务器CPU数量：      $_SERVER['PROCESSOR_IDENTIFIER']
获取服务器系统目录：      $_SERVER['SystemRoot']
获取服务器域名：$_SERVER['SERVER_NAME']   (建议使用：$_SERVER["HTTP_HOST"])
获取用户域名：            $_SERVER['USERDOMAIN']
获取服务器语言：          $_SERVER['HTTP_ACCEPT_LANGUAGE']
获取服务器Web端口：      $_SERVER['SERVER_PORT']


