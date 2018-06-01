# Javascript Testing
本文对常用前端测试框架、工具进行分析总结。

## 测试类型
通常测试包括：
- 单元测试：对模拟输入与预期输出的方式测试独立的函数与类。
- 集成测试：将多个功能模块进行连接，并校验连接后的整体功能是否符合预期。
- 功能测试（UI测试）：不关心具体内部实现，只对真实环境下使用是否符合预期进行校验。
  - 快照测试：

## 方案
具体选择一个框架或多个框架或工具进行结合，是视测试需求而定的。框架没有优劣之分，每一个框架都有它的优点与不足。

### QUnit 
由 jQuery 团队成员开发，是 jQuery 库使用的单测框架。项目起于 2018年5月，2009年末被重构，独立于 jQuery。

特点
- 运行环境 Browser > Node
- 遵循 commonJS 规范
- 自带断言库
- 使用方便
  - 新建 html 文件，引入指定的 js 和 css
  - 编写测试代码
  - 打开本地浏览器访问 html 文件，查看测试结果
- 哪些知名团队使用？jQuery

参考
- [QUnit](http://qunitjs.com/)

### Jasmine
“BDD for JavaScript”

特点
- 适用于 Browser、Node
- 不依赖三方库，自身提供 assert、mocks、spies、stubs 等功能
- 哪些知名团队使用？淘宝UED

参考
- [Jasmine](https://jasmine.github.io/)
- [Github](https://github.com/jasmine/jasmine)

### Mocha + Chai
Mocha 是目前广泛使用的测试框架，它需要依赖三方库（chai），才具有 assert、spies、mock 等功能。

特点
- 运行环境 Node > Browser
- 功能强大，有较强的扩展性
- 学习曲线陡峭

参考
- [Mocha](https://Mochajs.org/)
- [Chai](https://Mochajs.org/)

## 开发模式
### TDD
测试驱动开发 `test-driven development` 。
- 在实现功能开发前，先完成测试代码，在进行功能开发。
- 服务于单元测试，检查函数的输入与输出是否符合预期。

### BDD
行为驱动开发 `behavior-driven development` 。
- 鼓励非开发人员参与协作。
- 从用户的行为出发，强调系统行为。
- 包括验收测试、客户测试驱动等。
- 检查已知流程是否合法，执行函数与传入参数是否符合预期。

## 补充知识
### karma
karma 是写 Angularjs 的工程师们创造出的。严格意义说，它只是一个测试运行器，它帮你加载依赖的测试框架，运行你的测试代码，并返回运行结果。使用它能方便的进行自动化测试，实现测试驱动开发的完整流程。

特点
- 支持多种应用场景，例如 karma-chrome-launcher、karma-firefox-launcher、karma-jsdom-launcher 等
- 支持多种测试框架，例如 karma-qunit、karma-jasmine、karma-Mocha 等

参考
- [官网](http://karma-runner.github.io/)
- [Github](https://github.com/karma-runner/karma)


### Spies
记录函数被执行的次数、被谁调用等信息，通常在获取性能消耗方面有很大的帮助。

### Stubs
假定某个函数返回设定的值。例如某个流程需要经过三个环节，我们如何从第二个环节开始测试？可以预期第一个环节已经返回给定的结果。

### Mocks || Fakes
假定某个服务的行为，服务通常由多个方法或服务的聚合。所以仅使用 Stubs 是无法覆盖的。例如模拟 http 服务。

### 浏览器环境
除真实浏览器环境下运行测试代码外，还可以用类浏览器环境，例如 
- JSDom：一个纯净的模拟真实浏览器的 JS 环境，没有 UI 不做任何界面渲染。但是它提供了 window、document、body、location、cookies、选择器等等所有在浏览器中运行的 JavaScript 代码可能用到的对象。
- Headless Browser：“无头浏览器”，提供了无界面的浏览器，能更快执行代码。
