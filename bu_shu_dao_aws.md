# 部署到AWS

```
記得開security group 來開port
```

https://www.youtube.com/watch?v=WxhFq64FQzA

##上傳檔案

如果沒辦法用SFTP出現permission deny須先把該EC2上的資料夾使用`sudo chmod 777 資料夾名稱 `來開啟權限即可

GUI部分，在`window`可用WINSCP或FileZillz在`Mac`可用CyberDuck

http://stackoverflow.com/questions/20939562/scp-permission-denied-publickey-on-ec2-only-when-using-r-flag-on-directories