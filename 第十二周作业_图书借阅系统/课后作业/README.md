### 题目一
#### 案例一逻辑设计（E-R图）
![image]()
#### 物理实施（代码）
```
CREATE TABLE user_info(
    user_id INT NOT NULL   COMMENT '用户编号' ,
    user_name VARCHAR(128)    COMMENT '用户名' ,
    password VARCHAR(128)    COMMENT '登陆密码' ,
    realname VARCHAR(128)    COMMENT '真实姓名' ,
    gender VARCHAR(128)    COMMENT '性别' ,
    postcode VARCHAR(128)    COMMENT '地址邮编' ,
    telephone VARCHAR(128)    COMMENT '电话号码' ,
    E-mail VARCHAR(128)    COMMENT '电子邮件' ,
    regist_time DATETIME    COMMENT '注册时间' ,
    PRIMARY KEY (user_id)
) COMMENT = '用户 ';;

CREATE TABLE manager(
    manager_user_name VARCHAR(128) NOT NULL   COMMENT '管理员账号' ,
    password VARCHAR(128)    COMMENT '密码' ,
    PRIMARY KEY (manager_user_name)
) COMMENT = '管理员 ';;

CREATE TABLE book_cata(
    type_id VARCHAR(128) NOT NULL   COMMENT '类型编号' ,
    type_name VARCHAR(128)    COMMENT '类型名称' ,
    explaination TEXT    COMMENT '说明' ,
    PRIMARY KEY (type_id)
) COMMENT = '图书分类表 ';;

CREATE TABLE book_info(
    book_id INT NOT NULL   COMMENT '图书编号' ,
    book_name VARCHAR(128)    COMMENT '书名' ,
    type_id VARCHAR(128)    COMMENT '类型编号' ,
    writter VARCHAR(128)    COMMENT '作者' ,
    press VARCHAR(128)    COMMENT '出版社' ,
    publish_time DATETIME    COMMENT '出版日期' ,
    introduction TEXT    COMMENT '内容简介' ,
    total_amount INT    COMMENT '总数量' ,
    remain_amount INT    COMMENT '剩余数量' ,
    cover VARCHAR(128)    COMMENT '封面' ,
    unit_price DECIMAL(32,10)    COMMENT '单价' ,
    comment_amount INT    COMMENT '评论条数' ,
    click_times INT    COMMENT '点击次数' ,
    PRIMARY KEY (book_id)
) COMMENT = '图书信息 ';;

CREATE TABLE book_comment(
    comment_id INT NOT NULL   COMMENT '评论编号' ,
    book_id INT    COMMENT '图书编号' ,
    explanation TEXT    COMMENT '说明' ,
    comment TEXT    COMMENT '评论' ,
    reader_id INT    COMMENT '读者编号' ,
    comment_time DATETIME    COMMENT '评论日期' ,
    PRIMARY KEY (comment_id)
) COMMENT = '图书评论 ';;

CREATE TABLE order(
    user_id INT    COMMENT '用户编号' ,
    order_id INT NOT NULL   COMMENT '订单号' ,
    book_id INT    COMMENT '书号' ,
    order_amount INT    COMMENT '订购数' ,
    total_amount INT    COMMENT '总计' ,
    orderor_id INT    COMMENT '订购者编号' ,
    PRIMARY KEY (order_id)
) COMMENT = '订单 ';;

CREATE TABLE orderors(
    orderor_id INT NOT NULL   COMMENT '订购者编号' ,
    address VARCHAR(128)    COMMENT '邮寄地址' ,
    postcode VARCHAR(128)    COMMENT '邮编' ,
    telephone VARCHAR(32)    COMMENT '移动电话' ,
    remark TEXT    COMMENT '邮寄备注' ,
    send_method VARCHAR(128)    COMMENT '邮寄方法' ,
    pay_method VARCHAR(128)    COMMENT '付款方法' ,
    order_date DATETIME    COMMENT '订购日期' ,
    invoice VARCHAR(1)    COMMENT '是否要发票' ,
    PRIMARY KEY (orderor_id)
) COMMENT = '图书订购者详情 ';;
```
### 题目二
#### 概念设计
在图书信息表中增加条码属性。
小票信息表：是否打印小票，小票标题，是否使用读码器，打印机类型，打印单号，打印日期，打印收费员，说明，联系电话。
退书单：退货单号，退货日期，退货类型，发货方式，退书数量，操作员，审核状态，添加日期，备注，源单号。
图书促销及赠品表：活动编号，促销书编号，促销时间，折扣，是否有赠品，赠品描述。
#### 逻辑设计
![image]()
#### 物理实现
```
alter table book_info add column barcode varchar(30);
CREATE TABLE ticket(
    ticket VARCHAR(1)    COMMENT '是否打印小票' ,
    headline VARCHAR(128)    COMMENT '小票标题' ,
    code_reader VARCHAR(1)    COMMENT '是否使用读码器' ,
    printer_type VARCHAR(128)    COMMENT '打印机类型' ,
    print_id INT    COMMENT '打印单号' ,
    print_date DATETIME    COMMENT '打印日期' ,
    print_man VARCHAR(128)    COMMENT '打印收费员' ,
    explaination TEXT    COMMENT '说明' ,
    telephone VARCHAR(128)    COMMENT '联系电话' 
) COMMENT = '小票 ';;
CREATE TABLE sendback(
    back_id INT NOT NULL   COMMENT '退货单号' ,
    back_date DATETIME    COMMENT '退货日期' ,
    back_type VARCHAR(128)    COMMENT '退货类型' ,
    back_method VARCHAR(128)    COMMENT '发货方式' ,
    back_amount INT    COMMENT '退书数量' ,
    operator VARCHAR(128)    COMMENT '操作员' ,
    status VARCHAR(128)    COMMENT '审核状态' ,
    date DATETIME    COMMENT '添加日期' ,
    remark TEXT    COMMENT '备注' ,
    origin_id INT    COMMENT '源单号' ,
    PRIMARY KEY (back_id)
) COMMENT = '退货 ';;
CREATE TABLE sale(
    sale_id INT NOT NULL   COMMENT '活动编号' ,
    book_id INT    COMMENT '促销书编号' ,
    sale_time DATETIME    COMMENT '促销时间' ,
    discount DECIMAL(4,2)    COMMENT '折扣' ,
    giveaway VARCHAR(1)    COMMENT '是否有赠品' ,
    giveaway_dis TEXT    COMMENT '赠品描述' ,
    PRIMARY KEY (sale_id)
) COMMENT = '图书促销和赠品 ';;
```
