可參考http://www.epooll.com/archives/768/

先增加file operator

```
sudo sh -c "ulimit -n 65535 && exec su $LOGNAME"
```


#1.使用Webbench

https://github.com/EZLippi/WebBench
```
wget http://blog.zyan.cc/soft/linux/webbench/webbench-1.5.tar.gz
tar zxvf webbench-1.5.tar.gz
cd webbench-1.5
sudo make &&sudo make install
```
使用
```
webbench -c 5000 -t 120 <url>
```

#2.使用Apache Benchmark(ab)
https://httpd.apache.org/download.cgi

```
 sudo apt-get install apache2-utils
```

```
ab -k -n 50000 -c 9000 -r <url>
```

>打https會ssl handshake failed

如果併發數過高出現error可參考以下調整(輸入-r)

https://superuser.com/questions/323840/apache-bench-test-error-on-os-x-apr-socket-recv-connection-reset-by-peer-54

如果用ip當url錯誤，在後面加`/`即可

```
ab -c 19000 -n 220000 -r 114.28.22.129/

```

#3.使用hey(written in Golang)

優點:可直接打https

先安裝golang https://www.digitalocean.com/community/tutorials/how-to-install-go-1-6-on-ubuntu-16-04

https://github.com/rakyll/hey

之後hey會在go資料夾裡面的bin內