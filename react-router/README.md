# React router



## React router

延續上一章

先`npm install react-router`

### 1.接著將我們的client/client.js 改為如下

```text
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
    <Route path="/" component={App}/>
     <Route path="/Proptest" component={Proptest}/>
     <Route path="/TextDisplay" component={TextDisplay}/>
    </Router> 
  ),document.getElementById('app'))
```

即可看到現在可以再url輸入[http://localhost:3000/\#/about](http://localhost:3000/#/about)

進入另一個元件

### 2.Link

接著到App.js加上

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
     <ul role="nav">
          <li><Link to="/TextDisplay">TextDisplay</Link></li>
          <li><Link to="/Proptest">Proptest</Link></li>
        </ul>
    <TextDisplay/>
    </div>
  )}

}
export default App
```

即可看到點選li跳至不同元件，及更改了url

### 3.我們現在想讓App.js變成一個nav然後點選後App.js不動，在他的下面render不同的component，所以我們要把route改為巢狀

client.js

```text
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
        <Route path="/" component={App}>
             <Route path="/Proptest" component={Proptest}/>
             <Route path="/TextDisplay" component={TextDisplay}/>
        </Route>
    </Router> 
  ),document.getElementById('app'))
```

App.js

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
     <ul role="nav">
          <li><Link to="/TextDisplay">TextDisplay</Link></li>
          <li><Link to="/Proptest">Proptest</Link></li>
        </ul>
         {this.props.children}

    </div>
  )}

}
export default App
```

，因為今天兩個component在client.js下為App.js的子代，所以要用`{this.props.children}`去顯示  
\(更改後記得重新整理\)

### 4.

幫link 加上active時的style

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
     <ul role="nav">
          <li><Link to="/TextDisplay" activeStyle={{ color: 'orange' }}>TextDisplay</Link></li>
          <li><Link to="/Proptest" activeStyle={{ color: 'red' }}>Proptest</Link></li>
        </ul>
         {this.props.children}

    </div>
  )}

}
export default App
```

另一種方式是幫他寫上class  
`activeClassName="active"`  
然後即可加入css檔案 和正常的class一樣

### 5.像Express 使用參數url

新增一個元件`Repo.js`

```text
import React from 'react'

export default React.createClass({
  render() {
    return (
      <div>
        <h2>{this.props.params.repoName}</h2>
      </div>
    )
  }
})
```

更改Client.js

```text
import React from 'react'
import { render } from 'react-dom'
import App from '../components/App'
import Proptest from '../components/Proptest'
import TextDisplay from '../components/TextDisplay'
import Repo from '../components/Repo'
import { Router, Route, hashHistory } from 'react-router'

render(( 
    <Router history={hashHistory}>
        <Route path="/" component={App}>
        <Route path="/repo/:userName/:repoName" component={Repo}/>
             <Route path="/Proptest" component={Proptest}/>
             <Route path="/TextDisplay" component={TextDisplay}/>
        </Route>
    </Router> 
  ),document.getElementById('app'))
```

之後在url輸入[http://localhost:3000/\#/repo/this/is](http://localhost:3000/#/repo/this/is)  
即可

## PS:巢狀route，當url是子代時，會把所有的父代component都render出

### 6.給route 一個預設的頁面

建立home component

Home.js

```text
import React, { Component } from 'react'


export default class extends Component {
      render() {
    return ( 
        <div>Home</div>
  )}
}
```

App.js

```text
import React, { Component } from 'react'
import TextDisplay from './TextDisplay'
import Home from './Home'
import { Link } from 'react-router'

class App extends Component {

  render() {
    return (
    <div>
     <ul role="nav">
          <li><Link to="/TextDisplay" activeStyle={{ color: 'orange' }}>TextDisplay</Link></li>
          <li><Link to="/Proptest" activeStyle={{ color: 'red' }}>Proptest</Link></li>
        </ul>
        <div>

          {this.props.children || <Home/>}
        </div>
    </div>
  )}

}
export default App
```

\(not understand now\)另一個標籤\(IndexRoute\)

[https://github.com/reactjs/react-router-tutorial/tree/master/lessons/08-index-routes](https://github.com/reactjs/react-router-tutorial/tree/master/lessons/08-index-routes)

[https://github.com/reactjs/react-router/blob/master/docs/guides/IndexRoutes.md](https://github.com/reactjs/react-router/blob/master/docs/guides/IndexRoutes.md)

## 7.消除原本在url上的`#`

將hash history改為browser history

App.js

```text
import { Router, Route, browserHistory, IndexRoute } from 'react-router'
```

```text
<Router history={browserHistory}>
```

之後點選link發現url後不再出現`#`，但點選後這時按F5，發現出現了`Cannot get`

只要把server.js

改為

```text
app.get('*', function (req, res) {
    res.sendFile(path.resolve('client/index.html'));
});
```

即可

參考:  
[https://github.com/reactjs/react-router/blob/master/docs/guides/Histories.md\#configuring-your-server](https://github.com/reactjs/react-router/blob/master/docs/guides/Histories.md#configuring-your-server)

參考至

[https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route](https://github.com/reactjs/react-router-tutorial/tree/master/lessons/02-rendering-a-route)

## React router Version 4, 5 用法

[https://github.com/ReactTraining/react-router/issues/4928](https://github.com/ReactTraining/react-router/issues/4928)

> 記得要在/的前面加上exact，不然所有路由都會顯示Lobby畫面。

```javascript
      <Router>
        <div>
          <Route exact path="/" component={Lobby} />
          <Route path="/worldcup" component={WorldCup} />
        </div>
      </Router>
```

**1.換頁可用如下**

```javascript
class aa {
 ....
}
export default withRouter(aa);
```

然後在aa裡面可用

```text
this.props.history.push("/some/Path");
```

**2.監聽路由**

```javascript
this.props.history.listen((location, action) => {
  console.log(location.pathname);
});
```

