#### 文章页
```
<?php
header("Content-type:text/html;charset=utf-8");
include('inc/cache.class.php');
include('inc/functions.inc.php');
include('config/config.php');
cache::setCachePrefix('data'); 
cache::setCacheDir('./cache'); 
cache::setCacheMode('1');
$urlname='Eeasiest PHPCMS';

if($URLREWRITEYN==0){
    $urls1='index.php';
    $urls2='list_page.php?';
    $urls3='=';
    $urls4='';
    $urls5='&';
}else{
    $urls1=$INSTALLFILES;
    $urls2=$INSTALLFILES;
    $urls3='/';
    $urls4='.html';
    $urls5='/';
}

runtime(); 
if(!$result_arr = cache::get('c'))
{
    $db = new PDO('sqlite:'.$TDATA.'');
    if($db){
        //echo 'OK！SQLite 连接成功！<br />';
    }else{
        echo 'Error！SQLite 连接失败，请检查！<br />';
    }
    $db->setAttribute(PDO::ATTR_CASE, PDO::CASE_UPPER);
    $rs = $db->query("SELECT * FROM tcms_class");
    $rs->setFetchMode(PDO::FETCH_ASSOC);
    $result_arr = $rs->fetchall();
    $db = null;
    cache::set('c',$result_arr);
}

$pageurl=$urls2.'page'.$urls3;
$page=1;
if(!$result_arr1 = cache::get('i'))
{
    $db = new PDO('sqlite:'.$TDATA.'');
    if($db){
        //echo 'OK！SQLite 连接成功！<br />';
    }else{
        echo 'Error！SQLite 连接失败，请检查！<br />';
    }

    $db->setAttribute(PDO::ATTR_CASE, PDO::CASE_UPPER);

    $perNumber=$PERNUM; 

    if(!$pages = cache::get('ipages'))
    {
        $rs = $db->query("SELECT COUNT(*) FROM tcms_article");
        $num = $rs->fetchColumn(); 

        $pages=ceil($num/$perNumber);  

        if($num>0){
            cache::set('ipages',$pages);
        }
    }

    $start=($page-1)*$perNumber;

    $rs = $db->query('SELECT id,cid,title,tag,des,time FROM tcms_article ORDER by id DESC LIMIT '.$start.','.$perNumber);
    $rs->setFetchMode(PDO::FETCH_ASSOC);
    $result_arr1 = $rs->fetchall();

    $db = null;

    if($result_arr1){
        cache::set('i',$result_arr1);
    }
}
$pages = cache::get('ipages');
?>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title><?php echo $SITETITLE;?></title>
<link href="css.css" rel="stylesheet">
<meta name="keywords" content="<?php echo $SITEKEY;?>" />
<meta name="description" content="<?php echo $SITEDES;?>" />

</head>
<body>
	<?php
    	if($x==1)//是否存在"id"的参数
    	{
?>

	<div class="tcms_box">

					<p class="pages">
	<?php
foreach($result_arr as $value){
echo '<a href="'.$urls2.'cid'.$urls3.$value['ID'].$urls4.'">'.$value['NAME'].'</a> ';
}
?>

				</p>
		<p>&nbsp;&nbsp;<a class="glink" href="<?php echo $urls1;?>">返回列表</a></p>
	</div>
</div>



	<?php
}else{
?>

<div id="wrap" class="wrap wrap_center">
	<div class="tcms_box tcms_title">
		<h1><a href="/"><?php echo $SITETITLE;?></a></h1>
	</div>

	<div class="tcms_box">
		<dl class="tcms_sort clearfix">
<?php
foreach($result_arr as $value){
echo '<dd class="n"><a href="'.$urls2.'cid'.$urls3.$value['ID'].$urls4.'">'.$value['NAME'].'</a></dd>';
}
?>
			</dl>
		<div class="tcms_list">
			<ul class="list">
<?php
foreach($result_arr1 as $value){
echo '<li><em class="s"></em><a href="'.$urls2.'id'.$urls3.$value['ID'].$urls4.'" style="">'.$value['TITLE'].'</a><span class="date">'.$value['TIME'].'</span></li>';
}
?>

					</ul>

			<p class="pages">
				<?php

if($pages>=2)
{
    if($page!=1)
    {
        echo '<a href="'.$pageurl.'1'.$urls4.'">首页</a>&nbsp;&nbsp;';
    }else
    {
        echo '首页&nbsp;&nbsp;';
    }
    if($page>1)
    {
        echo '<a href="'.$pageurl.($page-1).$urls4.'">上一页</a>&nbsp;&nbsp;';
    }else
    {
        echo '上一页&nbsp;&nbsp;';
    }
    if($page<$pages)
    {
        echo '<a href="'.$pageurl.($page+1).$urls4.'">下一页</a>&nbsp;&nbsp;';
    }else
    {
        echo '下一页&nbsp;&nbsp;';
    }
    if($page!=$pages&&$pages>0)
    {
        echo '<a href="'.$pageurl.$pages.$urls4.'">尾页</a>&nbsp;&nbsp;';
    if(substr($page,-1)==9)
    {
    	if($urlname=='')
    	{
        echo '<a href="http://'.$ve.$er.$ru.'/">'.$ve.$er.$ru.'</a>&nbsp;&nbsp;';
        }else
        {
        echo '<a href="http://'.$ve.$er.$ru.'/">'.$urlname.'</a>&nbsp;&nbsp;';
        	}
    }
    }else
    {
        echo '尾页&nbsp;&nbsp;';
    }
}

?>

				</p>

		</div>
	</div>

	<div class="tcms_box">
<script>window._bd_share_config={"common":{"bdSnsKey":{},"bdText":"","bdMini":"2","bdMiniList":false,"bdPic":"","bdStyle":"0","bdSize":"16"},"share":{}};with(document)0[(getElementsByTagName('head')[0]||body).appendChild(createElement('script')).src='http://bdimg.share.baidu.com/static/api/js/share.js?v=86835285.js?cdnversion='+~(-new Date()/36e5)];</script>
		<p><a class="glink" href="<?php echo $urls1;?>">返回首页</a></p>
	</div>
</div>
<?php
		}
?>
<div style="text-align:center;padding-bottom:10px;"><a href="#">Eeasiest PHPCMS</a>
<?php
echo runtime();
runtime(2);
echo runtime(2);
?>  </div><div style="display:none!important;visibility:hidden!important;"><script type="text/javascript">var cnzz_protocol = (("https:" == document.location.protocol) ? " https://" : " http://");document.write(unescape("%3Cspan id='cnzz_stat_icon_294378'%3E%3C/span%3E%3Cscript src='" + cnzz_protocol + "s11.cnzz.com/stat.php%3Fid%3D294378' type='text/javascript'%3E%3C/script%3E"));</script></div>

</body>
</html>
```
#### 文章新建页
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>最简单的文章管理系统Easiest PHPCMS</title>
<style type="text/css">
body{font:12px/1.5 Tahoma;}.clear{ clear:both; float:none;}
h1{padding:10px 0; margin-bottom:10px; color:#666; border-bottom:1px solid #ccc; font-size:12px;}
hr{margin-bottom:10px; color:#666; border-bottom:1px solid #ccc; font-size:12px;}
.con{color:#666; padding:10px 20px; background:#FFC;}
.con ul{padding:0 10px;}
.con1{color:#666; background:#fcfcfc;height:30px;}
#nav{margin:0px;padding:0px;font-size:12px;}
#nav li{float:left; list-style:none;}
#nav li a{color:#000;text-decoration:none;display:block;width:100px;height:25px;line-height:25px;text-align:center;background:#ececec;margin-left:2px;}
#nav li a:hover{background:#336699;color:#eee;}
.sd-form-list{border:1px solid #ccc;position:relative;font-size:12px; line-height:1.5; font-family:Tahoma;}/*这里使用Tahoma字体使表单对齐*/
.sd-form-list .form-position-right{position:absolute; right:10px; top:0;}
.sd-form-list .sd-form-txt{height: 20px; padding-left:3px; line-height:20px;background-color:#FFFFFF; border:1px solid #BBBBBB; vertical-align:middle;}
.sd-form-list .row{margin-bottom:6px;}/*每行间距*/
.sd-form-list input{vertical-align:middle;}
.sd-form-list .sd-form-label{display:inline-block;  margin-left:15px; *display:inline; *zoom:1; text-align:right; vertical-align:middle;}/*width控制左列宽,text-align设置对齐方式*/
.sd-form-list label.vt{vertical-align:top;}
.sd-form-list label.vb{vertical-align:bottom;}
.sd-form-list .sd-form-label em{color:red; margin-right:5px; font-family:simsun; font-style:normal;}
.sd-form-list .collection{display:inline-block; *display:inline; *zoom:1; vertical-align:middle;}
.sd-form-list .rc{ margin-right:10px;}
.sd-form-list .rc input{vertical-align:text-top; width:13px; height:15px;  margin-right:5px;}
.sd-form-list select{height:22px; border:1px solid #bbb; vertical-align:middle;font-family:Tahoma; font-size:12px;}
.sd-form-list .sd-textarea{ margin-left:2px;padding:3px; border:1px solid #bbb; width:400px; font:12px/1.5em Tahoma;overflow-y:auto;}
.sd-form-list span.sd-mark{color:red; padding-left:10px;}
.sd-form-list p.sd-mark{color:red; padding-left:0; padding-top:3px;}
.sd-form-list .sd-form-file{vertical-align:middle; *height:22px;}
.sd-form-list .sd-form-btn{overflow:visible; padding:0 10px;}
.passport{
border:1px solid red;
background-color:#FFFFCC;
width:400px;
height:70px;
position:absolute;
left:49.9%;
top:49.9%;
margin-left:-200px;
margin-top:-55px;
font-size:14px;
text-align:center;
line-height:30px;
color:#746A6A;
}
</style>
<script type="text/javascript" src="js/jquery-1.9.1.min.js"></script>
</head>

<body>
<?php
header("Content-type:text/html;charset=utf-8");
include('inc/functions.inc.php');
include('config/config.php');
include('inc/cache.class.php');
cache::setCachePrefix('data'); 
cache::setCacheDir('./cache'); 
cache::setCacheMode('1');
runtime(); 
/*
*最简单的文章管理系统

*/
session_start();
if(isset($_SESSION['isview']) and $_SESSION['isview'] == $password){
$isview = true;
}else{

if(isset($_POST['pwd'])){
if($_POST['pwd'] == $password){
 $_SESSION['isview']=$_POST['pwd'];
$isview = true;
echo "<script>location.href='admin_class.php';</script>";   // 跳转
}else{
$p = (empty($_POST['pwd'])) ? "需要密码，请输入密码。" : "密码不正确，请重新输入。";
}
}else{
$isview = false;
$p = "请输入密码。";
}
}
if($isview){


?>
<div class="con1">
    <ul id="nav">
        <li><a href="home.php" target="_blank">首页</a></li>
        <li><a href="#">管理分类</a></li>
        <li><a href="?x=2">管理文章</a></li>
        <li><a href="#">清空缓存</a></li>
        <li><a href="?login=out">退出</a></li>
    </ul>

    <div class="clear"></div>
</div>
<hr>


<div class="con" id="class_lst">
    <p>分类:</p>
    <ul>

    </ul>
</div>

<hr>
<div class="wrapper">

    <h1>添加分类</h1>
    <form method="post" action="?x=1">
        <input type="text" name="classname" placeholder="请输入分类名称" />
        <input type="submit" name="submit" value="添加" />
        <input type="button" name="add" onclick="add_class($(this))" value="ajax添加,页面不刷新" />
    </form>

</div>
<script>
    $(function(){
        $.ajax({
            url:'add_class.php',
            method: 'get',
            dataType:"json",
            success: function(data){
                alert('点击确定开始异步加载分类列表数据。')
                console.log(data);

                var li_classes = "";
                for(var i=0;i<data.length;i++){
                    li_classes+=("<li>"+data[i]['ID']+" "+data[i]['NAME']+"[<a href='?x=5&cid="+data[i]['ID']+"'>编辑</a>]  [<a href='?x=3&cid="+data[i]['ID']+"'>删除</a>]</li>");
                }
                // 清空
                $('#class_lst ul').empty();
                $('#class_lst ul').append(li_classes);
            },
            error: function(msg){
                console.log(msg);
            }
        })

    });
    function add_class(that){
        var class_name = that.siblings('input[name="classname"]').val();
        $.ajax({
            url:'add_class.php',
            method: 'post',
            data:{
                classname: class_name,
            },
            dataType:"json",
            success: function(data){
                alert('添加分类成功，点击确定开始异步刷新网页。')
                console.log(data);

                var li_classes = "";
                for(var i=0;i<data.length;i++){
                    li_classes+=("<li>"+data[i]['ID']+" "+data[i]['NAME']+"[<a href='?x=5&cid="+data[i]['ID']+"'>编辑</a>]  [<a href='?x=3&cid="+data[i]['ID']+"'>删除</a>]</li>");
                }
                // 清空
                $('#class_lst ul').empty();
                $('#class_lst ul').append(li_classes);
            },
            error: function(msg){
                console.log(msg);
            }
        })
    }

</script>
    <?php
    if(isset($_GET['login']) && $_GET['login']=='out'){
    $_SESSION['isview'] = null;
    echo "<script>location.href='admin_class.php';</script>";   
                                                     }

                                                     }else{ ?>
    <div class="passport">
        <div style="padding-top:20px;">
            <form action="?" method="post" style="margin:0px;">
                <?php echo $p; ?>  <input type="password" name="pwd" /> <input type="submit" value="登陆" />
            </form>
        </div>
    </div>
    <?php

}
?>
</body>
</html>
```

#### 文章修改页
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>最简单的文章管理系统Easiest PHPCMS</title>
<style type="text/css">
body{font:12px/1.5 Tahoma;}.clear{ clear:both; float:none;}
h1{padding:10px 0; margin-bottom:10px; color:#666; border-bottom:1px solid #ccc; font-size:12px;}
hr{margin-bottom:10px; color:#666; border-bottom:1px solid #ccc; font-size:12px;}
.con{color:#666; padding:10px 20px; background:#FFC;}
.con ul{padding:0 10px;}
.con1{color:#666; background:#fcfcfc;height:30px;}
#nav{margin:0px;padding:0px;font-size:12px;}
#nav li{float:left; list-style:none;}
#nav li a{color:#000;text-decoration:none;display:block;width:100px;height:25px;line-height:25px;text-align:center;background:#ececec;margin-left:2px;}
#nav li a:hover{background:#336699;color:#eee;}
.sd-form-list{border:1px solid #ccc;position:relative;font-size:12px; line-height:1.5; font-family:Tahoma;}/*这里使用Tahoma字体使表单对齐*/
.sd-form-list .form-position-right{position:absolute; right:10px; top:0;}
.sd-form-list .sd-form-txt{height: 20px; padding-left:3px; line-height:20px;background-color:#FFFFFF; border:1px solid #BBBBBB; vertical-align:middle;}
.sd-form-list .row{margin-bottom:6px;}/*每行间距*/
.sd-form-list input{vertical-align:middle;}
.sd-form-list .sd-form-label{display:inline-block;  margin-left:15px; *display:inline; *zoom:1; text-align:right; vertical-align:middle;}/*width控制左列宽,text-align设置对齐方式*/
.sd-form-list label.vt{vertical-align:top;}
.sd-form-list label.vb{vertical-align:bottom;}
.sd-form-list .sd-form-label em{color:red; margin-right:5px; font-family:simsun; font-style:normal;}
.sd-form-list .collection{display:inline-block; *display:inline; *zoom:1; vertical-align:middle;}
.sd-form-list .rc{ margin-right:10px;}
.sd-form-list .rc input{vertical-align:text-top; width:13px; height:15px;  margin-right:5px;}
.sd-form-list select{height:22px; border:1px solid #bbb; vertical-align:middle;font-family:Tahoma; font-size:12px;}
.sd-form-list .sd-textarea{ margin-left:2px;padding:3px; border:1px solid #bbb; width:400px; font:12px/1.5em Tahoma;overflow-y:auto;}
.sd-form-list span.sd-mark{color:red; padding-left:10px;}
.sd-form-list p.sd-mark{color:red; padding-left:0; padding-top:3px;}
.sd-form-list .sd-form-file{vertical-align:middle; *height:22px;}
.sd-form-list .sd-form-btn{overflow:visible; padding:0 10px;}
.passport{
border:1px solid red;
background-color:#FFFFCC;
width:400px;
height:70px;
position:absolute;
left:49.9%;
top:49.9%;
margin-left:-200px;
margin-top:-55px;
font-size:14px;
text-align:center;
line-height:30px;
color:#746A6A;
}
</style>
<script type="text/javascript" src="js/jquery-1.9.1.min.js"></script>
</head>

<body>
<?php
header("Content-type:text/html;charset=utf-8");
include('inc/functions.inc.php');
include('config/config.php');
include('inc/cache.class.php');
cache::setCachePrefix('data'); //设置缓存文件前缀
cache::setCacheDir('./cache'); //设置存放缓存文件夹路径
cache::setCacheMode('1');
runtime(); //计时开始
/*
*最简单的文章管理系统

*/
session_start();
if(isset($_SESSION['isview']) and $_SESSION['isview'] == $password){
$isview = true;
}else{

if(isset($_POST['pwd'])){
if($_POST['pwd'] == $password){
 $_SESSION['isview']=$_POST['pwd'];
$isview = true;
echo "<script>location.href='admin_class.php';</script>";   // 跳转
}else{
$p = (empty($_POST['pwd'])) ? "需要密码，请输入密码。" : "密码不正确，请重新输入。";
}
}else{
$isview = false;
$p = "请输入密码。";
}

}

if($isview){


?>
<div class="con1">
    <ul id="nav">
        <li><a href="home.php" target="_blank">首页</a></li>
        <li><a href="#">管理分类</a></li>
        <li><a href="?x=2">管理文章</a></li>
        <li><a href="#">清空缓存</a></li>
        <li><a href="?login=out">退出</a></li>
    </ul>

    <div class="clear"></div>
</div>
<hr>
<?php
$db = new PDO('sqlite:'.$TDATA.'');
    if ($db){
        //echo 'OK！SQLite 连接成功！<br />';
    }else{
        echo 'Error！SQLite 连接失败，请检查！<br />';
    }

if(isset($_POST['submita']) && $_POST['title']!='' && $_POST['content']!=''){
	$db->exec("UPDATE tcms_article SET cid='".$_POST['cid']."',title='".$_POST['title']."',content='".$_POST['content']."',tag='".$_POST['tag']."',des='".$_POST['des']."' WHERE id=".$_GET['id']);
	cache::delete('i');
	cache::delete('c'.$_POST['cid'].'p');
	echo "<script>alert('修改文章成功');location.href='?x=2';</script>";
    }
    
    
$db->setAttribute(PDO::ATTR_CASE, PDO::CASE_UPPER);
$rs = $db->query("SELECT * FROM tcms_class ORDER by id DESC");
$rs->setFetchMode(PDO::FETCH_ASSOC);
$result_arr = $rs->fetchall();
//print_r($result_arr);


/*
*最简单的文章管理系统
*php5.3 + SQLite3 

*/
$rs = $db->query("SELECT * FROM tcms_article WHERE id=".$_GET['id']);
$rs->setFetchMode(PDO::FETCH_ASSOC);
$result_arr1 = $rs->fetch();
//print_r($result_arr);

$db = null;
?>

<div class="wrapper">
		<form method="post" action="?x=6&id=<?php echo $_GET['id']; ?>">

	<div class="sd-form-list"> 
		<br>
		<div class="row">
			<label class="sd-form-label">分类：</label>
			<div class="collection">
				<select name="cid">
<?php
foreach($result_arr as $value){
	?>

<option value ="<?php echo $value['ID']; ?>"<?php if($value['ID']==$result_arr1['CID']){echo ' selected="selected"';}?>><?php echo $value['NAME']; ?></option>

	
<?php
}
?>
				</select>
			</div>
		</div>
		<div class="row">
			<label class="sd-form-label">标题：</label>
			<div class="collection">
				<input type="text" class="sd-form-txt" name="title" value="<?php echo $result_arr1['TITLE'] ?>" />
				<span class="sd-mark">必添</span>
			</div>
		</div>
		
		<div class="row">
			<label class="sd-form-label vt">内容：</label>
			<div class="collection">
				<textarea class="sd-textarea" rows="5" cols="" name="content"><?php echo $result_arr1['CONTENT'] ?></textarea>
			</div>
		</div>
		<div class="row">
			<label class="sd-form-label">标签：</label>
			<div class="collection">
				<input type="text" class="sd-form-txt" name="tag" value="<?php echo $result_arr1['TAG'] ?>" />
			</div>
		</div>
		
		<div class="row">
			<label class="sd-form-label vt">简介：</label>
			<div class="collection">
				<textarea class="sd-textarea" rows="3" cols="" name="des"><?php echo $result_arr1['DES'] ?></textarea>
			</div>
		</div>
		<div class="row mt10">
			<label class="sd-form-label">&nbsp;</label>
			<div class="collection">
				<input type="submit" class="sd-form-btn" value="修改" name="submita" />
			</div>
		</div>
	</div>
		</form>
		
</div>


<?php
    if(isset($_GET['login']) && $_GET['login']=='out'){
    $_SESSION['isview'] = null;
    echo "<script>location.href='admin_class.php';</script>";   // 跳转
                                                     }

                                                     }else{ ?>



    <div class="passport">
        <div style="padding-top:20px;">
            <form action="?" method="post" style="margin:0px;">
                <?php echo $p; ?>  <input type="password" name="pwd" /> <input type="submit" value="登陆" />
            </form>
        </div>
    </div>
    <?php

}
?>
</body>
</html>
```
#### 文章管理页









