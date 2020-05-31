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
index.php
```
<HTML>
<HEAD>
<TITLE>欢迎使用简约不简单-我的留言本</TITLE>
<style type=text/css>
TD{
font-size:12px;
line-height:150%;
}
</style>
</HEAD>
<BODY>
<table border=1 cellspacing=0 cellspadding=0 style="border-collapse:collapse" align=center width=400 bordercolor=black height="382">
<tr><td height=100 bgcolor=#6C6C6C style="font-size:30px;line-height:30px" >
<font color=#ffffff face="黑体">简约不简单-我的留言本</font>
</td><tr>
<tr><td height=25>
&nbsp;<a href=send.php>[我要写留言]</a>&nbsp;&nbsp;<a href=login.php>[管理留言]</a>
</td></tr>
<tr><td height=200>
  <?php
   $id=mysql_connect("localhost","root","1234");
   mysql_select_db("gbook",$id);
   $query="select * from message ";
   $result=mysql_query($query,$id);
   if(mysql_num_rows($result)<1){
   echo "&nbsp;目前数据表中还没有任何留言！";
   }else{ //ELSE开始
     $totalnum=mysql_num_rows($result);
     $pagesize=10;
     $page=$_GET["page"];
     if($page==""){
       $page=1;
     }
     $begin=($page-1) * $pagesize;
     $totalpage=ceil($totalnum/$pagesize);

     echo "<table border=0 width=95%><tr><td>";
     $datanum=mysql_num_rows($result);
     echo "共有留言".$totalnum."条。";
     echo "每页".$pagesize."条，共".$totalpage."页<br>";
     
     for($j=1;$j<=$totalpage;$j++){
      echo "<a href=index.php?page=".$j.">[".$j."]&nbsp;</a>";
     }
     echo "<br>";
     
     $query="SELECT * FROM message  order by addtime desc limit $begin,$pagesize";
     $result=mysql_query($query,$id);  
     for($i=1;$i<=$datanum;$i++){
      $info=mysql_fetch_array($result,MYSQL_ASSOC);
      echo "->[".$info['author']."]于".$info['addtime']."说：<br>";
      echo "&nbsp;&nbsp;".$info['content']."<br>";
       if($info['reply']!=""){
       echo "<b>管理员回复：</b>".$info['reply']."<br>";
       }
      echo "<hr>";
      }
     echo "</table>";
   } //ELSE结束
   mysql_close($id);
  ?>
</td></tr>
<tr><td height=60 bgcolor=#6c6c6c align=center>
<font color=#FFFFFF>版权所有：我的工作室<br>E-mail:rznqp@163.com
</td></tr>
</table>
</body>
</html>
```
login.php
```
<?php
$name=$_POST["name"];
if($name!=""){
 $password=$_POST["password"];
 $id=mysql_connect("localhost","root","1234");
 mysql_select_db("gbook",$id);
 $query="select * from admin where username='$name'";
 $result=mysql_query($query,$id);
 if(mysql_num_rows($result)<1){
 echo "该用户不存在!请重新登陆!";
 }else{
 $info=mysql_fetch_array($result,MYSQL_ASSOC);
 if($info['userpass']!=$password){
 echo "密码输入错误!请重新登陆!";
 }else{
  session_start();
  $_SESSION["login"]="YES";
  echo "<script language=javascript>alert('登陆成功!');location.href='manage.php';</script>";
  exit;
   }
 }
 mysql_close($id);
}
?>
<HTML>
<HEAD>
<TITLE>欢迎使用简约不简单-我的留言本</TITLE>
</HEAD>
<BODY>
<table border=1 cellspacing=0 cellspadding=0 style="border-collapse:collapse" align=center width=400 bordercolor=black height="358">
<tr><td height=100 bgcolor=#6C6C6C>
<font style="font-size:30px" color=#ffffff face="黑体">简约不简单-我的留言本</font>
</td>
<tr><td height=25>
&nbsp;<a href=send.php>[我要写留言]</a>&nbsp;&nbsp;<a href=login.php>[管理留言]</a>
</td></tr>
<tr><td height=178>
  <form method="POST" action="login.php">
	<table border="1" width="95%" id="table1" cellspacing="0" cellpadding="0" bordercolor="#808080" style="border-collapse: collapse" height="154">
		<tr>
			<td colspan="2" height="29">
			<p align="center">欢迎管理员登陆</td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">用户名</td>
			<td width="67%"><input type="text" name="name" size="20"></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">密&nbsp; 码</td>
			<td width="67%"><input type="password" name="password" size="20"></td>
		</tr>
		<tr>
			<td width="99%" colspan="2">
				<p align="center"><input type="submit" value="登陆" name="B1"></p>		
			</td>
		</tr>
	</table>
	</form>
	</td></tr>
<tr><td height=58 bgcolor=#6c6c6c align=center>
<font color=#FFFFFF>版权所有：我的工作室<br>E-mail:rznqp@163.com
</td></tr>
</table>
</body>
</html>
```
logout.php
```
<?php
session_start();
$_SESSION["login"]="";
echo "已成功退出。[<a href=index.php>回首页</a>]";
exit;
?>
```
manage.php
```
<?php
session_start();
if($_SESSION["login"]!="YES"){
 echo "你还没有登陆,无法管理留言!";
 exit;
}

$delid=$_GET["delid"];
if($delid!=""){
 $id=mysql_connect("localhost","root","1234");
 mysql_select_db("gbook",$id);
 mysql_query("delete from message where id=".$delid);
 echo "<script language=javascript>alert('删除成功!');</script>";
 mysql_close($id);
}
?>
<HTML>
<HEAD>
<TITLE>欢迎使用简约不简单-我的留言本</TITLE>
<style type=text/css>
TD{
font-size:12px;
line-height:150%;
}
</style>
</HEAD>
<BODY>
<table border=1 cellspacing=0 cellspadding=0 style="border-collapse:collapse" align=center width=400 bordercolor=black height="382">
<tr><td height=100 bgcolor=#6C6C6C style="font-size:30px;line-height:30px" >
<font color=#ffffff face="黑体">简约不简单-我的留言本</font>
</td>
<tr><td height=25>
&nbsp;&nbsp;<a href=logout.php>[注销登陆]</a>
</td></tr>
<tr><td height=200>
  <?php
   $id=mysql_connect("localhost","root","1234");
   mysql_select_db("gbook",$id);
   $query="select * from message";
   $result=mysql_query($query,$id);
   if(mysql_num_rows($result)<1){
   echo "&nbsp;目前数据表中还没有任何留言！";
   }else{ //ELSE开始
     $totalnum=mysql_num_rows($result);
     $pagesize=10;
     $page=$_GET["page"];
     if($page==""){
       $page=1;
     }
     $begin=($page-1) * $pagesize;
     $totalpage=ceil($totalnum/$pagesize);

     echo "<table border=0 width=95%><tr><td>";
     $datanum=mysql_num_rows($result);
     echo "共有留言".$totalnum."条。";
     echo "每页".$pagesize."条，共".$totalpage."页<br>";
     
     for($j=1;$j<=$totalpage;$j++){
      echo "<a href=manage.php?page=".$j.">[".$j."]&nbsp;</a>";
     }
     echo "<br>";
     
     $query="SELECT * FROM message  order by addtime desc limit $begin,$pagesize";
     $result=mysql_query($query,$id);  
     for($i=1;$i<=$datanum;$i++){
      $info=mysql_fetch_array($result,MYSQL_ASSOC);
      echo "->[".$info['author']."]于".$info['addtime']."说：<br>";
      echo "&nbsp;&nbsp;".$info['content']."<br>";
       if($info['reply']!=""){
       echo "<b>管理员回复：</b>".$info['reply']."<br>";
       }
       ///增加了删除和回复
      echo "[<a href=manage.php?delid=".$info['id'].">删除此留言</a>]&nbsp;&nbsp;[<a href=reply.php?id=".$info['id'].">回复留言</a>]";
      echo "<hr>";
      }
     echo "</table>";
   } //ELSE结束
   mysql_close($id);
  ?>
</td></tr>
<tr><td height=60 bgcolor=#6c6c6c align=center>
<font color=#FFFFFF>版权所有：我的工作室<br>E-mail:rznqp@163.com
</td></tr>
</table>
</body>
</html>
```
reply.php
```
<?php
session_start();
if($_SESSION["login"]!="YES"){
 echo "你还没有登陆,无法管理留言!";
 exit;
}
$msgid=$_GET["id"];
$reply=$_POST["reply"];
if($reply!=""){
 $id=mysql_connect("localhost","root","1234");
 mysql_select_db("gbook",$id);
 $query="update message set reply='$reply' where id=".$msgid;
 mysql_query($query,$id);
 echo "<script language=javascript>alert('回复成功!');location.href='manage.php';</script>";
 exit;
 mysql_close($id);
}

 $id=mysql_connect("localhost","root","1234");
 mysql_select_db("gbook",$id);
 $query="select * from  message where id=$msgid";
 $result=mysql_query($query,$id);
 if(mysql_num_rows($result)<1){
 echo "没有此留言";
 exit;
 }
 $msg=mysql_fetch_array($result,MYSQL_ASSOC);
?>
<HTML>
<HEAD>
<TITLE>欢迎使用简约不简单-我的留言本</TITLE>
</HEAD>
<BODY>
<table border=1 cellspacing=0 cellspadding=0 style="border-collapse:collapse" align=center width=400 bordercolor=black height="358">
<tr><td height=100 bgcolor=#6C6C6C>
<font style="font-size:30px" color=#ffffff face="黑体">简约不简单-我的留言本</font>
</td>
<tr><td height=25>
&nbsp;&nbsp;<a href="manage.php">[返回管理]</a>
</td></tr>
<tr><td height=178>
  <form method="POST" action="reply.php?id=<?php echo $msgid;?>">
	<table border="1" width="95%" id="table1" cellspacing="0" cellpadding="0" bordercolor="#808080" style="border-collapse: collapse" height="154">
		<tr>
			<td colspan="2" height="29">
			<p align="center">管理员回复留言</td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">留言ID</td>
			<td width="67%"><?php echo $msg['id'];?></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">留言人</td>
			<td width="67%"><?php echo $msg['author'];?></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">留言时间</td>
			<td width="67%"><?php echo $msg['addtime'];?></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">留言内容</td>
			<td width="67%"><?php echo $msg['content'];?></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">请输入回复</td>
			<td width="67%"><textarea rows="7" name="reply" cols="33"></textarea></td>
		</tr>
		<tr>
			<td width="99%" colspan="2">
				<p align="center"><input type="submit" value="确定" name="B1"></p>		
			</td>
		</tr>
	</table>
	</form>
	</td></tr>
<tr><td height=58 bgcolor=#6c6c6c align=center>
<font color=#FFFFFF>版权所有：我的工作室<br>E-mail:rznqp@163.com
</td></tr>
</table>
</body>
</html>
<?php mysql_close(); ?>
```
send.php
```
<?php
$name=$_POST["name"];
if($name!=""){
 $content=$_POST["content"];
 $addtime=date("Y-m-d h:i:s");
 $id=mysql_connect("localhost","root","1234");
 mysql_select_db("gbook",$id);
 $query="insert into message(author,addtime,content,reply) values('$name','$addtime','$content','')";
 mysql_query($query,$id);
 mysql_close($id);
 echo "<script language=javascript>alert('留言成功!点击确定查看留言.');location.href='index.php';</script>";
 exit;
}
?>
<HTML>
<HEAD>
<TITLE>欢迎使用简约不简单-我的留言本</TITLE>
</HEAD>
<BODY>
<table border=1 cellspacing=0 cellspadding=0 style="border-collapse:collapse" align=center width=400 bordercolor=black>
<tr><td height=100 bgcolor=#6C6C6C>
<font style="font-size:30px" color=#ffffff face="黑体">简约不简单-我的原创留言本</font>
</td>
<tr><td height=25>
&nbsp;<a href=send.php>[我要写留言]</a>&nbsp;&nbsp;<a href=login.php>[管理留言]</a>
</td></tr>
<tr><td height=200>
  <form method="POST" action="send.php">
	<table border="1" width="95%" id="table1" cellspacing="0" cellpadding="0" bordercolor="#808080" style="border-collapse: collapse" height="265">
		<tr>
			<td colspan="2" height="29">
			<p align="center">欢迎您填写留言</td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">您的名字</td>
			<td width="67%"><input type="text" name="name" size="20"></td>
		</tr>
		<tr>
			<td width="32%">
			<p align="right">留言内容</td>
			<td width="67%"><textarea rows="10" name="content" cols="31"></textarea></td>
		</tr>
		<tr>
			<td width="99%" colspan="2">
				<p align="center"><input type="submit" value="提交" name="B1"></p>		
			</td>
		</tr>
	</table>
	</form>
	</td></tr>
<tr><td height=80 bgcolor=#6c6c6c align=center>
<font color=#FFFFFF>版权所有：我的工作室<br>E-mail:rznqp@163.com
</td></tr>
</table>
</body>
</html>
```
