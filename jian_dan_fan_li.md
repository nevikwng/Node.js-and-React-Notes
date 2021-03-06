# 簡單範例

## 簡單範例

## 使用表單post傳送資料

1.加入express

```text
var express = require("express");
var app = express();
```

2.加入模板引擎Handlebars

```text
var exphbs  = require('express-handlebars');
app.engine('hbs', exphbs());
app.set('view engine', 'hbs');
```

3.設定監聽port與簡單路由

```text
app.get("/",function(req,res){

res.render('home');
});
app.post("/signup",function(req,res){
req.on("data",function(data){
    console.log(data.toString());
});
});

app.listen("3000",function(){
    console.log("listening3000");
});
```

根目錄下建造一個views資料夾 裡面放入home.hbs

```text
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Example App</title>
</head>
<body>

    <form id="signup" method="POST" action="http://localhost:3000/signup">  
      <label>使用者名稱：</label><input type="text" id="username" name="username" /><br>  
      <label>電子郵件：</label><input type="text" id="email" name="email" /><br>  
      <input type="submit" value="註冊我的帳號" /><br>  
    </form> 
</body>
</html>
```

點擊按鈕後即可看到console

使用bodyParser

```text
var bodyParser = require('body-parser')
```

不用使用on去監聽

```text
app.post("/signup",function(req,res){
console.log(req.body);
res.end();
});
```

記得導回頁面，和結束回應

```text
res.redirect('/')
res.end();
```

## 延續前兩章

現在專案目錄如下

```text
 index.js

 package.json

|| node_modules

|| public

|| routes----index.js
```

根目錄的index.js為

```text
var express = require('express');

////
var mongo = require('mongodb');
var Server = mongo.Server;
var Db = mongo.Db;

var server = new Server('ds013898.mlab.com',13898, {auto_reconnect : true});
var db = new Db('forclass', server);


db.open(function(err, client) {
    client.authenticate('forclass1', 'test123', function(err, success) {
        if(success){
        console.log("connect success")
      }else{
        console.log("client.authenticate error")
      };

    });
});

////
var app = express();
var router = require('./routes/index.js')(app);
app.use(express.static(__dirname + '/public'));/* 將預設路徑設在public*/




app.listen(8080);
```

routes下的index.js為

```text
module.exports = function (app) {

app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.send('Helloo worldd');

  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```

public資料夾為空

## 使用Handlebars 當模板引擎

```text
npm install express-handlebars
```

在根目錄的index.js加入

```text
var exphbs  = require('express-handlebars');
```

之後再下面再加入\(記得加在var app = express\(\);後\)

```text
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');
```

再把routes中的index.html裡面的這段改為

```text
 app.get('/', function (req, res) {
    res.render('main');///從send改為render

  });
```

最後創建views資料夾於根目錄，裡面放入main.handlebars

```text
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
    <h1>title</h1>
    <p>Welcome to Handlebars</p>
  </body>
</html>
```

資料夾名稱必須為views，因為那是express預設的模板資料夾

重啟server即可看到畫面。

### 之後我們試著讓render時傳入參數

routes內的index.js改為

```text
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm Title"});

  });
```

views內的main.handlebars改為

```text
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
    <h1>{{title}}</h1>
    <p>Welcome to Handlebars</p>
  </body>
</html>
```

即可看到，產生的畫面資料變為render提供的第二個物件參數的值

### 使用簡單的條件判斷render

先在routes的index.js

```text
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle",title1:false});

  });
```

main.handlebars

```text
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>

  {{#if title1}}
    <h1>It's true</h1>
  {{else}}
    <h1>It's false</h1>
  {{/if}}

    <h1>{{title}}</h1>

    <p>Welcome to Handlebars</p>
  </body>
</html>
```

之在在把

```text
res.render('main',{title:"I'm fhTitle",title1:false});
```

title1的false改為true看看

### 使用each再render時 去遍歷陣列

```text
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle", people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]});
```

```text
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>
 {{#each people}}
    <li>{{this}}</li>
  {{/each}}

    <h1>{{title}}</h1>

    <p>Welcome to Handlebars</p>
  </body>
</html>
```

### 測試ES6的\[...\]方法

```text
module.exports = function (app) {
var people= [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ];

  console.log([...people]);



app.get('/hi', function (req, res) {
  res.send('Hiiii!');
});
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle", people: [
    "Yehuda Katz",
    "Alan Johnson",
    "Charles Jolley"
  ]});

  });
  app.get('/customer', function(req, res){
    res.send('customer page');
  });
  app.get('/admin', function(req, res){
    res.send('admin page');
  });

};
```

發現直接執行會錯誤，所以要改為

```text
node --harmony index
```

有關Node.js目前支援的ES6方法可參考 [https://nodejs.org/en/docs/es6/](https://nodejs.org/en/docs/es6/)

## render時加入function

```text
 app.get('/', function (req, res) {
    res.render('main',{title:"I'm fhTitle", people:(function(){return 5})() });

  });
```

```text
<!DOCTYPE html>
<html>
  <head>
    <title>test</title>

  </head>
  <body>

    <li>{{people}}</li>


    <h1>{{title}}</h1>

    <p>Welcome to Handlebars</p>
  </body>
</html>
```

## 簡單範例part2

```text
var express = require("express");
var app = express();
var exphbs  = require('express-handlebars');
var bodyParser = require('body-parser')
var mongoose = require("mongoose");
mongoose.connect('mongodb://forclass1:test123@ds013898.mlab.com:13898/forclass',function(err){
    if(err){throw err};
});
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log("connect mongo")
});
var Cat = mongoose.model('Dog', {
  name: String,
  time: String
});

/*var kitty = new Cat({ name: 'Zildjian', time:'12:00'});
//使用save方法後才會存入
kitty.save(function (err) {
  if (err) // ...
  console.log('meow');
});*/


//-----
app.engine('hbs', exphbs());
app.set('view engine', 'hbs');

app.use(bodyParser.urlencoded({ extended: false }))
app.get("/",function(req,res){

res.render('home');
});


app.post("/signup",function(req,res){

    var kitty = new Cat({ name:req.body.username, time:req.body.email});

    kitty.save(function (err) {
      if (err) // ...
      console.log('save err');
    });
    res.redirect('/read')
    res.end();
});


app.post("/updatelist",function(req,res){

    Cat.update({_id:req.body.id[0]},{time:req.body.event[0],name:req.body.event[1]},function(err){
        if(err){
        console.log(err);
         }
    });
        res.redirect('/read');
        res.end();
});




app.post("/deletelist",function(req,res){


    Cat.remove({_id:req.body.id},function(err){
        if(err){throw err};
    });

        res.redirect('/read');
        res.end();
});


app.get("/read",function(req,res){

    var find = Cat.find({},{name:1,time:1,_id:1},function(err,doc){
    res.render("home",{text:doc});
    });

});



app.listen("3000",function(){
    console.log("listening3000");
});
```

```text
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Example App</title>
</head>
<body>

    <form id="signup" method="POST" action="http://localhost:3000/signup">  
      <label>事件：</label><input type="text" id="username" name="username" /><br>  
      <label>時間：</label><input type="text" id="email" name="email" /><br>  
      <input type="submit" value="加入計畫" /><br>  

    </form> 

     <form method="POST" action="http://localhost:3000/updatelist">
        <label>要更改的計畫：<br></label>時間:<input type="text" id="changelist1" name="event" /><input type="text" id="changeid" name="id" readonly/><br>

       事件:<input type="text" id="changelist2" name="event" /><input type="text" id="changeid1" name="id" readonly/><br>
       <input type="submit" value="更改計畫" /><br>   
    </form>




    <form method="POST" action="http://localhost:3000/deletelist">
        <label>要移除的計畫：</label><input type="text" id="deletelist1" name="event" readonly/><input type="text" id="deleteid1" name="id" readonly/><br>
       <input type="submit" value="移除計畫" /><br>  
    </form>

    <div style="position:absolute;right:50px;top:20px;">
    <select onchange="deletelist(this.options[this.selectedIndex].value,this.options[this.selectedIndex].id)">
    {{#each text}}
    <option id="{{_id}}">時間:{{time}}事件:{{name}}</option>
    {{/each}}
    </select>
    </div>


    <script>

    function deletelist(value,id){
        document.getElementById("changelist1").value = value.slice(3,value.lastIndexOf("件")-1);
        document.getElementById("changelist2").value = value.slice(value.lastIndexOf(":")+1);
        document.getElementById("changeid").value = id;
        document.getElementById("changeid1").value = id;
        document.getElementById("deletelist1").value = value;
        document.getElementById("deleteid1").value = id;

    };
    </script>
</body>
</html>
```

