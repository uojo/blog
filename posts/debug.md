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
