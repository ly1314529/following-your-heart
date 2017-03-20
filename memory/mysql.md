直接切入正题，我是在linux上安装的mysql，mysql很多的安装教程，就不多说了

这里将会介绍node.js 是如何链接mysql的，总的来说，有点坑！！！

----
* 打开mysql

我用的是在windows下安装的命令行的linux，更加的快捷，运行起来，下载东西毫无压力，如果你也想拥有一款[看教程](https://github.com/ly1314529/ly/blob/master/test/cucumber%20run.md)
```
$mysql -u root -p //root是你的mysql的用户名,然后输入你的密码
mysql>select host,user,password from mysql.user; //只想说一句，分号很重要，don't ask why,这句话你可以查出你的host，user,password(加了md5编码的)
mysql>insert into mysql.user(Host,User,password) values('localhost','用户名','密码');//create a new user
mysql>select user(); //查看当前用户
mysql>show variables like 'port'; //find the port
mysql>show databases; //展示数据库表
```
* 源码
   
   * 链接数据库
   
```javascript
   
var mysql = require('mysql');
var connection = mysql.createConnection({
    host: 'localhost',
    user: '用户名',
    password: '密码',
    database:'表名',
    port: 3306  //端口号，默认3306
});
connection.connect(function (err) {
if (err) console.log('连接数据库失败');   
else {console.log('链接数据库成功');   
connection.end(function (err) {
  if (err) console.log('关闭数据库失败');  
else {
console.log('关闭数据库成功');
}
});
}
});

```

--------

mysql 基本语句

```
* mysql -u root(登录名) -p
* create database samp_db character set gbk;  //创建一个叫sample_db的数据库
* show databases;  //展示数据库
* drop database db; //删掉db这个数据库
* create table students
	（
		id int unsigned not null auto_increment primary key,
		name char(8) not null,
		sex char(4) not null,
		age tinyint unsigned not null,
		tel char(13) null default "-"
	);  //创建表students
  
  
  * insert into students value("值");  //插入数据
  * select name,age from students; //查询数据
  * select * from students where id>2; //条件查询语句
  * update students set tel='111' where id=3;  //从students表中将id为3的tel改为‘111’
  * delete from students where id=2;
