### cms系统逻辑设计
![image]()
### cms系统代码
```
CREATE TABLE content(
    articleID INT NOT NULL   COMMENT '主键' ,
    channelID INT    COMMENT '所属栏目' ,
    classID INT    COMMENT '分类' ,
    typeID INT    COMMENT '类型' ,
    title VARCHAR(128)    COMMENT '标题' ,
    summary VARCHAR(3072)    COMMENT '简介' ,
    content TEXT    COMMENT '内容' ,
    hits INT    COMMENT '人气' ,
    searchtext TEXT    COMMENT '搜索' ,
    addeddate DATETIME    COMMENT '添加日期' ,
    addedpersonID INT    COMMENT '添加人' ,
    updateddate DATETIME    COMMENT '最后修改日期' ,
    updatedpersonID INT    COMMENT '最后修改人' ,
    PRIMARY KEY (articleID)
) COMMENT = '内容 ';;
```
```
CREATE TABLE channels(
    channelID INT NOT NULL   COMMENT '主键' ,
    channelname VARCHAR(128)    COMMENT '栏目名称' ,
    sort INT    COMMENT '排序' ,
    url VARCHAR(128)    COMMENT '栏目网址' ,
    PRIMARY KEY (channelID)
) COMMENT = '网站内容 ';;
```
```
CREATE TABLE download(
    downloadID INT NOT NULL   COMMENT '主键' ,
    articleID INT    COMMENT '内容ID' ,
    title VARCHAR(128)    COMMENT '标题' ,
    downurl VARCHAR(128)    COMMENT '下载地址' ,
    downcount INT    COMMENT '下载次数' ,
    addeddate DATETIME    COMMENT '上传时间' ,
    version VARCHAR(128)    COMMENT '版本' ,
    PRIMARY KEY (downloadID)
) COMMENT = ' ';;
```
```
CREATE TABLE comment(
    commenttitle VARCHAR(128)    COMMENT '评论标题' ,
    commentcontent TEXT    COMMENT '评论内容' ,
    commentator INT    COMMENT '评论人' ,
    commenttime DATETIME    COMMENT '评论时间' 
) COMMENT = ' ';;
```
```
CREATE TABLE class(
    classID INT NOT NULL   COMMENT '主键' ,
    channelID INT    COMMENT '所属栏目' ,
    class VARCHAR(128)    COMMENT '分类' ,
    PRIMARY KEY (classID)
) COMMENT = ' ';;
```
```
CREATE TABLE video(
    videoID INT NOT NULL   COMMENT '视频ID' ,
    videoname VARCHAR(128)    COMMENT '视频名称' ,
    videoclass VARCHAR(128)    COMMENT '视频类型' ,
    videourl VARCHAR(128)    COMMENT '视频地址' ,
    playtime INT    COMMENT '播放次数' ,
    PRIMARY KEY (videoID)
) COMMENT = ' ';;
```
