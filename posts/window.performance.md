window.performance 是 W3C 性能小组引入的 API，IE9 以上的浏览器都兼容

## 属性

### timing

记录从输入 url 到用户可以使用页面的全过程的时间统计，单位毫秒。

#### 应用

```javascript
function getPerformanceTiming() {
  var performance = window.performance;

  if (!performance) {
    // 当前浏览器不支持
    console.log("你的浏览器不支持 performance 接口");
    return;
  }

  var t = performance.timing;
  var times = {};

  //【重要】页面加载完成的时间
  //【原因】这几乎代表了用户等待页面可用的时间
  times.loadPage = t.loadEventEnd - t.navigationStart;

  //【重要】解析 DOM 树结构的时间
  //【原因】反省下你的 DOM 树嵌套是不是太多了！
  times.domReady = t.domComplete - t.responseEnd;

  //【重要】重定向的时间
  //【原因】拒绝重定向！比如，http://example.com/ 就不该写成 http://example.com
  times.redirect = t.redirectEnd - t.redirectStart;

  //【重要】DNS 查询时间
  //【原因】DNS 预加载做了么？页面内是不是使用了太多不同的域名导致域名查询的时间太长？
  // 可使用 HTML5 Prefetch 预查询 DNS ，见：[HTML5 prefetch](http://segmentfault.com/a/1190000000633364)
  times.lookupDomain = t.domainLookupEnd - t.domainLookupStart;

  //【重要】读取页面第一个字节的时间
  //【原因】这可以理解为用户拿到你的资源占用的时间，加异地机房了么，加 CDN 处理了么？加带宽了么？加 CPU 运算速度了么？
  // TTFB 即 Time To First Byte 的意思
  // 维基百科：https://en.wikipedia.org/wiki/Time_To_First_Byte
  times.ttfb = t.responseStart - t.navigationStart;

  //【重要】内容加载完成的时间
  //【原因】页面内容经过 gzip 压缩了么，静态资源 css/js 等压缩了么？
  times.request = t.responseEnd - t.requestStart;

  //【重要】执行 onload 回调函数的时间
  //【原因】是否太多不必要的操作都放到 onload 回调函数里执行了，考虑过延迟加载、按需加载的策略么？
  times.loadEvent = t.loadEventEnd - t.loadEventStart;

  // DNS 缓存时间
  times.appcache = t.domainLookupStart - t.fetchStart;

  // 卸载页面的时间
  times.unloadEvent = t.unloadEventEnd - t.unloadEventStart;

  // TCP 建立连接完成握手的时间
  times.connect = t.connectEnd - t.connectStart;

  return times;
}
```

补充：

- 解析 dom 树耗时 = domComplete - domInteractive
- 白屏时间 = domloadng - fetchStart
- domReady 时间 = domContentLoadedEventEnd - fetchStart

### memory

记录内存多少，单位字节。在 Chrome 中添加的非标准属性。

- jsHeapSizeLimit: 内存大小限制
- totalJSHeapSize: 可使用的内存
- usedJSHeapSize: JS 对象(包括 V8 引擎内部对象)占用的内存，不能大于 totalJSHeapSize，如果大于，有可能出现了内存泄漏

### navigation

记录当前页面是通过什么导航过来的。仅有两个属性：type、redirectCount

type 枚举值描述包括

- 普通进入
- 刷新进入
- 通过操作历史记录进入
- 其他非以上类型的方式进入

###

应用

## 方法

### getEntries() 返回页面中所有 HTTP 请求（包括本地缓存）信息的数组

数组内对象属性包括：

- name：资源名称，是资源的绝对路径或调用 mark 方法自定义的名称
- startTime:开始时间
- duration：加载时间
- entryType：资源类型，entryType 类型不同数组中的对象结构也不同！具体见下表
- initiatorType：谁发起的请求，具体见下表

#### entryType 对照表

|            |                             |                                               |
| ---------- | --------------------------- | --------------------------------------------- |
| mark       | PerformanceMark             | 通过 mark()方法添加到数组中的对象             |
| measure    | PerformanceMeasure          | 通过 measure()方法添加到数组中的对象          |
| resource   | PerformanceResourceTiming   | 所有资源加载时间，用处最多                    |
| navigation | PerformanceNavigationTiming | 现除 chrome 和 Opera 外均不支持，导航相关信息 |
| frame      | PerformanceFrameTiming      | 现浏览器均未支持                              |
| server     | PerformanceServerTiming     | 未查到相关资料                                |

#### initiatorType 对照表

|                                      |                           |                                                              |
| ------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| a Element                            | link/script/img/iframe 等 | 通过标签形式加载的资源，值是该节点名的小写形式               |
| a CSS resourc                        | css                       | 通过 css 样式加载的资源，比如 background 的 url 方式加载资源 |
| a XMLHttpRequest object              | xmlhttprequest            | 通过 xhr 加载的资源                                          |
| a PerformanceNavigationTiming object | navigation                | 当对象是 PerformanceNavigationTiming 时返回                  |

### getEntriesByName(name,type[optional]) 返回制定资源名信息的集合

### getEntriesByType(type) 返回页面中所有 HTTP 请求信息的数组

### clearResourceTimings()

该方法无参数无返回值，可以清楚目前所有 entryType 为"resource"的数据，用于写单页应用的统计脚本非常有用

### mark(markName)

标记当前时间，可使用 `clearMarks([name])` 方法清除标记。

```javascript
// 获取所有标记的数据
var marks = window.performance.getEntriesByType("mark");
console.log("marks: ", marks);
```

### measure(measureName,startMarkName,endMarkName)

测量两个标记点的时间差，使用 `clearMeasures([name])` 清除。

```javascript
// 所有测量记录
var measure = window.performance.getEntriesByType("measure");
console.log("measure: ", measure);

// 获取单个测量记录
var entries = window.performance.getEntriesByName("measureRandomFunc888");
console.log("entries: ", entries);
```

### now() 更精准的计算程序执行时间（微秒）

## 参考

- https://www.w3.org/TR/navigation-timing-2/
- https://www.cnblogs.com/libin-1/p/6501951.html
- https://www.cnblogs.com/cangqinglang/p/9892745.html
