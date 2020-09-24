## commonjs

```javascript
// mod.js
module.exports = { a: 1 };

// app.js
var mod = require("./mod");
var { a } = require("./mod");
// mod > {a:1}
// a > 1
```

## es6

```javascript
// mod.mjs
export const da = { a: 1 };
export function fn() {}
export default { b: 2 };

// app.mjs
import * as modAlias from "./mod.mjs";
import { da } from "./mod.mjs";
import mod from "./mod.mjs";
import { default as defaultAlias } from "./mod.mjs";
// modAlias > {da:{a:1},fn:function(){},default:{b:2}}
// da > {a:1}
// mod > {b:2}
// defaultAlias > {b:2}
```

> node >= v8.5 执行 es6 模块 `node --experimental-modules xxx.mjs`
