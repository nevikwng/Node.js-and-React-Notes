# EJS

類似於Handlebars的模板引擎

好處為語法即為javascript使用上較為直覺

http://www.embeddedjs.com/
https://www.npmjs.com/package/ejs
https://github.com/tj/ejs

##使用

安裝好Express後用

```

app.set('view engine', 'ejs');
```
引入即可，之後創建views資料夾，資料夾內檔案後檔名取.ejs

##!!如出現找不到ejs module
1.原因為如果是使用-g的express必須將ejs也安裝在-g

2.或是將express和ejs都安裝在loacl

```
<% %>同一行有HTML tag 則加等號 <%= %>

```