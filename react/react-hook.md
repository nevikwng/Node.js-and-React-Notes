# React hook



## React hook 教學

可以讓functional component也擁有 ref, state, 生命週期等方法。

### State

```javascript
import React, { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

> tip: 多個boolean狀態管理在同個object

```javascript
const [studyListOpen, setStudyListOpen] = React.useState({});
setStudyListOpen({ ...studyListOpen, [id]: !studyListOpen[id] || false });
```

### ComponentDidMount

```javascript
import React, { useEffect } from 'react';

function Example() {

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    console.log('Mounted')
  }, /** 記得傳此參數，否則會重複render/);

  return (
    <div>
      <p>You clicked {count} times</p>
        Click me
      </button>
    </div>
  );
}
```

> 用 useEffect 有個雷要注意，記得傳第二個參數，否則會發現怎麼一直重複render
>
> [https://reactjs.org/docs/hooks-reference.html\#conditionally-firing-an-effect](https://reactjs.org/docs/hooks-reference.html#conditionally-firing-an-effect)

### ComponentWillMount

[https://reactjs.org/docs/hooks-reference.html\#uselayouteffect](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)

```text
useLayoutEffect(()=> {
  console.log('I am about to render!');
},[]);
```

### Ref

```javascript
import React, { useState, useEffect, useRef } from 'react';

const Map = () => {
  const mapEle = useRef(null);
  return (
    <div ref={mapEle}>
  </div>
  )
};

export default Map;
```

> 之後用`mapEle.current` 即可存取

**Memo**

memo只能用在stateless component

功能類似pure component

```javascript
import React,{ memo } from 'react';

 const Test = memo(function Son(props){
    return (
        <div>{props.val}</div>
    )
})
export default Test;
```



### forwardRef, useImperativeHandle

用來將子元件的方法傳給父元件

```javascript
const { forwardRef, useRef, useImperativeHandle } = React;

// We need to wrap component in `forwardRef` in order to gain
// access to the ref object that is assigned using the `ref` prop.
// This ref is passed as the second parameter to the function component.
const Child = forwardRef((props, ref) => {

  // The component instance will be extended
  // with whatever you return from the callback passed
  // as the second argument
  useImperativeHandle(ref, () => ({

    getAlert() {
      alert("getAlert from Child");
    }

  }));

  return <h1>Hi</h1>;
});

const Parent = () => {
  // In order to gain access to the child component instance,
  // you need to assign it to a `ref`, so we call `useRef()` to get one
  const childRef = useRef();

  return (
    <div>
      <Child ref={childRef} />
      <button onClick={() => childRef.current.getAlert()}>Click</button>
    </div>
  );
};
```

