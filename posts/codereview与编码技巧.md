## 参考

- [[译]提升你 react 和 js 编码的 5 个技巧](https://juejin.im/post/6844904099331178509)

### 函数

#### 封装过程类代码

函数作为常用且简单的代码封装“工具”，在封装代码上下文时的优点：

- 主函数内执行多个过程函数。提升了
- 通过函数名，取代上下文中的注释。避免了一个函数体内，通过大量的注释来说明上下文的执行过程。

#### if...else 中的异常处理统一放在一侧

```javascript
if (state1) {
  // do stuff
  if (!state2) {
    // throw
  } else {
    // do stuff
  }
} else {
  // throw
}

// 改为

if (state1) {
  // do stuff
  if (state2) {
    // do stuff
  } else {
    // throw
  }
} else {
  // throw
}
```

#### 覆盖原方法实现惰性函数

如果判断较多，资源节省还是可观的。

```javascript
function foo() {
  if (a !== b) {
    console.log("aaa");
  } else {
    console.log("bbb");
  }
}

// 改写

function foo() {
  if (a != b) {
    // 经过判断后覆盖原函数
    foo = function () {
      console.log("aaa");
    };
  } else {
    foo = function () {
      console.log("bbb");
    };
  }
  return foo();
}
```

#### 数据合法性检查函数化

```javascript
function isSuccessResponse() {}
function responseCallback(data) {
  if (isSuccessResponse(data)) {
    // do stuff
  }
}
```

#### 使用策略模式改写 if...if...if（类 switch 场景）

```javascript
function main(state) {
  if (state === 1) {
    // ...
    return;
  }
  if (state === 2) {
    // ...
    return;
  }
}
// 改为

function handleState1() {}
function handleState2() {}
const handleStateMap = {
  1: handleState1,
  2: handleState2,
};
function main(state) {
  return handleStateMap[state]();
}
```

#### 使用职责链模式改写 if...elseif

```javascript
function main(state) {
  if (state === 1) {
    // ...
  } else if (state === 2) {
    // ...
  }
}

// 改为

function handle1(state) {}
function handle2(state) {}
const handles = [handle1, handle2];
function main(state) {
  for (let handle of handles) {
    if (handle(state)) return;
  }
}
```

#### 使用解构提升可读性

```javascript
const UserProfile = (props) => (
  <div>
    <span>{props.firstName}</span>
    <span>{props.lastName}</span>
    <img src={props.profilePhoto} />
  </div>
);

// 改为

const UserProfile = ({ firstName, lastName, profilePhoto }) => (
  <div>
    <span>{firstName}</span>
    <span>{lastName}</span>
    <img src={profilePhoto} />
  </div>
);
```

#### 命名约定

变量命名建议

- 组合单词命名的变量，前缀使用形容词。例如 tableTitle、maxCount 等
- 函数命名，使用驼峰结构，前缀使用动词，用于操作属性的，前缀使用 get 、set。例如 getValue、setTitle 等。
- 返回或值为布尔值时，命名前缀为：is、has、should 等
- 事件处理函数，使用 handle+eventName 为前缀，例如 handleClick。

|动词|含义|
|can|判断是否可以进行某个操作|
|has|判断是否含有某个值|
|is|判断是否为某个值|
|get|获取值|
|set|设置值|
|load|加载数据|

```javascript
// 例子1

let User = {};
User.car = true;
User.admin = true;

function NewUser() {
  return User;
}

function add_photo(photo) {
  user.photo = photo;
}

// 改为

function getUser() {
  return user;
}

function setUserPhoto(photoUrl) {
  user.photoUrl = photoUrl;
}
```

#### 使用 await + promise.all 解决复杂（依赖）数据流

```javascript
const [user, account] = await Promise.all([fetch("/user"), fetch("/account")]);
```

#### 强制传入参数

```javascript
mandatory = () => {
  throw new Error("Missing parameter!");
};
foo = (bar = mandatory()) => {
  // 这里如果不传入参数，就会执行 mandatory 函数报出错误
  return bar;
};
```

#### 函数默认值

```javascript
func = (a, b = 2, c = 3) => a + b + c;
func(1); // 6 注意，null 会覆盖参数默认值
```

#### 一次性函数，第一次执行后，函数体将变更

```javascript
var foo = function () {
  console.log("msg");
  foo = function () {
    console.log("foo");
  };
};
foo(); // msg
foo(); // foo
foo(); // foo
```

### 组件

#### 展开操作符，分离公共属性与自定义属性

为注入公共属性（key、className、style）准备

```javascript
const UserProfile = (props) => {
  const { firstName, lastName, profilePhoto, ...rest } = props;
  return (
    <div {...rest}>
      <span>{firstName}</span>
      <span>{lastName}</span>
      <img src={profilePhoto} />
    </div>
  );
};
```

#### 分离组件内业务逻辑代码，使组件作为展示组件

将组件内的业务逻辑分离出，将计算后的数据传入组件供其展示。使组件容易调试、测试、维护。

```javascript
import axios from 'axios'

const UserProfile = props => {
  const [user, setUser] = React.useState(null);
  React.useEffect(() => {
    getUser();
  }, []);

  async function getUser() {
    try {
      const user = await axios.get('/user/25')
    } catch(error) {
      console.error(error)
    }

    if(user.country === "DE") {
      user.flag = "/de-flag.png"
    } else if(user.country === "MX") {
      user.flag = "/mx-flag.png"
    }
    setUser(user);
  }

  const { firstName, lastName, profilePhoto, userFlag} = user

  return (<div>
    <span>{firstName}</span>
    <span>{lastName}</span>
    <img src={profilePhoto}/>
    <img src={userFlag}>
  </div>)
}
```

改为 dumb components 方式。

> dumb components 指“哑组件”，指仅用于展示的组件，不依赖 store、actions，接收与改变数据均有 props，内部不必使用 state，内部可能会引入其它 dumb components、this.props.children

```javascript
// UserProfilePage.jsx
// 操作所有的UserProfilePage相关，添加任何额外的props或业务逻辑

import { fetchUser } from '../api'

const UserProfilePage = props => {
  const [user, setUser] = React.useState(null);
  React.useEffect(() => {
    getUser();
  }, []);

  async function getUser() {
    const user = fetchUser(error => console.error(error))
    if(user.country === "DE") {
      user.flag = "/de-flag.png"
    } else if(user.country === "MX") {
      user.flag = "/mx-flag.png"
    }
    setUser(user);
  }
  return <UserProfile {...user}/>
}

// API.js
// 获取数据并处理错误

export const fetchUser = async (errorHandler) => {
  try {
    const user = await axios.get('/user/25')
    retrun user
  } catch(error) {
    errorHandler(error)
  }
}

// UserProfile.jsx
// UserProfile.jsx如下

const UserProfile = props => {
  const { firstName, lastName, profilePhoto, ...rest} = props
  return (<div {...rest}>
    <span>{firstName}</span>
    <span>{lastName}</span>
    <img src={profilePhoto}/>
  </div>)
}

```

#### 添加组件的类型检查

```javascript
interface IUserProfile {
  firstName: string
  lastName: string
  profilePhoto: string
  shouldShowPhoto?: boolean
}

const UserProfile = (props: IUserProfile) => {
  const { firstName, lastName, profilePhoto, shouldShowPhoto } = props
  return (<div>
    <span>{firstName}</span>
    <span>{lastName}</span>
    {shouldShowPhoto && <img src={profilePhoto}/>}
  </div>)
}
```

### 运算符

#### 判断奇偶数

与位

```javascript
const num = 3;
!!(num & 1); // true 奇数
!!(num % 2); // true 偶数
```

#### 取整数

使用：或位、双位、Math.floor

```javascript
1.3 | 0; // 1 正数
-1.9 | 0; // -1 负数
Math.floor(4.9) === 4; //true
~~4.9 === 4; //true
~~-4.5 === -4; //true
```

#### 短路运算符

从左向右执行，只要遇到满足条件的表达式就停止执行并返回

```javascript
let param1 = expr1 && expr2; // 遇到 false 停止
let param2 = expr1 || expr2; // 遇到 true 停止
```

#### 类型转化

转化为数字（加减乘除均可）

```javascript
+"123"; // 123
+"ds"; // NaN
+""; // 0
+null; // 0
+undefined; // NaN
+{ valueOf: () => "3" }; // 3
```

### 原生 API

#### 过滤数组中假值

```javascript
const compact = (arr) => arr.filter(Boolean);
compact([0, 1, false, 2, "", 3, "a", "e" * 23, NaN, "s", 34]); // [ 1, 2, 3, 'a', 's', 34 ]
```

### 数字

#### 数字前补 0

```javascript
const addZero1 = (num, len = 2) => `0${num}`.slice(-len);
const addZero2 = (num, len = 2) => `${num}`.padStart(len, "0");
addZero1(3); // 03
addZero2(32, 4); // 0032
```

#### 返回指定位数的小数

```javascript
const round = (n, decimals = 0) =>
  Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
round(1.345, 2); // 1.35
round(1.345, 1); // 1.3
```

### 数组

#### 使用 reduce 实现 map + filter 的效果

```javascript
// 遍历并过滤数组
const numbers = [1, 2, 3, 4, 5];
const rlt = numbers.reduce((arr, item) => {
  if (item > 3) {
    arr.push(item);
  }
  return arr;
}, []);
// rlt => [4,5]
```

#### 使用 reduce 实现 hashMap 计数效果

```javascript
// 遍历并过滤数组
const numbers = ["a", "b", "b"];
const rlt = numbers.reduce((countMap, key) => {
  countMap[key] = countMap[key] ? ++countMap[key] : 1;
  return countMap;
}, {});
// rlt => {a:1, b:2}
```

#### 解构数组实现交互变量的值

```javascript
let param1 = 1;
let param2 = 2;
[param1, param2] = [param2, param1];
console.log(param1); // 2
console.log(param2); // 1
```

#### 获取数组值生成对象

```javascript
const csvFileLine = "1997,John Doe,US,john@doe.com,New York";
const { 2: country, 4: state } = csvFileLine.split(",");

country; // US
state; // New Yourk
```

### 对象

#### 使用解构过滤对象中不需要的属性

```javascript
const old = { a: 1, b: 2, _c: 3 };
const { _c, ...newObj } = old;
// newObj => {a:1, b:2}
```

### 其它

#### xxx

```javascript

```
