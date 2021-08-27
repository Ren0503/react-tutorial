# Props & State

![](.gitbook/assets/react-props-blog-image-design-2.jpg)

Các component trả về một JSX, nên bằng tư duy bắc cầu chúng cũng có thể lồng nhau như JSX. Một component có thể bao gồm các component con khác nằm bên trong chúng.

## Props

Props là viết tắt của properties nghĩa là thuộc tính, tức là những thứ mà nó nhận được từ component cha của chúng, hiểu nôm na là dữ liệu di truyền. Ví dụ như sau :

```javascript
import React from 'react';

function App() {
    return (
    <div classname="App">
        <header classname="App-header">
            <p>Welcome to Tech diagonal tutorials</p>
            <FunctionalComponent name={"Welcome"}/>
            <FunctionalComponentArrow course={"React"}/>
            <ClassComponentExample message={"Component"}/>
        </header>
    </div>
    );
}

function FunctionalComponent(props){
    return <h1>Func Component Example {props.name}</h1>
}

const FunctionalComponentArrow = (props) => {
    return <h1>Func Component Arrow Example {props.course}</h1>
}

class ClassComponentExample extends React.Component {
    render() {
        return <h1>Class Component {this.props.message}</h1>;
    }
}

export default App;
```

Ví dụ trên truyền props theo ba cách sử dụng component, ta có thể thấy các function nhận props thông qua tham số, còn class nhận thông qua con trỏ this. Các component con trong ví dụ hiển thị một đoạn văn bản \(định dạng h1\) với phần cuối chưa xác định nó tuỳ thuộc vào dữ liệu mà nó nhận vào từ component cha. Đều phải lưu ý là phần khai báo dữ liệu truyền vào ở component cha và sử dụng dữ liệu ở component con phải đồng nhất. Tức khi ta viết :

```javascript
...
<FunctionalComponent name={"Welcome"}/>
...
// It requirement
return <h1>Func Component Example {props.name}</h1>
// It is wrong if
return <h1>Func Component Example {props.title}</h1>
...
```

Bên cạnh đó để phòng trừ trường hợp ta quên truyền dữ liệu từ phía cha, ta nên thiết lập props mặc định ở component con bằng defaultProps:

```jsx
ClassComponentExample.defaultProps = {
    message:"Default Msg"
}
```

## State

Không phải viết tắt gì cả, state được sử dụng đúng với định nghĩa của nó là trạng thái của các component. State biểu diễn trạng thái hiện tại của component, tức là nó hiển thị các dữ liệu nội tại của component đó. Ví dụ dưới đây để có thể dễ mường tượng.

```javascript
class ClassComponentExample extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            message:"Default Message",
        }
    }
    btnClick = () =>{ this.setState({ message:"Button Clicked"})}
        
    render() {
        return <div>
            <h1>{this.props.message}</h1>
            <button onclick="{this.btnClick}">Button</button>
        </div>
    }
}
```

Như trong ví dụ ban đầu trạng thái của đoạn h1 sẽ là "Default Message", tuy nhiên sau khi ta click vào nút button nó sẽ thay đổi thành "Button Clicked". Cứ mỗi khi state thay đổi thì component đó sẽ được re-render lại. Quá trình tạo trạng thái trong Class Component như sau.

### 1. Constructor\(\):

Đây là hàm khởi tạo trong Class Component, cú pháp khởi tạo như sau :

```javascript
constructor(props){
    super(props);
    //… Other Statements
}
```

Ở đây ta dùng super\(props\) về cơ bản thì câu lệnh này mang ý nghĩa truyền props vào constructor ở component cha. Nếu thắc mắc tại sao phải làm vậy có thể đọc bài viết [này](https://overreacted.io/vi/why-do-we-write-super-props/).

### 2. Initialize State:

Giai đoạn này ta thiết lập các giá trị ban đầu cho trạng thái, nó được viết ngay trong constructor :

```javascript
this.state = {
    name: value,
    ...
}
```

### 3. Access State:

Khi tạo xong rồi, ta có thể sử dụng state ở trong render\(\). Ví dụ như một đoạn văn bản nhận dữ liệu từ state name.

```javascript
render() {
    return <h1>{this.state.name}</h1>
}
```

### 4. Updating State:

Trong quá trình sử dụng các state sẽ thay đổi, ta không thể cập nhật trạng thái cho state ở trong render hay constructor, mà phải thay đổi bằng một hàm độc lập.

```jsx
btnClick = () =>{ 
    this.setState({ name: "Button Clicked" })
}
```

Trong ví dụ trên setState là cú pháp để thay đổi giá trị cho state. Lý do mà ta không thể thay đổi trực tiếp qua this.state là vì nó sẽ không được re-render lại tại component.

