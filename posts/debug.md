## 参考

[debug](https://www.npmjs.com/package/debug)

## Dynamically

```javascript
const debug = require("debug");
const httpDebug = debug("http");

if (httpDebug.enabled) {
  // do stuff...
}

debug.enable("foo:*,-foo-bar");

let namespaces = debug.disable();
debug.enable(namespaces);
```

## Extend

```javascript
const log = debug("auth");
// creates new debug instance with extended namespace
const logSign = log.extend("sign");

function circle(n) {
  // 遍历随机数
  for (let i = 0; i < n; i++) {
    // do nothing
  }
}
const circle9 = () => circle(999999999);

logSign("apple"); // auth:sign apple +0ms
circle9();
logSign("apple juice"); // auth:sign apple +781ms
circle9();
logSign("green apple juice"); // auth:sign apple +781ms
```
