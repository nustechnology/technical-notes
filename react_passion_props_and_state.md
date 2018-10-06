## React Passion: Props and State

Chào các bạn, hôm nay mình sẽ chia sẻ kiến thức về Props và State trong React, cũng như mối liên hệ giữa chúng trong/ngoài Component. Blog này hướng đến đối tượng là các bạn đã làm việc một thời gian với React, hãy dạo qua https://reactjs.org/ nếu bạn chưa có vốn kiến thức nào.

#### Props là gì?

Props là 1 thành phần của Component, nó là một object bất biến bên trong Component, nghĩa là không thể change Props của 1 Component bên trong Component đó.

#### Cách khởi tạo Props?
```
<User name="david" age="30" />
```

#### Khi nào Props của Component thay đổi?

Props của Component sẽ thay đổi khi giá trị từ bên ngoài truyền vào thay đổi

```
render(){
  let user = {...this.state.user}

  return <User name={user.name} age={user.age}/>
}
```
Mỗi khi user thay đổi, Component User sẽ nhận được Props mới

#### State là gì?

State là 1 thành phần của Component, nó là một object có thể thay đổi được và được khởi tạo bên trong Component.

#### Cách khởi tạo State?

Có 2 cách để khởi tạo state, 1 là bên trong `constructor` của class Component, 2 là bên trong callback `getDerivedStateFromProps` của Component

*1. Khởi tạo State bằng Component constructor*

constructor sẽ được run 1 lần duy nhất khi Component được khởi tạo

```
class User extends Component {

  constructor(props){
    super(props)
    this.state = {
      user: {
        name: "David",
        age: 30
      }
    }
  }
}
```

*2. Khởi tạo State bằng getDerivedStateFromProps*

callback getDerivedStateFromProps sẽ được gọi khi Props của Component thay đổi, callback này nhận vào 2 đối số là nextProps và oldState, callback này yêu cầu trả ra State mới

```
class User extends Component {
  static getDerivedStateFromProps(nextProps, oldState){
    let newState = oldState
    newState.user = {
      name: nextProps.name,
      age: nextProps.age
    }
  }

  return newState;
}
```

#### Làm cách nào để thay đổi State?

Nếu chúng ta gán trực tiếp `this.state.user.name = "Ken"`, chúng ta sẽ nhận được 1 warning từ React và điều này thật sự không đúng đắn, một trong những hậu quả là Component sẽ không tự render lại view, dẫn đến những thay đổi ở State sẽ không được phản ánh ra view.

Cách duy nhất để thay đổi State là `this.setState`

```
let user = {...this.state.user}
user.name = "Ken"
this.setState({user: user})
```
