BOM 的 API

## 属性

- length

- state

## 方法

### pushState

history.pushState(stateInfo,title,url)

### replaceState

history.pushState(stateInfo,title,url)

### back

- back() 后退

### forward

- forward() 前进

### go

- go(-1) 等同于 back()
- go(1) 等同于 forward()

## 事件

### onpopstate

```javascript
window.onpopstate = (event) => {
  console.log(event.state);
};
```
