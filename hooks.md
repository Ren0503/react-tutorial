# Hooks

![](.gitbook/assets/1_xdkwrklnelaupucqq6m0tw.png)

Như đã nói ở bài viết component, các function component là stateless tức là không có state và cũng không có lifecycle. Chỉ có thể hiển thị giao diện tĩnh. Nhưng từ phiên bản 16+, React đã hỗ trợ các hooks để có thể quản lý state và lifecycle ngay trong function component. Cụ thể ta có :

* Basic :
  * useState
  * useEffect
  * useContext
* Advanced:
  * useMemo
  * useCallback
  * useReducer

### Basic

#### useState\(\)

Hàm này nhận đầu vào là giá trị khởi tạo của 1 state và trả ra 1 mảng gồm có 2 phần tử, phần tử đầu tiên là state hiện tại, phần tử thứ 2 là 1 function dùng để update state \(giống như hàm setState cũ vậy\). Nó thay thế phương thức `constructor` ở class component.

{% tabs %}
{% tab title="Class Component" %}
```javascript
constructor(props) {
    super(props);
    this.state = { count: 0 }
 }

 const setCount = (number) => {
     this.setState({
         count: number,
     });
 }
```
{% endtab %}

{% tab title="Function Component" %}
```javascript
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(0)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```
{% endtab %}
{% endtabs %}

 Khi muốn update state cho `count` là `1` thì chỉ cần gọi đến hàm `setCount(number)`.  
Nếu như đang làm việc với React-Redux để quản lý State thì chỉ nên sử dụng useState để quản lý các UI State \(là những state có giá trị boolean nhằm mục đích render ra UI\) để tránh việc conflict với cả Redux State và maintain sau này.

#### useEffect\(\)

Điểm đặc biệt của useEffect\(\) là nó không phải thay thế một giai đoạn mà là cả 3 giai đoạn từ mount, updating đến unmount. useEffect tương tự 3 hàm là  **componentDidMount**, **componentDidUpdate** và **componentWillUnMount** trong LifeCycle khi khai báo `arrayDependencies` là `[]`

Khi chúng ta muốn dùng `componentDidUpdate` trong `function component` chúng ta sẽ dùng như sau: `useEffect(function)`

```javascript
() => {
  const [count, setCount] = useState(0);
  const [something, setSomething] = useState(false);
  const handleClick = () => setCount(count + 1);
 
  useEffect(() => {
    console.log('Watch Here');
    document.title = 'Count is: ' + count;
  })

  return <div>
    <p>Title page will be change when we click the button</p>
    <button onClick={handleClick}>Increment Count</button>
    <button onClick={() => setSomething(!something)}>Change something</button>
  </div>
}
```

 **NOTE**:  
Như trong ví dụ trên thì thấy hàm `useEffect`  luôn được gọi mỗi khi component được render xong. Vậy `arrayDependencies` thì sao? Nếu ta không truyền bất kì biến nào vào `arrayDependencies` thì bất kể state nào thay đổi cũng bị React chạy lại Hook.

Ở đây nếu đối số thứ 2 là **arrayDependencies** khi thay đổi thì đối số thứ nhất mới thực thi `(function)` vậy nếu **arrayDependencies** **không thay đổi** tức ta gán bằng `[]` thì điều này có nghĩa nó **tương tự** với `componentDidMount` vì khi này function trong `useEffect` chỉ gọi 1 lần.

  
Vậy nếu là `componentWillUnMount` thì  `useEffect` sẽ thay thế đó như thế nào. Như mọi người đã biết thì `componentWillUnMount` chạy mỗi khi một component chuẩn bị remove khỏi tree dom. Do đó ta chỉ cần return 1 function.

`useEffect` cho phép chúng ta return 1 function, function này sẽ thực thi trước khi mà component đó được unmounted.

```javascript
const MouseTracker = () => {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });
  const onMouseMove = ({ clientX, clientY }) => {
    setMousePosition({ x: clientX, y: clientY });
  }
  useEffect(() => {
    document.addEventListener("mousemove", onMouseMove);
    return () => {
      document.removeEventListener("mousemove", onMouseMove);
    };
  }, []);

  return (
    <pre
      style={{
        position: "absolute",
        width: 100,
        height: 100,
        borderRadius: "50%",
        background: "skyblue",
        top: mousePosition.y + 30,
        left: mousePosition.x + 30
      }}
    >
      {JSON.stringify(mousePosition, null, 2)}
    </pre>
  );
};
```

Nhìn component trên ta sẽ hiểu. Sau khi load hết DOM thì React sẽ add 1 event di chuyển chuột. Event này sẽ gọi tới 1 function là set position. Khi component này bị remove khỏi DOM thì sẽ remove event di chuyển chuột đi.  
Do em set `arrayDependencies` là `[]`. Nên effect `document.addEventListener("mousemove", onMouseMove);` chỉ chạy 1 lần.

**Tóm lại**: nếu set 1 biến state trong `arrayDependencies` thì React sẽ chạy lại Hook này khi biến state đó thay đổi. Nếu chỉ muốn chạy 1 lần thì set `[]`

#### useContext\(\)

Ở bài viết trước ta đã thấy cách sử dụng Context API với Class Component. Bây giờ ta sẽ xem cách nó làm với Function Component. Về phần khởi tạo ta vẫn sử dụng `React.createContext()` và `Provider`. Nhưng thay vì sử dụng `contextType` và `Consumer` ta sử dụng **useContext\(\)**. 

```javascript
import React, { createContext, useContext } from 'react';

const AppContext = createContext('Initial Value');

export default function App() {
    const initData = {
        firstValue: 'first value',
        secondValue: 'second value',
    };
    
    return (
        <AppContext.Provider value={initData}>
            <FirstChild />
            <SecondChild />
        </AppContext.Provider>
    );
};

function FirstChild () {
    const valueFromContext = useContext(AppContext);
    return <h1>{valueFromContext.firstValue}</h1>
}

function SecondChild() {
    const valueFromContext = useContext(AppContext);
    return <h1>{valueFromContext.secondValue}</h1>
}
```

### Advanced

#### useMemo\(\)

useMemo giúp ta kiểm soát việc được render dư thừa của các component con, nó khá giống với hàm **shouldComponentUpdate** trong lifecycle. Bằng cách truyền vào 1 tham số thứ 2 thì chỉ khi tham số này thay đổi thì **useMemo** mới được thực thi. Ví dụ:

```javascript
import React, { useState, useMemo } from "react";

const App = () => {
  const [text, setText] = useState("Hello!");

  const ChildComponent = ({ text }) => {
    console.log("rendered again!");
    return <div>{text}</div>;
  };

  const MemoizedComponent = useMemo(() => <ChildComponent text={text} />, [
    text
  ]);

  return (
    <div>
      <button onClick={() => setText("Hello!")}>Hello! </button>
      <button onClick={() => setText("Hola!")}>Hola!</button>
      {MemoizedComponent}
    </div>
  );
};
```

Ở ví dụ trên ta sẽ thấy khi **on load** dưới console sẽ chạy `rendered again`. Khi ta click button **Hello** console không chạy lại `rendered again`. Nhưng khi ta click **Hola** thì console log lại render  `rendered again`.

Nếu **không** **sử dụng** **useMemo** thì khi click 1 trong 2 button cũng đều render lại `ChilComponent`.

**Có** **sử dụng** **useMemo** thì `ChildComponent` sẽ được render lần đầu tiên và cache lại. `ChilComponent` sẽ render lại khi biến text thay đổi.

#### useCallback\(\)

useCallback trả về một function và một array chứa các dependencies \(những biến số được truyền vào từ bên ngoài mà function này phụ thuộc khi chạy\).

useCallback sử dụng cơ chế memorization – ghi nhớ kết quả của một function vào trong memory và sẽ trả về function được ghi nhớ trong trường hợp các dependencies này không thay đổi.

**useCallback** có nhiệm vụ tương tự như **useMemo** nhưng khác ở chỗ function truyền vào useMemo **bắt buộc phải ở trong quá trình render**. Ví dụ :

```javascript
import React, { useState, useEffect, useCallback } from "react";

const ChildComponent = ({ loggingStatus }) => {
  useEffect(() => {
    loggingStatus();
  }, [loggingStatus]);
  return <div />;
};

const UseMemoComponent = () => {
  const [count, setCount] = useState(0);
  const loggingStatus = useCallback(() => {
    console.log("Run from ChildComponent");
  }, []);
  const addMore = () => {
    setCount((prev) => prev + 1);
  };

  return (
    <div>
      <p>Current: {count}</p>
      <ChildComponent loggingStatus={loggingStatus} />
      <button onClick={addMore}>Click</button>
    </div>
  );
};
```

Để ý ví dụ ở trên, chúng ta có `ParentComponent` và `ChildComponent`.

Nếu chúng ta không xài **useCallback** mà chỉ xài function chỗ **loggingStatus:**

```javascript
const loggingStatus = () => {
    console.log("Run from ChildComponent");
 };
```

Khi nhấn vào button ở `ParentComponent`, component này sẽ bị render lại do có sự thay đổi về `state`. Kéo theo đó, `ChildComponent` cũng bị render lại theo cho dù không có sự thay đổi gì \(props truyền vào y nguyên\).

**useCallback** được sinh ra để giải quyết vấn đề này. Nó sẽ ghi nhớ lại function được truyền vào và danh sách các dependencies. Lúc này, khi nhấn vào button, React sẽ render lại `ParentComponent` và so sánh xem props của `ChildComponent` có thay đổi không để render tiếp. Vì `loggingStatus` hoàn toàn không phụ thuộc vào dependency nào, nên giá trị của nó không thay đổi và `ChildComponent` sẽ không bị render lại.

#### useReducer\(\)

Thực tế khi sử dụng useState thì nó sẽ trả về 1 phiên bản đơn giản của useReducer, vậy nên chúng ta có thể coi useReducer như một phiên bản nâng cao hơn dùng để thay thế cho việc sử dụng useState. Nếu đã làm việc với React-Redux thì chắc hẳn sẽ dễ dàng nhận ra flow quen thuộc này. Giống như reducer trong Redux thì useReducer cũng nhận vào một reducer dạng \(state, action\) và trả ra một newState. Khi sử dụng chúng ta sẽ nhận được một cặp bao gồm current state và dispatch function. Ví dụ:

```javascript
const initialState = {count: 0}

function Reducer(state, action) {
  const [count, setCount] = useState(0);
  switch (action.type) {
    case 'INCREMENT':
      return setCount( count + 1);
    case 'DECREMENT':
      return setCount( count - 1);
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState)
  return (
    <>
      <StyledLogo src={logo} count={count}/>
      <Count count={count} />
      <div style={{display: 'flex'}}>
        <button onClick={() => dispatch({type: 'DECREMENT'})}> - </button>
        <button onClick={() => dispatch({type: 'INCREMENT'})}> + </button>
      </div>
    </>
  )
}
```

Ngoài những hooks cơ bản hay được sử dụng ngoại trừ **useContext** ở trên thì vẫn còn 1 số hook khác như là **useRef**, **useLayoutEffect**, **useDebugValue**, **useImperativeHandle** mọi người có thể vào trang chủ của [react hooks](https://reactjs.org/docs/hooks-reference.html) để tìm hiểu thêm nhé.

