---
title: node JS
date: 2019-01-07 12:43:18
tags:
categories:
- 개발공부
- 뉴딜과정
---

## node 기초

![](https://dthumb-phinf.pstatic.net/?src=%22https%3A%2F%2Fpostfiles.pstatic.net%2FMjAxODA3MThfMjQ2%2FMDAxNTMxOTAyMzg1MTg2.5rsGwSB2qJ6-wA9AZHK11ndP0jje_KV7oz-dPSeTz_wg._8ylBC4oisCJSn_njxg11g9erRzmICf8sFHyR_lNTRAg.PNG.nbb_pinetree%2FNodejs-Processing-Model.png%3Ftype%3Dw966%22&type=cafe_wa740)



```javascript
var util = require('util');
function Parent() {
}
Parent.prototype.sayHello = function() {
    // parent를 부모로 하는 것들은
    // sayhello 함수를 쓸 수 있게 붙인다
    // prototype 원시타입
    // function 키워드가 class도 생성 가능
 console.log('Hello World, from Parent Class!');
}

var obj = new Parent();
obj.sayHello();
function Child() {
}
// 상속
util.inherits(Child, Parent);

var obj2 = new Child();
obj2.sayHello();
```

1. 기본 모듈 
   <https://nodejs.org/dist/latest-v10.x/docs/api/process.html> 문서 에서 확인

   1. 프로세스 환경 : `os`, `process`
   2. 파일과 경로 , URL : `fs`, `path`, `URL`, `querystring`, `stream`
   3. 네트워크 모듈 : `http`, `net`, `dns` 

2. 전역객체(global)  (Java 에서 Console 클래스 사용하는 것처럼 ....)
   별도의 모듈 로딩 없이 사용가능
   `global.console.log()`  >>  `console.log()`   >> global 생략가능

3. 전역객체 
   1. process
   2. console
   3. Buffer
   4. require
   5. `__filename`, `__dirname`
   6. module
   7. exports
   8. Timeout 함수 등

4. 실습

   ```javascript
   // process.js
   
   console.log(process.env);
   console.log(process.arch);
   console.log(process.platform);
   ```

   ```javascript
   // timeout.js
   function sayHello() {
        console.log('Hello World');
   }
   setTimeout(function() {
        sayHello();
   }, 2*1000);
   ```

   ```javascript
   //interval.js
   function sayGoodbye(who) {
      console.log('Good bye', who);   
   }
   ```

   ```javascript
   //console.js
   var intVal = 3;
   var obj = {
        name : 'NodeJS',
        how : 'Interesting'
   };
   console.log('hello world');
   console.log('intVal : ' + intVal);
   console.log('obj : ' + obj);
   console.log('obj : ', obj);
   setInterval(sayGoodbye, 1 * 1000, 'Friend');
   ```

   ```javascript
   // console_custom.js
   
   var fs = require('fs');
   var output = fs.createWriteStream('stdout.log');
   var errorOutput = fs.createWriteStream('error.log');
   var Console = require('console').Console;
   var logger = new Console(output, errorOutput);
   logger.info('info message');
   logger.log('log message');
   logger.warn('warning');
   logger.error('error message');
   ```

   ```javascript
   // consoleTime.js
   
   console.time('TIMER');
   var sum = 0;
   for(var i = 1 ; i < 100000; i++ ) {
   	sum += i;
   }
   console.log('sum : ', sum);
   console.timeEnd('TIMER');
   ```

   ```javascript
   // util.js
   
   var util = require('util');
   var str1 = util.format('%d + %d + %d', 1, 2, (1+2));
   console.log(str1);
   var str2 = util.format('%s %s', 'Hello', ' NodeJS');
   console.log(str2);
   ```

5. Node.js 애플리케이션의 이벤트들

   1. 클라이언트의 접속 요청

   2. 소켓에 데이터 도착

   3. 파일 오픈/읽기 완료

   4. 이벤트 처리

   5. 비동기 처리

   6. 리스너 함수

      ```javascript
      process.on('exit', function() {console.log('occur exit event');}); 
      process.once('exit', function() {console.log('occur exit event');});
      
      process.emit('exit');  //이벤트 발생
      ```

6. 경로 : path

   ```javascript
   var pathUtil = require('path');
   ```

   1. 경로정보

      - 전역객체
        `__filename`
        `__dirname`
      - 같은 폴더 내 이미지 경로

      ```javascript
      var path = __dirname + '/image.jpg';
      ```

      ```javascript
      // path.js
      
      var pathUtil = require('path');
      var path = 'C:\NewDeal\Script\read.txt';
      
      console.log('dirname : ', pathUtil.dirname(path));
      console.log('basename : ', pathUtil.basename(path));
      console.log('extname : ', pathUtil.extname(path));
      console.log(__dirname);
      console.log(__filename);
      ```

   2. 파일 시스템 다루기 

      - 파일 시스템 모듈 : `fs` 

        ```javascript
        var fs = require(‘fs’); 
        ```

      - 주요 기능

        - 파일 생성/읽기/쓰기/삭제 
        - 파일 접근성/속성 
        - 디렉토리 생성/읽기/삭제 
        - 파일 스트림주의 : 모든 플랫폼에 100% 호환되지 않음 
          (윈도우, 리눅스 파일 경로 다르듯)

      - fs 모듈 사용시

        - 비동기식 : callback 사용

        - 논-블럭 방식

          ```javascript
          fs.readFile('textfile.txt', 'utf8',function(error, data) {}); 
          
          fs.readFile('none_exist.txt', 'utf-8', function(err, data) {
          
                  if ( err ) { console.error('Readfile error ', err); 
          
                 } else { // 정상 처리 } 
          
          });
          ```

        - 동기식 : `readFileSync`

          - 블럭방식 : 성능상 주의

            ```javascript
            var data = fs.readFileSync('textfile.txt', 'utf8'); 
            try { 
            	var data = fs.readFileSync('none_exist.txt', 'utf-8');}
            catch ( exception ) { 
            	console.error('Readfile Error : ', exception); 
             }
            ```

      ```javascript
      var fs = require('fs');
      
      fs.writeFile('textData.txt', 'Hello World', function(err) {
         if ( err ) {
            console.error('Error : ', err);
            return;
         }
         console.log('Write');
      });
      ```

      ```javascript
      // FileRead.js
      
      var fs = require('fs');
      var file = 'read.txt';
      fs.access(file, fs.F_OK, function(err) {
         if ( err ) {
            console.log('파일 없음');
            process.exit(1);      
         }
         else {
            console.log('파일 존재');
            fs.stat(file, function(err, stats) {
            if ( err ) {
                  console.error('File Stats Error', err);
                  return;
             }
                
             console.log('Create : ', stats['birthtime']);
      		console.log('size : ', stats['size']);
      		console.log('isFile : ', stats.isFile());
      		console.log('isDirectory : ', stats.isDirectory());
      		console.log('isBlockDevice : ', stats.isBlockDevice());
      
      		if ( stats.isFile() ) {
                  fs.readFile(file, function(err, data) {
                     if ( err ) {
                        console.error('File Read Error', err);
                        return;
                     }
                     // encoding을 작성하지 않으면 Buffer로
                     var str = data.toString('utf-8');
                     console.log('File Contents : ', str);
                  });               
               }      
            });         
         }   
      });
      ```

7. 스트림 : 데이터 전송 흐름

   - 콘솔 입력/출력

   - 파일 읽기/쓰기

   - 서버/클라이언트 -데이터 전송

   - 스트림종류

     - 읽기 스트림 : Readable Stream

       - 모드 : flowing, paused 
       - flowing mode 
         - 데이터를 자동으로 읽는 모드
         - 전달되는 데이터를 다루지 않으면 데이터 유실 
       - paused mode 
         - 데이터가 도착하면 대기 
         - read() 함수로 데이터 읽기
       - 상태
         - readable : 읽기 가능한 상태
         - data : 읽을 수 있는 데이터 도착 
         - end : 더 이상 읽을 데이터가 없는 상태
         - close : 스트림이 닫힌 상태 
         - error : 에러

       ```javascript
       var is = fs.createReadStream(file);
       
       is.on('readable', function() {
        console.log('== READABLE EVENT');
       });
       
       is.on('data', function(chunk) {
        console.log('== DATA EVENT');
        console.log(chunk.toString());
       });
       
       // end 이벤트
       is.on('end', function() {
        console.log('== END EVENT');
       });
       
       var is = fs.createReadStream(file);
       
       // 'data' 이벤트가 없으면 paused mode
       is.on('readable', function() {
        console.log('== READABLE EVENT');
       
        // 10바이트씩 읽기
        while( chunk = is.read(10) ) {
         console.log('chunk : ', chunk.toString());
        }
       })
       ```

     - 쓰기 스트림 : Writeable Stream

       - 데이터 출력
       - http 클라이언트의 요청 
       - http서버의 응답 
       - 쓰기 스트림
       - tcp 소켓
       - 이벤트
         - drain : 출력 스트림에 남은 데이터를 모두 보낸 이벤트 
         - error : 에러 
         - finish : 모든 데이터를 쓴 이벤트 
         - pipe : 읽기 스트림과 연결(pipe)된 이벤트 
         - unpipe : 연결(pipe) 해제 이벤트

       ```javascript
       var fs = require('fs');
       var os = fs.createWriteStream('output.txt');
       os.on('finish', function() {
        console.log('== FINISH EVENT');
       });
       
       os.write('1234\n');
       os.write('5678\n');
       os.end();
       ```

     - 읽기/쓰기 

     - 변환

8. URL `var url = require(‘url'); `

   - `url.parse(urlStr[, parseQueryString][, slashesDenoteHost]) `
     - urlStr : URL 문자열 
     - parseQueryString : 쿼리 문자열 파싱, 기본값 false 

   ```javascript
   var url = require('url');
   var urlStr = 'http://api.flickr.com/services/feeds/photos_public.gne?tags=raccoon&tagmode=any&format=json&jsoncallback=?';
   var parsed = url.parse(urlStr, true);
   console.log(parsed);
   console.log('protocol : ', parsed.protocol);
   console.log('host : ', parsed.host);
   console.log('query : ', parsed.query);
   ```

## 모듈 만들기

1. 모듈 만들기
   - 소스 코들 분리
   - 모듈 단위로

2. 모듈 작성 방법 `module.exports`

3. 모듈 사용하기

   - 모듈 로딩 : `require`
     `require('mymodule.js');`

   - 모듈 로딩 에러
     `require('mymodule.js');` 

     이 경우 에러가 난다.
     (`./mymodule.js`)  사용자가 생성 한 모듈처리 해야함

4. 모듈 만들기

   ```javascript
   // mymodule.js
   
   module.exports.goodMorning = function() {
    // 모듈 함수 기능 작성
   }
   
   exports.goodNight = function(arg, callback) {
    // module 생략 가능
   }
   ```

5. 사용하기

   ```javascript
   var greeting = require('mymodule.js');
   greeting.goodMorning();
   ```

   > exports 하지 않은 함수는 사용 불가^^
   >
   >
   >
   > 예제)
   >
   > ```javascript
   > // student.js
   > 
   > var student = {
   >  hour : 0,
   >  study : function() {
   >  this.hour++;
   >  console.log(this.hour + '시간째 공부 중');
   >  }
   > };
   > 
   > module.exports = student;
   > ```
   >
   > 사용할 때는
   >
   > ```javascript
   > var you = require('student.js');
   > you.study();
   > ```

## http

### 서버 구동

```javascript
var http = require('http'); 
var server = http.createServer(function(req, res) { 
    res.end(‘Hello World’); 
}).listen(3000);
```

### http 요청( request)

`var server = http.createServer(function(req, res){})`

- req.url : 요청 url, 경로와 쿼리 문자열 
- req.method : 요청 메소드 
- req.headers : 요청 메시지의 헤더 
- req(streamable) : 요청 메시지 바디

### 요청 쿼리 문자열 분석

```javascript

```

### 헤더 메시지 분석

```javascript
var headers = req.headers;
headers.host
headers.content-type
headers.user-agent

var server = http.createServer(function(req, res) {
     console.log(‘HTTP Method : ' + req.method);
     console.log(‘HTTP URL : ' + req.url);
     console.log('== HEADERS ==');
     console.log(req.headers);
     res.end('Hello Node.js');
});
```

### 응답

- 응답 메시지

- 상태 코드와 상태 메시지

- 응답 메시지 헤더

- 응답 메시지 바디
- 메시지 상태
  - response.statusCode
  - response.statusMessage
- 메시지 헤더
  - response.writeHead(statusCode, statusMessage, headers)
    (statusMessage, headers  생략 가능)
  - response.removeHeader(name)
  - response.getHeader(name)
  - response.setHeader(name, value)
- 메시지 바디
  - response.end()
    - data, encoding, callback을 파라미터로 가질 수 있으며 생략도 가능
  - response.write()
    - chunk, encoding, callback을 파라미터로 가질 수 있으며 생략도 가능

```javascript
// 200 OK
response.statusCode = 200;
response.statusMessage = ‘OK’;
// 404 Error
response.statusCode = 404;
response.statusMessage = 'Not found';
```

- 응답 메시지
  `res.writeHead(200, { 'Content-Length': body.length, 'Content-Type': 'text/plain' }); `

- 헤더 작성

  ```javascript
  res.setHeader("Content-Type", "text/html");
  res.setHeader("Content-Length“, body.length);
  ```

- 예제 파일들

  ```javascript
  var http = require('http');
  var server = http.createServer(function(req, res) {
   console.log('Method : ', req.method);
   console.log('url : ', req.url);
   console.log('headers : ', req.headers['user-agent']);
     
   res.write('Hello World');
   res.end();
  }).listen(3000);
  ```

  ```javascript
  var http = require('http');
  var server = http.createServer(function(req, res) {
   res.statusCode = 200;
   res.statusMessage = 'OKOK';
   res.setHeader('content-type','text/plain');  //설정에 따라 Client 화면 다르게 보인다
   
   res.write('<html><body><h1>Hello World</h1></body></html>');
   res.end();
  }).listen(3000);
  ```

  ```javascript
  var http = require('http');
  var fs = require('fs');
  var server = http.createServer(function(req, res) {
   fs.access('cat.jpg', function(err) {\
   //이름 없는 이름 cat1.jpg 테이스 한후에
   if ( err ) {
     res.statusCode = 404;
     res.end();
     return;
    }
    fs.readFile('cat.jpg', function(err, data) {   
     res.end(data);
    });
   });
  }).listen(3333);
  ```

  ```javascript
  var http = require('http');
  var url = require('url');
  var server = http.createServer(function(req, res) {
   // URL 분석 : 쿼리 문자열
   var parsed = url.parse(req.url, true);
      // 이 때 parsed 안에는 프로토콜, 호스트 등의 정보가 담긴 json 객체
   var query = parsed.query;
      // 맵 구조로 돌아온다.
      // http://127.0.0.1:3000/cal?start=1&end=10
      // 이런 url을 치면 실행됨
      // 이 때 query 안에는 start 키와 값, end 키와 값이 담긴 json 객체
  
   // start와 end를 사용해 파라미터 값을 받을 수 있다
   var start = parseInt(query.start);
      // 파라미터 이름이 start인 값
   var end = parseInt(query.end);
      // 파라미터 이름이 end인 값
   
   if ( !start || !end ) {
       // 파라미터가 없으면
    res.statusCode = 404;
    res.end('Wrong Parameter');
   }
   else {
    // 합계 구하기
    var result = 0;
    for(var i = start ; i < end ; i++) {
     result += i;
    }
    res.statusCode = 200;
    res.end('Result : ' + result);
   }
  
  }).listen(3000);
  ```

## 정적 파일 서비스

```javascript
var server = http.createServer(function(req, res) {
 if ( req.url == '/favicon.ico' ) {}
 else if ( req.url == '/image.png' ) {
 res.writeHeader(200, {‘Content-Type':'image/png'});
 fs.read…
 }
 else if ( req.url == '/music.mp3' ) {
 res.writeHead(200, {'Content-Type':'audio/mp3'});
 fs.createReadStream…
 }
 else if ( req.url == '/movie.mp4' ) {
 res.writeHead(200, {'Content-Type':'video/mp4'});
 fs.createReadStream…
 }
});
```

- 요청 URL의 경로를 실제 파일 경로 매핑

  - myServier.com/resource/image.png -> ./resources/image.png
  - myServier.com/resource/audio.mp3 -> ./resources/audio.mp3

- 요청 URL에서 경로 생성

  ```javascript
  var pathUtil = require('path');
  var path = __dirname + pathUtil.sep + 'resources' + req.url;
  // + 로 해야한다!
  ```

## GET , POST 다루기

- 중복 POST 요청 방지

  - POST 요청 처리 후 redirect 응답

  - PRG(Post-Redirect-Get) 패턴

  - 리프레쉬 – Get 요청 중복(OK)

  - 응답 메시지 작성 코드

    - Redirection : 클라이언트 주소 옮기기
      - 상태코드 : 302
      - 헤더 필드 : Location

  - PRG 패턴 적용 코드

    ```javascript
    req.on('end', function() {
    // POST 요청 메세지 바디 분석/처리
     res.statusCode = 302;
     res.setHeader(‘Location’, URL);
     res.end();
    });
    ```

- 폼 인코딩 방식(enctype)

  - application/x-www-form-urlencoded  (default)
  - multipart/form-data (파일 전송)
  - text/plain

- 멀티파트 방식

  ```jsp
  <form method="post" action="upload"
   enctype="multipart/form-data">
  </form>
  ```

-  post 요청

  - 요청 데이터 얻기

    ```javascript
    var body = '';
    request.on('data', function(chunk) {
     console.log('got %d bytes of data', chunk.length);
     body += chunk;
    });
    request.on('end', function() {
     console.log('there will be no more data.');
     console.log('end : ' + body);
    });
    ```

  - 데이터 처리

    ```javascript
    request.on('end', function() { 
    var parsed = querystring.parse(body); 
    console.log(‘name1 : ' + parsed.name1); 
    console.log(‘name2 : ' + parsed.name2);
     }); 
    ```

- 실습

  ```javascript
  var http = require('http');
  var querystring = require('querystring');
  // 파라미터를 뽑아낼 수 있는 모듈
  var movieList = [{title : '비트', director : '뉴딜'}];
  var server = http.createServer(function(req, res) {
      if(req.method.toLowerCase() == 'post') {
          // post 방식으로 전송되었다면
          addNewMovie(req, res);
      } else {
          // get 방식으로 전송되었다면
          showList(req, res);
      }
  });
  server.listen(3000);
  // 3000번 포트 사용
  
  function showList(req, res) {
      res.writeHeader(200, { 'Content-Type': 'text/html; charset=UTF-8' });
      // 포트 번호, 클라이언트에게 응답할 뷰단 설정
      res.write('<html>');
      res.write('<meta charset="UTF-8">');
      res.write('<body>');
  
      res.write('<h3>Favorite Movie</h3>');
      res.write('<div><ul>');
  
      movieList.forEach(function (item) {
          // each문과 비슷한 역할
          res.write('<li>' + item.title + '(' + item.director + ')</li>');
      }, this);
      res.write('</ul></div>');
  
      res.write(
          '<form method="post" action="."><h4>새 영화 입력</h4>' +
          '<div><input type="text" name="title" placeholder="영화제목"></div>' +
          '<div><input type="text" name="director" placeholder="감독"></div>' +
          '<input type="submit" value="upload">' +
          '</form>'
          );
      res.write('</body>');
      res.write('</html>');
      res.end();
  }
  
  function addNewMovie(req, res) {
      var body = '';
      req.on('data', function(chunk) {
          // 데이터 들어오면 발생하는 이벤트 부착
          body += chunk;
      });
      // PRG 방식, 데이터 들어올 때마다 누적
      req.on('end', function() {
          // 데이터 다 들어오면 발생하는 이벤트 부착
          var data = querystring.parse(body);
          // 데이터는 json 객체
          var titledata = data.title;
          var directordata = data.director;
  
          // 배열에 넣기
          // 자바스크립트의 배열은 스택구조임
          // 때문에 스택의 메서드를 사용할 수 있음
          movieList.push({title:titledata, director:directordata});
          
          // 이렇게 해도 된다
          movieList.push(data);
          // json 객체 push
          res.end('success');
      });
  }
  ```

  ```javascript
  // 하단에 처리를 이렇게 해도 된다.
  res.statusCode = 302;
  res.setHeader('Location', '.');
  // 화면 재요청, get 방식으로 요청했을 때의 함수를 탄다
  res.end();
  ```

## node를 이용한 파일 업로드

### `formidable`을 이용한 멀티파일 분석 모듈 이용

```
npm install formidable
```

#### IncomingForm의 이벤트

- `field`      >  이름/값 도착 이벤트
- `file`       > 파일 도착 이벤트
- `aborted`  >  요청 중지(클라이언트)
- `end`      >  종료

#### IncomingForm의 프로퍼티

 form.uploadDir 

 form.keepExtension 확장자 보존 

 form.multiples 다중 파일 업로드

```javascript
form.parse(req, function(err, fields, files) {}
           // fields : 이름 - 값(fields.이름) 데이터
           // files : 업로드 파일 정보
```

업로드 파일 정보 : `Formidable.File `

> file.size 업로드 된 파일의 크기(바이트) 
>
> file.path 파일 경로 
>
> file.name 파일 이름 
>
> file.type 파일 타입 
>
> file.lastModifiedDate 최종 변경일 
>
> file.hash 해쉬값

#### 파일 업로드 서비스 구현

> 파일 업로드(formidable) - 임시 폴더 
>
> 파일 업로드 후 
>
> -파일을 임시 폴더 -> 리소스 저장소로 이동 
>
> -리소스 저장소에서 이름이 충돌되지 않도록 이름 변경 
>
> -날짜, 일련번호, 사용자 계정, ...