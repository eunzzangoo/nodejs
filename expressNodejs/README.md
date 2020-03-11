# Node.js + Express + Bootstrap + MongoDB

> ### 최초 작성일 : 2020년 03월 11일



Node.js, Express, MongoDB,  Bootstrap을 사용하여 기본 RESTful CRUD 구조를 만들것입니다. 

그리곤 Postman을 사용하여 테스트를 해볼것입니다.  


## 1. 설치하기

```shell
npm install express-generator -g #express 설치
npm install nodemon -g #수정할때마다 자동으로 재실행해주는 모듈
express --view=ejs [폴더명] #ejs view를 엔진으로 하는 express 프로젝트 뼈대 생성
cd [폴더명] #생성된 프로젝트로 이동
npm install #package.json이 의존하고 있는 모듈을 한번에 설치
npm install mongoose body-parser method-override  --save #mongoose, body-parser, method-override 설치
```

참고로 --save옵션은 npm version 5이상 default값이라 안써줘도 된다고 합니다.

* Body-paser는 post로 요청된 body를 쉽게 추출하기 위해 설치한다.
* Metod-override 는 REST API를 사용하기 위해 설치합니다. 기본적으로 POST만 제공 된다. PUT, DELETE 추가하기 위해!  


## 2. bootstrap 설치

Bootstrap은 아래와같이 로컬환경에 파일을 두는 방법이있고 CDN을 이용하는 방법이있다. 트래픽을 절약하기 위해 CDN을 사용하겠습니다.

```shell
npm install bootstrap jquery popper.js #bootstrap jquery popper.js 설치
```

```js
<!-- Bootstrap Core CSS -->
<link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css">
<!-- Bootstrap core JavaScript -->
<script rel="stylesheet" type="text/JavaScript" src="/js/bootstrap.min.js"></script>
```

```js
<!-- app.js -->
app.use('/js', express.static(path.join(__dirname, '/node_modules/bootstrap/dist/js')));
app.use('/css', express.static(path.join(__dirname, '/node_modules/bootstrap/dist/css')));
```



[bootstrap download page](http://bootstrapk.com/getting-started/#download) 에서  CDN 내용 확인

```js
<!-- CDN 기본코드 -->
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
  </body>
</html>
```

+ 참고로 유용하게 [무료테마 bootswatch](https://bootswatch.com/_) 제공되는 사이트이다. 다운받아서 css파일에 덮어주거나 CDN을 사용한다면 herf경로를 파일 경로로 바꿔주면 된다.  


## 3. view layout 나눠주기

bootstrap에서 제공하는 cover theme를 적용해 보았다. 또한 뷰 에서 재사용의 용이성을 높이기 위해 Partials 폴더를 만들어서 head.ejs, footer.ejs, script.ejs 부분을 나눠주었다

* Head.ejs

```ejs
 <!-- Required meta tags -->
 <meta charset="utf-8">
 <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
 <!-- Bootstrap CSS -->
 <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
 <link rel='stylesheet' href='/stylesheets/style.css' />
 <link rel='stylesheet' href='/stylesheets/cover.css' />
```

* Footer.ejs

```ejs
<footer class="mastfoot mt-auto">
    <div class="inner">
      <p>Cover template for <a href="https://getbootstrap.com/">Bootstrap</a>, by <a href="https://github.com/eunzzangoo">@eunzzangoo</a>.</p>
    </div>
  </footer>
```

* Script.ejs

```ejs
<!-- Optional JavaScript -->
<!-- jQuery first, then Popper.js, then Bootstrap JS -->
<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
```

* Index.ejs

```ejs
<!DOCTYPE html>
<html>
  <head>
    <% include partials/head %>
  </head>
  <body class="text-center">

    <div class="cover-container d-flex h-100 p-3 mx-auto flex-column">
      <header class="masthead mb-auto">
        <div class="inner">
          <h3 class="masthead-brand">Cover</h3>
          <nav class="nav nav-masthead justify-content-center">
            <a class="nav-link active" href="#">Home</a>
            <a class="nav-link" href="#">Features</a>
            <a class="nav-link" href="#">Contact</a>
          </nav>
        </div>
      </header>

      <main role="main" class="inner cover">
        <h1 class="cover-heading">Cover your page.</h1>
        <p class="lead">Cover is a one-page template for building simple and beautiful home pages. Download, edit the text, and add your own fullscreen background photo to make it your own.</p>
        <p class="lead">
          <a href="#" class="btn btn-lg btn-secondary">Learn more</a>
        </p>
      </main>
      <% include partials/footer %>
    </div>
    <% include partials/script %>
  </body>
</html>
```



현재까지의 tree 구조입니다.

```
├── app.js
├── bin
│   └── www
├── node_modules
├── package-lock.json
├── package-lock.json
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       ├── cover.css
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.ejs
    ├── index.ejs
    └── partials
        ├── footer.ejs
        ├── head.ejs
        └── script.ejs
```  


## 4. Mongoldb 연동

App.js에 mongoose 모듈을 가져오고 셋팅합니다.

```js
var mongoose = require('mongoose'); 

//database
mongoose.connect('mongodb://localhost/test_db', {
  useNewUrlParser: true
}).then(() => {
  console.log("Successfully connected to the database");    
}).catch(err => {
  console.log('Could not connect to the database. Exiting now...', err);
  process.exit();
});
```



오늘은 여기까지...

