# Reflect

提供静态方法用于操作对象。

> 将原先挂在 Object 上的语言相关的方法转义到 Reflect 对象上。

## 使用

### defineProperty(target, property, attributes)

拦截目标对象的属性赋值

`Object.defineProperty` => `const result = Reflect.defineProperty()`

```javascript
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

等效于

```javascript
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}
```

### has

Reflect.has
~= ’xx’ in obj

### set(target,propName,propValue,receiver)

```javascript
let obj = {
  name: "foo",
};

let result = Reflect.set(obj, "name", "bar");
console.log(result); // true
console.log(obj.name); // bar
```

### get

const value = Reflect.get(obj,key)
~= 与 API 与 Proxy 一致，handler

### deleteProperty(obj, name)

`delete obj.name` => `Reflect.deleteProperty(obj,name)`

### construct

`new Func(100)` => `Reflect.construct(Func,[100])`

### getPrototypeOf(target)

获取目标对象的原型对象

`Object.getPrototypeOf(obj)` => `Reflect.getPrototypeOf(obj)`

### setPrototypeOf(obj, newPrototype)

获取目标对象的原型对象

`Object.setPrototypeOf(obj,newPrototype)` => `Reflect.setPrototypeOf(obj,newPrototype)`

## 参考

https://www.jianshu.com/p/4a5eca0536c3
