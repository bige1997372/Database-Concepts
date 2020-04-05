### 测试使用python或php连接两种以上数据库服务端，并执行简单查询并打印返回结果
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
![image]()
#### 第二种方法（使用ODBC）

