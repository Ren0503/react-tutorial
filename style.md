# Style

![](.gitbook/assets/1_ydskboekfddmti6lmjvklw.png)

Có 4 cách chính để nhúng css vào react bao gồm :

* Dùng css stylesheet
* Inline styling
* CSS modules
* Styled-components

## CSS Stylesheet

Phương pháp đầu tiên vô cùng cơ bản là chỉ việc tạo file css rồi import nó vào file js thôi :

{% tabs %}
{% tab title="DottedBox.css" %}
```css
.DottedBox {
  margin: 40px;
  border: 5px dotted pink;
}

.DottedBox_content {
  font-size: 15px;
  text-align: center;
}
```
{% endtab %}

{% tab title="DottedBox.js" %}
```javascript
import React from 'react';
import './DottedBox.css';

const DottedBox = () => (
  <div className="DottedBox">
    <p className="DottedBox_content">Get started with CSS styling</p>
  </div>
);

export default DottedBox;
```
{% endtab %}
{% endtabs %}

Cần lưu ý là khi chạy webpack nó sẽ gộp tất cả các file css lại với nhau. Nên khi đặt tên cho các element không được trùng nếu không sẽ sinh lỗi UI.

## Inline Styling

Cách này thay vì viết css ở một file riêng, ta sẽ viết nó ngay tại file js. 

```javascript
import React from 'react';

const divStyle = {
  margin: '40px',
  border: '5px solid pink'
};

const pStyle = {
  fontSize: '15px',
  textAlign: 'center'
};

const Box = () => (
<div style={divStyle}>
  <p style={pStyle}>Get started with inline style</p>
</div>
);

export default Box;
```

Ta có thể tạo biến để lưu những style object này và truyền vào element bằng **style={tenBien}** hoặc ta có thể style trực tiếp element ví dụ **style={{color: 'pink'}}**.

## CSS Modules

Với việc dùng CSS Module tên class và animation được giới hạn để chỉ apply cho riêng một component.

{% tabs %}
{% tab title="DashedBox.css" %}
```css
:local(.container) {
  margin: 40px;
  border: 5px dashed pink;
}

:local(.content) {
  font-size: 15px;
  text-align: center;
}
```

 **:local\(.tenClass\)** khi dùng với **create-react-app** và **.tenClass** khi không dùng với **create-react-app**.
{% endtab %}

{% tab title="DashedBox.js" %}
```javascript
import React from 'react';
import styles from './DashedBox.css';

const DashedBox = () => (
  <div className={styles.container}>
    <p className={styles.content}>Get started with CSS Modules style</p>
 </div>
);

export default DashedBox;
```

Ta import file CSS vào component và access tên class trong file CSS giống như ta access một object vậy. Tên của class trong file CSS sẽ tự động trở thành tên class của element mà nó style.
{% endtab %}
{% endtabs %}

## Styled-components

Styled-components là một thư viện dành cho React và React Native giúp cho việc viết style CSS ngay trong một component. Đầu tiên ta cần tải về trước

```jsx
npm install styled-components
```

Sau đó ta import package và tạo những wrapper dùng để style element bằng **style.&lt;tên Element&gt;\`style cua element\`** và sử dụng chúng để render

```jsx
import React from "react"
import styled from "styled-components"

const Input = styled.input`
 background: green
`

const FormWrapper = () => <Input placeholder="hola" />

export default FormWrapper
```

