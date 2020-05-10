### 题目一
#### 安装XAMPP
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/1.jpg)
### 题目二
#### PHP调试工具的安装、配置和使用
配置xampp中自带的xdebug
在php.ini文件中添加下列语句：
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/7.jpg)
打开apache，开启phpinfo显示配置完成：
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/8.jpg)
编写测试代码测试xdebug：
```
<?php
testXdebug();
function testXdebug() {
       requireFile();   
}
function requireFile() {
       require_once(‘abc.php’);
}
?>
```
结果如下，错误信息显示彩色：
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/9.jpg)
#### 测试php，连接数据库
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/2.jpg)
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/3.jpg)
### 题目三
#### 安装调试python的ORM框架sqlalchemy
安装 sqlalchemy
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/4.jpg)
安装pymysql
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/5.jpg)
测试
![image](https://github.com/bige1997372/Database-Concepts/blob/master/%E7%AC%AC%E5%8D%81%E5%91%A8%E5%8F%8A%E7%AC%AC%E5%8D%81%E4%B8%80%E5%91%A8%E4%BD%9C%E4%B8%9A/%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A/6.jpg)
