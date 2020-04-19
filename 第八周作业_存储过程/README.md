### 存储过程的编写，in，out等参数的使用
```
delimiter //
CREATE PROCEDURE ad(IN uid INT)
BEGIN
SELECT * FROM biterm WHERE USER_ID LIKE uid;

END //
delimiter ;

mysql> delimiter //
mysql> create procedure out_param(out p_out int)
    ->   begin
    ->     select p_out;
    ->     set p_out=2;
    ->     select p_out;
    ->   end
    -> //
mysql> delimiter ;
mysql> select @p_out;

mysql> delimiter //
mysql> create procedure inout_param(inout p_inout int)
    ->   begin
    ->     select p_inout;
    ->     set p_inout=2;
    ->     select p_inout;
    ->   end
    -> //
mysql> delimiter ;
mysql> set @p_inout=1; 
mysql> call inout_param(@p_inout);
```
### 局部变量的定义
```
DELIMITER $$
CREATE PROCEDURE test_while_001(IN in_count INT) # 创建存储过程 学习while循环的用法
BEGIN
    DECLARE COUNT INT DEFAULT 0;
    DECLARE SUM INT DEFAULT 0;
    WHILE COUNT < in_count DO
        SET SUM = SUM + COUNT;
        SET COUNT = COUNT + 1;
    END WHILE;
    SELECT SUM;
END $$
DELIMITER ;
```
### 条件控制的使用
```
CREATE PROCEDURE example_if (IN x INT)
BEGIN
	IF x = 1 THEN
		SELECT 1;
	ELSEIF x = 2 THEN 
		SELECT 2;
	ELSE
		SELECT 3;
	END IF;
END;

```
### 循环控制的使用
```
DELIMITER $$  
  
DROP PROCEDURE IF EXISTS `test`.`WhileLoopProc` $$  
CREATE PROCEDURE `test`.`WhileLoopProc` ()  
BEGIN  
 DECLARE x  INT;  
 DECLARE str  VARCHAR(255);  
 SET x = 1;  
 SET str =  '';  
 WHILE x  <= 5 DO  
     SET  str = CONCAT(str,x,',');  
     SET  x = x + 1;  
 END WHILE;  
 SELECT str;  
END $$  
  
DELIMITER ;  
```
