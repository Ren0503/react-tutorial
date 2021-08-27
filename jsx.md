# JSX

![](.gitbook/assets/1_i_vj3gs7qo9fjcxelk2c2g.png)

JSX là một template engine mà React sử dụng để tạo UI cho trang web. Nó là sự kết hợp giữa JavaScript và XML. Giống như một HTML được lồng vào trong JavaScript. Nhưng vì trình duyệt không đọc được JSX, nên khi chạy nó được babel dịch từ JSX sang JavaScript. Ví dụ một đoạn code trong JavaScript : 

```javascript
React.creactElement("div",{ className: "body" },"Welcome to JSX")
```

Thì nó sẽ được dịch sang JSX như sau:

```jsx
<div className="circle">Welcom to Devil</div>
```

Có thể thấy JSX có cú pháp mở dấu &lt;&gt;&lt;/&gt; hay còn gọi là element tương tự với XML. Bao gồm hai phần :

* tag : là phần đánh dấu định dạng giống như HTML, có thể là p, div hay i. Trong trường hợp là input hay image. Thẻ tag sẽ kết thúc với dấu / mà không cần một dạng cặp thẻ, như &lt;input /&gt;, &lt;image /&gt;.
* data : là nội dung mà ta cần hiển thị, ví dụ như một đoạn văn bản, tiêu đề,...

Điều cần lưu ý là khi sử dụng JSX, ta phải để tất cả mọi thứ trong element root. Ví dụ :

```jsx
// Cú pháp đúng
const element = <div className="jsx">Welcom to Devil</div>
// Cú pháp sai
const element = <div className="jsx">Welcom to</div><div> Devil</div>
```

Ta có thể để các element con nằm trong element lớn, nhưng không được đè lên nhau.

```jsx
<div className="jsx">
    <h1>JSX</h1>
    <p>This is true syntax!!!</p>
</div>

// ------------------------------

<div className="jsx">
    <h1>JSX
    <p>
        This is false syntax!!!
    </h1>
    </p>
</div>
```

Bên cạnh hiển thị các dữ liệu như HTML, JSX còn có thể hiển thị dữ liệu từ giá trị truyền vào :

```jsx
import React from 'react';

const data = {
    value: "React",
    number: 1
};

export default class App extends React.Component {
    render() {
        return (<div>Hello {data.value}</div>);
    }
}
```

Đồng thời cũng có thể thực hiện các thao tác rẽ nhánh :

```jsx
import React from 'react';

export default class App extends React.Component {
    render() {
        var i = 1;
        return (<div>{i == 1 ? 'True!' : 'False'}</div>);
    }
}
```

Hay thực hiện toán tử && :

```jsx
import React from 'react';

export default class App extends React.Component {
    render() {
        var i = 1;
        return (<div>{i && 'Appear'}</div>);
    }
}
```

Bên cạnh đó tag của các element cũng có các thuộc tính tương tự như trong HTML như href, src, onClick,... Điểm khác chính là class trong HTML sẽ là className bên JSX. Thực ra khi rendered className sẽ tự động được chuyển thành class. Nếu viết class sẽ sinh lỗi khi render.

Như vậy JSX chỉ đơn giản là một dạng template mà React sử dụng để hiện thị giao diện UI thay vì dùng HTML hay JavaScript truyền thống.

