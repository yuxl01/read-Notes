# MongoDB数据库结合开发

  `npm下载包 Npm -install mongodb`
	
###### 一、利用Mongodb包连接MongoDB

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
