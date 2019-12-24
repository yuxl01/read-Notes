# MongoDB数据库结合开发
	
###### 一、利用Mongodb包连接MongoDB

``` .js
//npm i mongodb
const dbClient = require("mongodb");
const _connect = function(cb){
    var url = "mongodb://127.0.0.1:27017";
    dbClient.connect(url,function(err,db){
        cb(err,db);
    })
};

const inserSingle =function(dbName,collectionName,data,cb){
    _connect(function(err,db){
        const dbs = db.db(dbName);
        dbs.collection(collectionName).insertOne(data,function(err,result){
            cb(err,result);
            dbs.close();
        });
    });
}
    
exports.inserSingle = inserSingle;

```