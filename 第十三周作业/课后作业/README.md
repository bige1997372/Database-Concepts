#### Apache配置检查
![image]()
证明语法正确，配置无问题。
#### 编码并测试运行课件中留言板的功能代码，并熟练理解代码原理。
建库建表
```
CREATE DATABASE `gbook` ;
/* 创建表admin：*/
CREATE TABLE `admin` (
`username` VARCHAR( 20 ) NOT NULL ,
`userpass` VARCHAR( 20 ) NOT NULL 
)
/* 创建表message：*/
CREATE TABLE `message` (
`id` INT( 4 ) NOT NULL AUTO_INCREMENT PRIMARY KEY ,
`author` VARCHAR( 20 ) NOT NULL ,
`addtime` DATETIME NOT NULL ,
`content` VARCHAR( 1000 ) NOT NULL ,
`reply` VARCHAR( 1000 ) NOT NULL 
)
```
html，php代码
```
<html>
	<head>
		
	</head>
	<body>
		<form action="listmsg.php" method="post" name = "myform" onsubmit="return CheckPost();">
		用名:<input type="text" size="10" name="userName" /><br/> 
		标题:<input type="text" name="title" /><br/> 
		内容:<textarea name="content" cols="60" rows="9" ></textarea><br/> 
		<input type="submit" name="submit" value="提交留言" /> 
		</form> 
	</body>
</html>
```
```
<span style="font-family:Comic Sans MS;font-size:14px;"><?php 
include("index.php"); 
$dbName = "bbs"; 
$conn = @ mysqli_connect("localhost", "root", "") or die("数据库链接错误"); 
$flag = mysqli_select_db($conn, "gbook"); 
mysqli_query($conn,"set names 'GBK'"); //使用GBK中文编码; 
function toHtmlcode($content) 
{ 
return $content = str_replace("\n","<br>",str_replace(" ", "&nbsp;", $content)); 
} ;
?></span>
```
```
<html>
	<head>
		
	</head>
	<body>
		<form action="listmsg.php" method="post" name = "myform" onsubmit="return CheckPost();">
		用名:<input type="text" size="10" name="userName" /><br/> 
		标题:<input type="text" name="title" /><br/> 
		内容:<textarea name="content" cols="60" rows="9" ></textarea><br/> 
		<input type="submit" name="submit" value="提交留言" /> 
		</form> 
	</body>
</html>
<span style="font-family:Comic Sans MS;font-size:14px;"><?php 
// 引用之前写好的连接数据库文件 
include("conn.php"); 
if(@$_POST['submit']){ 
$sql = "insert into message (id,user,title,content,lastdate)" . 
"values ( '','$_POST[userName]','$_POST[title]','$_POST[content]',now())"; 
mysqli_query($conn,$sql); 
echo "添加成功"; 
} 
?> 
<SCRIPT language=javascript> 
function CheckPost() 
{ 
if (myform.userName.value=="") 
{ 
alert("请填写用户名"); 
myform.user.focus(); 
return false; 
} 
if (myform.title.value.length<5) 
{ 
alert("标题不能少于5个字符"); 
myform.title.focus(); 
return false; 
} 
if (myform.content.value=="") 
{ 
alert("必须要填写留言内容"); 
myform.content.focus(); 
return false; 
} 
} 
</SCRIPT> 
</span>
<span style="font-family:Comic Sans MS;font-size:14px;"><?php 
?> 
<table width=500 border="0" align="center" cellpadding="5" cellspacing="1" bgcolor="#add3ef"> 
<?php 
$sql = "SELECT * FROM message order by lastdate desc"; 
$query = mysqli_query($conn,$sql); 
while($row = mysqli_fetch_array($query,MYSQLI_ASSOC)){ 
?> 
<tr bgcolor="#eff3ff"> 
<td><b><big> 
标题：<?= $row['title']?></big><b/> <b><sub> 
用户：<?= $row['user']?></sub></b></td> 
</tr> 
<tr bgColor="#ffffff"> 
<td>内容：<?= toHtmlcode($row['content'])?></td> 
</tr> 
<?php 
} 
?> 
</table> 
</span>
```


