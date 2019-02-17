
## React Passion: Mổ Xẻ React Component

 Trong lập trình với React, React Component là thành phần then chốt cấu thành nên đặc trưng riêng cho React. Tư tưởng chung, page sẽ được chia thành các React Component và các Component có thể được sử dụng lặp đi lặp lại ở nhiều page hoặc nhiều nơi trên cùng 1 page.

 Đối với tôi, việc phân chia page thành các Component đem lại sự thích thú cho người lập trình bởi đơn giản tôi là 1 fan của nhẫn thuật "chia để trị". Thật sự với tư tưởng lập trình mà React đem đến, chúng ta có thể dễ dàng phân chia công việc cho nhiều devs làm chung 1 page phức tạp, mỗi người chỉ cần lập trình component của họ với input và output rõ ràng.

 Lợi ích và thích thú là vậy, nhưng chúng ta cần hiểu rõ React Component để có thể triển khai tư tưởng đó hiệu quả. Sau đây, chúng ta cùng mổ xẻ thành phần quan trọng này của React để vận dụng như thế nào cho đúng nhé.

Ví dụ 1 React Component điển hình:
```
class ProductsList extends React.Component {
  constructor(props){...}

  componentDidMount(){...}

  shouldComponentUpdate(nextProps, nextState){...}

  static getDerivedStateFromProps(nextProps, currentState){...}

  componentDidUpdate(prevProps, prevState){...}

  render(){...}

  componentWillUnmount(){...}
}
```
#### props and state
  Các bạn đọc qua blog này của mình để hiểu hơn về 2 thành phần này [https://technotes.nustechnology.com/react_passion_props_and_state](https://technotes.nustechnology.com/react_passion_props_and_state)

#### Lifecycle
Mỗi component đều có 1 lifecycle và chúng được chia thành 2 nhóm như bên dưới. Những method xuất hiện trong update sẽ được run nhiều lần, những methods chỉ xuất hiện trong create mà không xuất hiện trong update sẽ chỉ run 1 lần duy nhất.

a. Creating Process
*Bên dưới là trình tự các methods được call khi 1 component được tạo ra và mount vào cây DOM*
```
constructor()
static getDerivedStateFromProps()
render()
componentDidMount()
```

b. Updating Process
*Bên dưới là trình tự các methods được call mỗi khi 1 component được update*
```
static getDerivedStateFromProps()
shouldComponentUpdate()
render()
componentDidUpdate()
```
Vậy,
.  `constructor` và `componentDidMount` sẽ chỉ run 1 lần duy nhất.
. `getDerivedStateFromProps`  và `render` là những method được run trong cả 2 process.

#### constructor
Đây thực sự là constructor function của class trong ES6 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

Constructor sẽ được run mỗi khi component được khởi tạo và tất nhiên chỉ được run 1 lần duy nhất. Đây là nơi để chúng ta đặt khởi tạo state cho component bằng cách set trực tiếp(không cần thông qua function setState), lúc này component vẫn chưa được mount(insert) vào cây DOM.
```
constructor(props){
  super(props) // call constructor mặc định của React.Component
  this.state = {
    products: []
  }
}
```

#### render
Đây chính là nơi sinh ra output của component, chúng ta sẽ return view của component ở đây dựa vào props và state. Mỗi khi props hoặc state thay đổi, render sẽ được thực hiện lại để phản ánh sự thay đổi đó ra view.

```
render(){
  let products = this.state.products

  if (products.length == 0)
    return <div>There are no products</div>

  return <ul>
    {products.map( prod =>
      <li>{prod.name}</li>
    )}
  </ul>
}
```

#### componentDidMount
mount = inserted into DOM

Đây là callback khi component đã được mount vào cây DOM. Và chỉ được run 1 lần duy nhất nên đây là nơi lý tưởng để fetch data tính toán lại state hoặc bind các animation lên các DOM element.

```
componentDidMount(){
  let self = this
  let params = {user_id: this.props.userId}
  axios.get('/products', params).then(response => {
    self.setState({products: response.products})
  })

  $('.sidebar').fadeOut()
}
```

#### getDerivedStateFromProps
Đây là 1 static function trong ES6 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static) , vì vậy nó ko thể access/call được instance variable hoặc instance method.

Method này sẽ run mỗi khi component nhận vào props mới và yêu cầu return state mới(nextState) cho component.

Nhìn vào lifecycle, lúc này component vẫn chưa được mount hoặc update vào cây DOM.

Dựa vào các dự kiện trên, đây là nơi lý tưởng để tính toán state mới dựa vào props mới và state cũ trước khi render ra cây DOM. Nếu chúng ta cần access các instance method hoặc instance variable của component để tính toán state mới, đây thực sự không phải nơi thích hợp vì đây là 1 static class.

```
static getDerivedStateFromProps(nextProps, currentState){
  let nextState = {
    products: nextProps.products || currentState.products
  }
  return nextState
}
```

#### componentDidUpdate
Đây là method được gọi cuối cùng trong updating process, khi component đã được mount vào cây DOM.

Đây sẽ là nơi thích hợp để chúng ta fetch data tính toán lại state mỗi khi props change. Tuy nhiên, mỗi khi tính toán lại state, component sẽ lại call render để update và callback `componentDidUpdate` sẽ được gọi lại lần nữa; lúc này, chúng ta cần đặt điều kiện dừng bên trong method này để chấm dứt vòng lặp vô hạn.

```
componentDidUpdate(prevProps, prevState){
  if (prevProps.userId == this.props.userId)
    return

  let self = this
  let params = {user_id: this.props.userId}
  axios.get('/products', params).then(response => {
    self.setState({products: response.products})
  })
}
```
#### shouldComponentUpdate
Method này được call ngay trước render trong Udating Process và yêu cầu return true hoặc false. Nếu return false, render sẽ không được call và  componentDidUpdate cũng vậy, mặc định method này sẽ return true.

Đây là nơi để chúng ta đặt các xử lý caching, cải thiện performance cho component.

```
shouldComponentUpdate(nextProps, nextState){
  return nextProps.user.updatedAt != this.props.user.updatedAt
}
```

#### componentWillUnmount
Method này sẽ được gọi ngay trước khi component bị remove khỏi cây DOM.

Như vậy các bạn đã nắm được các callbacks nằm bên trong 1 ReactComponent, hy vọng bài viết này sẽ giúp các bạn vận dụng chính xác các methods này trong từng ngữ cảnh cụ thể.
