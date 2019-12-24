# MongoDB数据库结合开发
	
###### 一、利用Mongodb包连接MongoDB

``` .js
const dbClient = require("mongodb");
const _connect = function (cb) {
    var url = "mongodb://127.0.0.1:27017";
    dbClient.connect(url, function (err, db) {
        cb(err, db);
    })
};

//新增数据
const inserSingle = function (collectionName, data, cb) {
    _connect(function (err, db) {
        const dbs = db.db('test');
        dbs.collection(collectionName).insertOne(data, function (err, result) {
            cb(err, result);
            dbs.close();
        });
    });
}

//查询数据
const findAllData = function (collectionName, data, cb) {
    var result = [];
    if (3 != arguments.length) {
        cb("当前函数需要三个参数!", null);
        return;
    }
    _connect(function (err, db) {
        const dbs = db.db('test');
        let cursor = dbs.collection(collectionName).find(data);
        cursor.each(function (err, doc) {
            if (err) {
                cb(err, null);
                return;
            }
            if (null!=doc) {
                result.push(doc);
            } else {
                cb(null, JSON.stringify(result));
            }
        })
    });
}
exports.inserSingle = inserSingle;
exports.findAllData = findAllData;
```
