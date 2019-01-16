---
title: 몽고DB
date: 2019-01-16 09:39:18
tags:
categories:
- 개발공부
- 뉴딜과정
---

## 몽고 DB

[세팅](http://mainia.tistory.com/5738)

```
mongod.exe --dbpath data_폴더_경로
```

- 몽고DB의 포트번호는 : 27017

[GUI 툴](https://robomongo.org/)

[GUI 세팅](http://javacpro.tistory.com/65)

### 노드에서 사용하기

1. 일단 `npm install mongodb`로 드라이버를 설치한다

2. 데이터 베이스를 얻는다

   ```javascript
   var MongoClient = require('mongodb').MongoClient;
   
   MongoClient.connect(url,options,callback);
   ```

3. 데이터베이스와 연결

   ```javascript
   var MongoClient = require('mongodb').MongoClient
     var url = 'mongodb://localhost:21017/데이터베이스이름';
   
     var db;
     MongoClient.connect(url,function(err,database){
        
     });
   ```

### 컬렉션 다루기

   ```javascript
db.COLLECTION.insert
db.COLLECTION.find
   
ex) 노드에서
   var movies = db.collection('movies');
	movies.insert();

ex) gui에서
   db.컬렉션명.insert({name:'euna'})
   db.getCollection('컬렉션명').find({})
   ```

### 튜플, 로우 추가(insert)

```javascript
insert(document,options,callback)
insertMany(document,options,callback)
insertOne(document,options,callback)
```

#### insert 함수 결과

- `result` : MongoDB의 insert  결과

- `ops`    :  _id를 포함한 새로 추가된 Document(row) 정보

- `connection` : insert 함수가 동작한 연결 정보

  ```javascript
  function addMovie(req,res){
    var movies =db.collection('movies');
    movies.insert({title:'인스',director:'크리스' ,year:2018},
    function(err,result){
      var result = {
                    result : 'success',
                    newId : result.insertedIds[0]
                    };
      res.json(result);
     }
    );
  }
  ```

- insertOne

  ```javascript
  var MongoClient = require('mongodb').MongoClient;
  var url = "mongodb://localhost:27017/";
  
  MongoClient.connect(url, function(err,db){
         if(err) throw err;
         var dbopen = db.db("mymongodb");
         var obj = {title:"new" , day:"everyday" , importance : "high"};
      dbopen.collection("customers").insertOne(obj,function(err,res){
          if(err) throw err;
          console.log("one data insert");
          db.close();
       });
  });
  ```

- insertMany

  ```javascript
  var MongoClient = require('mongodb').MongoClient;
  var url = "mongodb://localhost:27017/";
  
  MongoClient.connect(url, function(err,db){
         if(err) throw err;
         var dbopen = db.db("mymongodb");
         var obj = [ 
                     {title:"new" , day:"everyday" , importance : "high"},
       {title:"new" , day:"everyday" , importance : "high"},
                     {title:"new" , day:"everyday" , importance : "high"},
                     {title:"new" , day:"everyday" , importance : "high"}, 
                   ];
         dbopen.collection("customers").insertMany(obj,function(err,res){
          if(err) throw err;
          console.log("multi data insert" + res.insertedCount);
          // 몇 건이 들어갔는지 insertedCount로 나온다
          db.close();
       });
  });
  ```

#### data 추가

##### 콜백 기반

```javascript
MongoClient.connect(url,function(err,db){
      var movies = db.collection('movies');
      
      movies.insert(
         {title:'인스',director:'크리스' ,year:2018},
         function(err,results){
              //if(err) ...
              console.log(results);
              db.close();
          }
       );
});
```

##### 프로미스 기반

```javascript
MongoClient.connect(url,function(err,db){
	var movies = db.collection('movies');
      
	movies.insert({title:'인스',director:'크리스' ,year:2018}).then(function(result){
	console.log(result);                     
	},function(err){
	console.log(err);
  });
});
```

### select

`Document find() >> select * , select * ...where id=`

- find(query) -> Cursor (data 집합)

  - each , forEach 결과 도큐먼트를 순회

  ```javascript
  var cursor = colletion.find(...);
      cursor.forEach(function(doc){
        
      },function(err){
  
      });
  ```

  - toArray() 도큐먼트의 배열 반환

  ```javascript
  MongoClient.connect(url,function(err.db){
      //컬렉션 얻기
      var movies = db.collection('movies');
      //2000 년이후   > 2000
      movies.find({year:{$gt:2000}}).toArray(function(err,docs){
          //{year:{$gt:2000}} : year가 2000년 이후인 조건
          //docs 배열 객체
            for(var i = 0 ; i < docs.length ; i++){
                var doc = doc[i];
                console.log(doc['title'] , doc['director']);
            }
        }
       
     });
  ```

- findOne(query,options,callback)

  - 이 경우 결과 값이 하나라고 생각하기에 순회를 돌지 않는다
  - 맨 처음 발견되는 하나의 값만이 리턴된다

- firstOne

  - 상위에 있는 한 건의 데이터만 조회

  ```javascript
  var MongoClient = require('mongodb').MongoClient;
  var url = "mongodb://localhost:27017/";
  
  MongoClient.connect(url, function(err,db){
         if(err) throw err;
         var dbopen = db.db("mymongodb");
         dbopen.collection("customers").firstOne({},function(err,result){
          if(err) throw err;
            console.log(result);  //또는 result.title  식으로 원하는 column 데이터만 출력 가능
            db.close();
        });    
  });
  ```

- 정규표현식을 이용한 조건 조회

  ```javascript
  var MongoClient = require('mongodb').MongoClient;
  var url = "mongodb://localhost:27017/";
  
     MongoClient.connect(url, function(err,db){
         if(err) throw err;
         var dbopen = db.db("mymongodb");
         var query = {title : /^e/};
         dbopen.collection("customers").find(query).toArray(function(err,result){
            if(err) throw err;
            console.log(result);
            db.close();
         });
       });
       
  // e로 시작하는  .... title 이 e로 시작하는 데이터
  ```

### sort

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var mysort = { name: 1 };
  // 이 경우 name 오름차순
  // -1이 내림차순
    dbo.collection("customers").find().sort(mysort).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err,db){
       if(err) throw err;
       var dbopen = db.db("mymongodb");
       var query = {title : 1};  //title 기준 오름 차순 정렬하기   >>   {title : -1}  내림차순 정렬
       dbopen.collection("customers").find().sort(sort).toArray(function(err,result){
          if(err) throw err;
          console.log(result);
          db.close();

       });
});
```

### delete

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: 'Mountain 21' };
  dbo.collection("customers").deleteOne(myquery, function(err, obj) {
    if (err) throw err;
    console.log("1 document deleted");
    db.close();
  });
});
```

### update

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err,db){
       if(err) throw err;
       var dbopen = db.db("mymongodb");
       var query = {title : "supeman"};  //검색
       var newvalues = {$set : {day:"update week"}}
       //변경
       dbopen.collection("dbopen").updateOne(query,newvalues,function(err,result){
          if(err) throw err;
          console.log(result);
          db.close();

       });
});
```

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err,db){
       if(err) throw err;
       var dbopen = db.db("mymongodb");
       var query = {title :/^e/};  //검색
       var newvalues = {$set : {day:"update week"}} //변경
       dbopen.collection("dbopen").updateOne(query,newvalues,function(err,result){
          if(err) throw err;
          console.log(result);
          db.close();

       });
});
```

### limit

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err,db){
       if(err) throw err;
       var dbopen = db.db("mymongodb");
       var query = {title : "supeman"};  //검색
       var newvalues = {$set : {day:"update week"}} //변경
       dbopen.collection("customers").find().limit(3).toArray(function(err,result){
          if(err) throw err;
          console.log(result);
          db.close();

       });
});
```

### join(`$lookup`을 통한 left outer join)

- 데이터 예시

  ```javascript
  //orders
  [
    { _id: 1, product_id: 154, status: 1 }
  ]
  // products
  [
    { _id: 154, name: 'Chocolate Heaven' },
    { _id: 155, name: 'Tasty Lemons' },
    { _id: 156, name: 'Vanilla Dreams' }
  ]
  ```

- 실행

  ```javascript
  var MongoClient = require('mongodb').MongoClient;
  var url = "mongodb://127.0.0.1:27017/";
  
  MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("mydb");
    dbo.collection('orders').aggregate([
      { $lookup:
         {
           from: 'products',
           localField: 'product_id',
           foreignField: '_id',
           as: 'orderdetails'
         }
       }
      ]).toArray(function(err, res) {
      if (err) throw err;
      console.log(JSON.stringify(res));
      db.close();
    });
  });
  ```

- 결과

  ```javascript
  [
    { "_id": 1, "product_id": 154, "status": 1, "orderdetails": [
      { "_id": 154, "name": "Chocolate Heaven" } ]
    }
  ]
  ```

### drop

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err,db){
       if(err) throw err;
       var dbopen = db.db("mymongodb");
    
  dbopen.collection("customers").drop(function(err,delok){  // dbopen.dropCollection("customers",function(){})
          if(err) throw err;
          if(delok) console.log("collection deleted");
          db.close();

       });
});
```

### MongoDb Query 특성  

- MongoDB에서 모든 쿼리는 단일 컬렉션에서 사용된다.   

- 사용자는 limit, skips 및 sort order를 사용하여 쿼리를 수정할 수 있다.   

- sort()가 설정되지 않으면 쿼리에서 반환된 다큐먼트 순서는 정의되지 않는다.   

- 기존의 다큐먼트를 update하는 동작은 Query가 갱신하려는 다큐먼트를 선택하는 것과 동일한 쿼리 문법을 사용한다.   

- $match 파이프라인 작업은 집계 파이프라인에서 MongoDB 쿼리에 접근한다

#### Projection 

MongoDB에서 쿼리는 기본적으로 매칭된 모든 다큐먼트 내부의 필드를 반환합니다.   

따라서 MongoDB가 애플리케이션으로 전송하는 데이터의 양을 줄이기 위해 쿼리에 **프로젝션**을 추가하여 사용할 수 있습니다. 

- field : 1 //출력 필드 지정

- field : 0// 출력 필드 제외

```javascript
db.getCollection('student').find({student_:id{$gt:10}}, {"homework":0, "score":0});

// homework와 score 필드 제외됨
// 1을 주면은 1을 쓴 필드만 나오므로 주의
// _id 필드는 무조건 나온다(1을 주지 않더라도 디폴드 값임, 없애려면 명시적으로 0을 준다)
```

즉 제한할 수 있다.

##### 특성

- 기본적으로 `_id` 필드는 결과에 포함된다. 결과 셋에서 `_id` 필드를 제거하려면 프로젝션 다큐먼트에서 `_id`를 0으로 설정해야 한다.   

- 몽고 DB는 배열이 포함된 필드에서 `$elemMatch`, `$slice`, `$`와 같은 프로젝션 연산자를 제공한다.

##### ex)

1. 결과 셋에서 1개 필드 추출하기
   `db.grades.find( {"student_id" : {$lt : 42} }, {"homework" : 0})  `
2. 2개 필드와 _id 필드를 추출하기
   `db.grades.find( {"student_id" : {$lt : 42} }, {"type" : 1, "score" : 1 })`
3. `_id` 필드를 제외하고 2개 필드 추출하기
   `db.grades.find( {"student_id" : {$lt : 42} }, {"_id" : 0, "type" : 1, "score" : 1 })`

#### 도큐먼트 개수 가져오기

```javascript
db.collection.count(query , options, callback);
```

