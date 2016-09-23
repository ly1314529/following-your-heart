最近 看了node.js权威指南，里面介绍了mogodb的使用方法，和我之前调用的库不一样，特此总结一下

安装方法不再说了，[这里有](https://github.com/ly1314529/ly/blob/master/NOSQL/mongoDB.md)

还是一样的，npm install mongodb,

* 源码
```javascript
var mongo=require('mongodb');   //调用库
var host='localhost';           //本机地址
var port=mongo.Connection.DEFAULT_PORT; //本机端口，这里会报一个错，下面再说
var server=new mongo.Server(host,port,{auto_reconnect:true});  //创建服务器数据库对象，自动连接
var db=new mongo.Db('node-mongo',server,{safe:true}); //创建一个数据库

//在数据库里操作
db.open(function (err,db) {    
  db.collection('users',function(err,collection) {
  collection.insert({name:'ly',age:18},   //插入数据
    function (err,docs) { 
    if (err) throw err;   //是否报错
    else {
    console.log(docs); //服务器返回数据
    db.close(); //关闭数据库
    }
    });
    });
    }
  );
```

 * 关于port报错
 
 在运行的时候，会报错，端口号找不到，这时需要启动mongodb，mongodb --dbpath (mongodb安装路径)
 ,最后一行有端口号信息，是27017(因人而异）,把port换成27017就可以运行了
 
 * 查询数据
 ```javascript
 db.collection('users',function(err,collection) {
   if (err) throw err;
   else {
   collection.find({}).toArray(function (err,data) {   //数据库输出数据依照数组形式展示
    console.log(data);   //find({name:'ly'}) ,search the users who's name is ly
    db.close();
    }
   );
   }
   });
   ```
 * 更新数据
 ```javascript
 collection.update({被操作的数据},{更新的数据}，callback());
  ```
    * findAndModify()
    
    ```javascript
    collection.findAndModify({被操纵的数据},{修改后的数据},{相关参数},callback());
    ```
    * 关于相关参数
       * {multi:true},可以用到$set:{更新的数据},所有的数据都会被改成{更新的数据}
       * [['name',1],['price',-1]]:第一个是升序排列，第二个是降序排列
       * {upsert:true},插入一条数据
  
  * 删除数据
   
   collection.remove();//一样的用法
