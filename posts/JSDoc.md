## 使用

- 安装 `npm install -g jsdoc`

### 配置

- 创建配置文件 `conf.json` 后，就可方便的执行 `jsdoc -c conf.json <ProjectDir>`

#### 参数

- `--tutorials <dirPath>` 将搜索到的教程作为导航显示在 API 文档的侧边。
  - 目录内支持的文件类型包括：html、md、xml 等。默认以文件名称作为标题，可以通过在目录内定义 .json 文件修改目录的顺序、标题、层次。

### 模板

#### 变更模板

以使用 [Jaguar.js](https://github.com/davidshimjs/jaguarjs-jsdoc) 模板为例。步骤如下

- 将模板项目拷贝到自己项目 `myProject` 的某个目录内，例如放置到 `jsdocTpl` 目录中。
- 在 `myProject` 根目录，执行 `jsdoc -t ./jsdocTpl src`

## 参考

- [JSDoc](https://jsdoc.app/index.html)
- [JSDoc 中文文档](https://www.html.cn/doc/jsdoc/index.html)
