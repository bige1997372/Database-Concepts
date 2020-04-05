### 测试使用python或php连接两种以上数据库服务端，并执行简单查询并打印返回结果
#### php连接MYSQL
```
<?php
$mysql_server="localhost";
$mysql_username="root";
$mysql_password="";
$mysql_database="第五周";
//建立数据库链接
$conn = mysqli_connect($mysql_server,$mysql_username,$mysql_password) or die("数据库链接错误");
//选择某个数据库
mysqli_query($conn,"set names 'utf8'");
mysqli_select_db($conn,'第五周');
//执行MySQL语句
$result=mysqli_query($conn,"SELECT cust_id,cust_name FROM customers");
if (!$result) {
    printf("Error: %s\n", mysqli_error($conn));
    exit();
}
//提取数据
while ($row=mysqli_fetch_row($result))
    {
        printf ("%s : %s",$row[0],$row[1]);
        echo "<br>";
    }
?>
```
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%85%AD%E5%91%A8%E4%BD%9C%E4%B8%9A_php%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/2.1.jpg)
#### python连接sqlite
```
#!/usr/bin/python

import sqlite3

conn = sqlite3.connect('test.db')
print ("Opened database successfully")
c = conn.cursor()

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (1, 'Paul', 32, 'California', 20000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (2, 'Allen', 25, 'Texas', 15000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (3, 'Teddy', 23, 'Norway', 20000.00 )")

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 )")
print ("Records created successfully")

cursor = c.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print("ID = ", row[0])
   print("NAME = ", row[1])
   print("ADDRESS = ", row[2])
   print ("SALARY = ", row[3], "\n")

print ("Operation done successfully")
conn.commit()
conn.close()
```
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%85%AD%E5%91%A8%E4%BD%9C%E4%B8%9A_php%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/1.2.jpg)

### 测试python或php使用两种以上不同方法连接同一数据库服务端，并执行简单查询并打印返回结果
#### 第一种方法（数据库函数mysqli连接）
```
<?php
$mysql_server="localhost";
$mysql_username="root";
$mysql_password="";
$mysql_database="第五周";
//建立数据库链接
$conn = mysqli_connect($mysql_server,$mysql_username,$mysql_password) or die("数据库链接错误");
//选择某个数据库
mysqli_query($conn,"set names 'utf8'");
mysqli_select_db($conn,'第五周');
//执行MySQL语句
$result=mysqli_query($conn,"SELECT cust_id,cust_name FROM customers");
if (!$result) {
    printf("Error: %s\n", mysqli_error($conn));
    exit();
}
//提取数据
while ($row=mysqli_fetch_row($result))
    {
        printf ("%s : %s",$row[0],$row[1]);
        echo "<br>";
    }
?>
```
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%85%AD%E5%91%A8%E4%BD%9C%E4%B8%9A_php%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/2.1.jpg)
#### 第二种方法（使用ODBC）
```
<html>
<body>

<?php
$conn=odbc_connect('23', 'root', '');
if (!$conn)
{
    exit("连接失败: " . $conn);
}

$sql="SELECT cust_id,cust_name FROM customers";
$rs=odbc_exec($conn,$sql);

if (!$rs)
{
    exit("SQL 语句错误");
}


while (odbc_fetch_row($rs))
{
    $cust_id=odbc_result($rs,"cust_id");
    $cust_name=odbc_result($rs,"cust_name");
    echo "<tr><td>$cust_id</td>";
    echo "<td>$cust_name</td></tr>";
	echo "<br>";
}
odbc_close($conn);
echo "</table>";
?>

</body>
</html>
```
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%85%AD%E5%91%A8%E4%BD%9C%E4%B8%9A_php%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/2.2.jpg)
#### 第三种方法（使用PDO）
```
<?php
 ini_set("display_errors", 0);
error_reporting(E_ALL ^ E_NOTICE);
error_reporting(E_ALL ^ E_WARNING);//忽略警告
$db = array(
    'host' => 'localhost',         //设置服务器地址
    'port' => '3306',              //设端口 
    'dbname' => '第五周',             //设置数据库名      
    'username' => 'root',           //设置账号
    'password' => '',      //设置密码
    'charset' => 'utf8',             //设置编码格式
    'dsn' => 'mysql:host=localhost;dbname=第五周;port=3306;charset=utf8',   //这里不知道为什么，也需要这样再写一遍。
);
$options = array(
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION, //默认是PDO::ERRMODE_SILENT, 0, (忽略错误模式)
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC, // 默认是PDO::FETCH_BOTH, 4
);
try{
    $pdo = new PDO($db['dsn'], $db['username'], $db['password'], $options);
}catch(PDOException $e){
    die('数据库连接失败:' . $e->getMessage());
}
echo '<pre/>';
$stmt = $pdo->query('select cust_id,cust_name from customers'); //返回一个PDOStatement对象
 
//$row = $stmt->fetch(); //从结果集中获取下一行，用于while循环
$rows = $stmt->fetchAll(); //获取所有
 
$row_count = $stmt->rowCount(); //记录数，2
$i=0;
for($i=0;$i<$row_count;$i++)
{
	echo $rows[$i][cust_id];
	echo $rows[$i][cust_name];
	echo '<br>';
}
?>
```
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%85%AD%E5%91%A8%E4%BD%9C%E4%B8%9A_php%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/2.3.jpg)
