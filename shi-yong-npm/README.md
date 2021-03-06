# 使用NPM



## 使用NPM

* 先安裝Node.js
* 以下為基本指令

npm install

npm install -g \(使用後可在cmd的任何路徑輸入package名稱執行，但如果是想在js檔內直接使用require的話，要再把環境變數加上才行\)\(如此即可不用在每個專案資料夾個別安裝package\)\)  
\(記得名稱要是NODE\_PATH\)

**所以共有兩個環境變數:**

1. 一個是NODE\_PATH =&gt; 安裝全域包後給js檔案require用

> NODE\_PATH `C:\Program Files\nodejs\node_modules`
>
> Mac 使用 require global package 可用  
> export NODE\_PATH=/usr/local/lib/node\_modules
>
> 如果不知道路徑是什麼可以先試著安裝-g 模組 然後看一下他print出來的安裝路徑
>
> ![](/assets/a.png)

2.另一個是`C:\Users\Jason\AppData\Roaming\npm`給在cmd直接輸入module名稱用

> PATH `C:\Users\Jason\AppData\Roaming\npm`
>
> 預設 安裝Node.js會自動幫你加上

```text

```

npm install --save

npm uninstall

npm search

npm ls -g

npm ls -gl

npm ls -l

npm update -g

npm update

### 為了避免部屬後環境module過大，可不必安裝dev用的module

一開始開發時將套件安裝到devDependencies

```text
npm install --save-dev  //記得save跟dev要用-連再一起
```

部屬時安裝

```text
npm install --production
```

> 當npm install出現一些版本錯誤，而無法安裝，這是記得先更新本地端`npm install -g`\(更新global的package\)  
> 更多可參考  
> [https://docs.npmjs.com/](https://docs.npmjs.com/)

## package.json教學

1. `"main"`表示require\('模組名稱'\)所預設加載的文件。

2.如下的寫法可用`npm run start`輸入此即會執行`node index.js`

```text
"scripts": {
  "start": "node index.js"
},
```

1. config用來設定環境變量，如下

```text
"config": { "port" : "8080" }
```

可在程式中使用  
`process.env.npm_package_config_port`讀取到

比較常用設定環境變量的方法為

```text
console.log(process.env.PORT)
```

然後執行

```text
PORT=8000 node test1.js //mac
```

> windows如果不行可以使用git bash來執行

nodemon可用類似如下

```text
set PORT=8080 && nodemon test.js
```

\# 控制版本

使用shrinkwrap

[http://syshen.cc/post/18425250521/npm-shrinkwrap-解決-nodejs-套件複雜的關連性問題](http://syshen.cc/post/18425250521/npm-shrinkwrap-解決-nodejs-套件複雜的關連性問題)

## 更新npm

```text
npm install npm@latest -g
```

## 發佈npm package

```text
npm adduser
npm publish
```

> 記得輸入npm publish時要在package.json的同層目錄下
>
> 每次更新記得要更改package.json的版本號

## 發佈為npm global package

> 可以直接在Terminal輸入指令的package

和之前一樣先npm adduser

然後確認package名字沒重複

1.記得將index.js檔案加上

```javascript
#!/usr/bin/env node

.....
```

2.之後因為是global所以加入bin的欄位到package.json檔案裏面

```text
  "bin": {
    "gendoc": "./index.js"
  },
```

之後別人使用-g安裝即可使用`gendoc` 指令

> npm link 也可在發布前直接把他加到Local環境變數進行測試，但在windows會沒作用

## 更新或復原npm版本

\(把數字改為你要的版本即可\)

```text
npm install npm@4.6.1
```

## 切換版本

**n**

```text
n use <version>
```

**nvm**

\(windows可能無法使用n,所以可以用nvm安裝檔\)

下載點:[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)

然後

```text
nvm install <version>
nvm use <version>
```

## 查看套件以前發布過的版本

```text
npm view <套件名稱>
```

## 常見問題

**1.node-gyp error**

有時如果安裝的套件是使用C++然後用node-gyp編譯的話，需要預先安裝如下

```text
npm install -g node-gyp
```

> 如果是ubuntu還需要安裝build-essential
>
> ```text
> sudo apt-get install build-essential
> ```

