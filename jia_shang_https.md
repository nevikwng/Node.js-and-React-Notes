# 加上https

先到此網站

https://www.sslforfree.com

輸入你的網域名稱

之後把它給你的兩個檔案下載，之後放到你的server www目錄下的它給你的路徑

ubuntu路徑如下

`/usr/share/nginx/`

之後點擊下面連結測試，如果有下載下來即為成功

之後就可以點下面綠色按鈕，往下一步

###接著是重點

它會給你三個框框，裡面分別為
```
ca_bundle.csr 
private.key 
certificate.csr
```
用nginx為例子

我個人把它們存放再`/usr/share/nginx/sslcrt`路徑都可以，只要後面的virtual host 檔案內設定相同即可

另外要輸入以下指令，將兩個crt合成一個bundle
```
sudo bash -c 'cat certificate.crt ca_bundle.crt >> bundle.crt'
```

我們要在nginx的virtual host設定檔案內設定https


ubuntu路徑如下

`/etc/nginx/sites-available`

會有一個default檔案，用vim等文字編輯器開啟


開啟後如前面沒設定過，會都是藍色的註解

把它更改如下圖(如果說private.key not match ，把ssl_certificate 的bundle.crt改為certificate.crt試試)

![df](https://cloud.githubusercontent.com/assets/11001914/17371582/f7e9e9de-59d2-11e6-9929-c00f005eebcb.png)

之後

```
1.sudo service nginx stop

2.sudo nginx
```
在你的網域前加上`https://`即可


參考至:https://free.com.tw/ssl-for-free/