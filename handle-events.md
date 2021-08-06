# Handle Events

![](.gitbook/assets/e241f2cc.jpg)

Ngắn gọn thì đây là các thao tác mà người dùng gửi đến cho trang web, thường là với các UI dạng button hay form hay select. Các thao tác này có thể là thay đổi theme trang web \(dark mode\), hay get/post dữ liệu. Ta có thể gọi nó là các sự kiện thay đổi trạng thái của component. 

## Button

Đây là element UI, thường được dùng để bắt sự kiện. Thẻ &lt;button&gt; mặc định có thuộc tính là `onClick`, trong React ta sẽ sử dụng onClick để gọi các hàm sự kiện. Ví dụ:

{% tabs %}
{% tab title="Class Component" %}
```javascript
import React from "react";

class App extends React.Component {
  message() {
    alert("Hello World!");
  }
  render() {
    return (
      <button onClick={this.message}>Click Me</button>
    );
  }
}
```
{% endtab %}

{% tab title="Function Component" %}
```javascript
import React from "react";

function App {
  message() {
    alert("Hello World!");
  }
  return (
    <button onClick={this.message}>Click Me</button>
  );
}
```
{% endtab %}
{% endtabs %}

Ta cũng có thể truyền dữ liệu vào onClick thông qua `Arrow Function` hoặc `this.bind` : 

{% tabs %}
{% tab title="Use Arrow Function" %}
```javascript
import React from 'react';

class App extends React.Component {
  message = (a) => {
    alert(a);
  }
  render() {
    return (
      <button onClick={() => this.message("Wow")}>Click me</button>
    );
  }
}
```
{% endtab %}

{% tab title="Use this.bind" %}
```javascript
import React from 'react';

class App extends React.Component {
  message = (a) => {
    alert(a);
  }
  render() {
    return (
      <button onClick={this.message.bind(this, "Wow")}>Click me</button>
    );
  }
}
```
{% endtab %}
{% endtabs %}

## Forms

Việc handle với form, ta cần lưu ý form không phải một element độc nhất như button, mà một form thường đi kèm với input, submit hay select. Thế nên thao tác với form cần thao tác với tất cả các thành phần trong nó. 

### 1. input:

Đây là thành phần thường xuất hiện trong form, nó có thể nhận giá trị text, number, email hay password. Để có thể làm việc với input, ta cần dùng `onChange`

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }
  myChangeHandler = (event) => {
    this.setState({username: event.target.value});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}
```

 Ví dụ ở trên dùng `onChange` để lấy dữ liệu được nhập vào input. Ta cũng có thể lấy nhiều dữ liệu từ nhiều input :

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
    };
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      </form>
    );
  }
}
```

Ta cũng có thể truyền dữ liệu vào input như sau :

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      description: 'The content of a input goes in the value attribute'
    };
  }
  render() {
    return (
      <form>
      <input value={this.state.description} />
      </form>
    );
  }
}
```

### 2. select :

Về select thì được sử dụng trong React không khác mấy với HTML 

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      mycar: 'Volvo'
    };
  }
  render() {
    return (
      <form>
      <select value={this.state.mycar}>
        <option value="Ford">Ford</option>
        <option value="Volvo">Volvo</option>
        <option value="Fiat">Fiat</option>
      </select>
      </form>
    );
  }
}
```

### 3. submit:

Đơn giản là một button \(có thể thay thế bởi input\) nhưng vì dùng trong form nên ta không cần onClick chỉ cần viết đơn giản thế này : 

```markup
<button type="submit">Submit</button>
```

Nhưng phần form cần có `onSubmit` như sau :

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: '' };
  }
  mySubmitHandler = (event) => {
    event.preventDefault();
    alert("You are submitting " + this.state.username);
  }
  myChangeHandler = (event) => {
    this.setState({username: event.target.value});
  }
  render() {
    return (
      <form onSubmit={this.mySubmitHandler}>
        <h1>Hello {this.state.username}</h1>
        <p>Enter your name, and submit:</p>
        <input
          type='text'
          onChange={this.myChangeHandler}
        />
        <button type="submit">Submit</button>
      </form>
    );
  }
}
```

### 4. Validation 

Trong trường hợp dữ liệu nhập vào không phù hợp với mong muốn của ta, ta có thể điều chỉnh thông báo lỗi đến với người dùng :

```javascript
class MyForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: '',
      age: null,
      errormessage: ''
    };
  }
  myChangeHandler = (event) => {
    let nam = event.target.name;
    let val = event.target.value;
    let err = '';
    if (nam === "age") {
      if (val !="" && !Number(val)) {
        err = <strong>Your age must be a number</strong>;
      }
    }
    this.setState({errormessage: err});
    this.setState({[nam]: val});
  }
  render() {
    return (
      <form>
      <h1>Hello {this.state.username} {this.state.age}</h1>
      <p>Enter your name:</p>
      <input
        type='text'
        name='username'
        onChange={this.myChangeHandler}
      />
      <p>Enter your age:</p>
      <input
        type='text'
        name='age'
        onChange={this.myChangeHandler}
      />
      {this.state.errormessage}
      </form>
    );
  }
}
```

