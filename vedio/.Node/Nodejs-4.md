# MongoDB在Node中使用

### 一、MongoDB命令

###### 1、基本操作
   
``` .mongodb
    1、查看数据库
    show dbs;
    2、切换到指定数据库
    use “dbName”
    3、查询总行数
    db.getCollection('student').count()
    db.collectionName.stats().count
    db.collectionName.find().count()
    4、查询所有数据
    db.getCollection('student').find({});
    5、查询name为sa的数据
    db.getCollection('student').find({'name':'sa'});
```
  
****

   
	
	
 
	
### 二、利用MongoDB包连接MongoDB
 `npm下载包 Npm -install mongodb`
``` .js
//解构初始化参数
const [dbClient, _connect] = [require("mongodb"), (cb) => {
    var url = "mongodb://127.0.0.1:27017";
    dbClient.connect(url, function (err, db) {
        cb(err, db);
    })
}]

//新增数据函数
const inserSingle = (collectionName, data, cb) => {
    _connect(function (err, db) {
        const dbs = db.db('test');
        dbs.collection(collectionName).insertOne(data, function (err, result) {
            cb(err, result);
            //dbs.close();
        });
    });
}

//查询数据
const find = (collectionName, data, cb) => {
    var result = [];
    _connect((err, db) => {
        const dbs = db.db('test');
        let cursor = dbs.collection(collectionName).find(data);
        cursor.each((err, doc) => {
            if (err) {
                cb(err, null);
                return;
            }
            if (null != doc) {
                result.push(doc);
            } else {
                cb(null, result);
            }
        })
    });
}

module.exports = {
    inserSingle: inserSingle,
    find: find
}

```
