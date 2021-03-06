# AWS S3

1. 申請IAM，取得access key 和 secret key
2. 下載aws-sdk

官方文件：[https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html\#createBucket-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property)

## 上傳到 S3 範例

> 以下程式開啟後會先當一個範例上傳網站，然後可以在此上傳檔案，之後會傳到S3，使用的上傳檔案模組為busboy

```javascript
var http = require('http'),
    path = require('path'),
    os = require('os'),
    fs = require('fs');

var Busboy = require('busboy');
const AWS = require('aws-sdk');

// 填入已經創建的S3 bucket名稱
const BUCKET_NAME = 'marketplace-model';

// 從IAM獲得
const IAM_USER_KEY = 'key';
const IAM_USER_SECRET = 'secret';

function uploadToS3(file, filename) {
 let s3bucket = new AWS.S3({
   accessKeyId: IAM_USER_KEY,
   secretAccessKey: IAM_USER_SECRET,
   Bucket: BUCKET_NAME,
 });
 s3bucket.createBucket(function () {
   var params = {
    Bucket: BUCKET_NAME,
    Key: filename,
    Body: file,
    ACL: "public-read"
   };
   s3bucket.upload(params, function (err, data) {
    if (err) {
     console.log('error in callback');
     console.log(err);
    }
    console.log('success');
    console.log(data);
   });
 });
}


http.createServer(function(req, res) {
    if (req.method === 'POST') {
    var busboy = new Busboy({ headers: req.headers });
    busboy.on('field', function(fieldname, val, fieldnameTruncated, valTruncated, encoding, mimetype) {
      folder_name = val
    });
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      // busboy讀完的是stream，需要轉為buffer然後上傳到S3
      var bufs = [];
      file.on('data', function(data) {
        bufs.push(data);
      });
      file.on('end', function() {
        var buf = Buffer.concat(bufs);
        uploadToS3(buf, filename);
      });
    });
    busboy.on('finish', function() {
      res.writeHead(200, { 'Connection': 'close' });
      res.end("That's all folks!");
    });
    return req.pipe(busboy);
  } else if (req.method === 'GET') {
    res.writeHead(200, { Connection: 'close' });
    res.end('<html><head></head><body>\
               <form method="POST" enctype="multipart/form-data">\
                <input type="text" name="textfield"><br />\
                <input type="file" name="filefield"><br />\
                <input type="submit">\
              </form>\
            </body></html>');
  }
}).listen(8000, function() {
  console.log('Listening for requests');
});
```

> 在createBucket的params的參數key前面加上路徑/test/test.png 則會自動在S3新增test資料夾

## 開啟CORS

到下圖中設定

![](https://github.com/easonwang01/web_advance/tree/1925ddcb36447378ab5377e38c84f5ccccca8136/assets/螢幕快照%202018-04-18%20上午11.14.50.png)

加上藍色那段

```text
<AllowedMethod>HEAD</AllowedMethod>
```

完整版為：

```text
<!-- Sample policy -->
<CORSConfiguration>
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>HEAD</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>Authorization</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

