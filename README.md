## Eslint + Prettier + Husky + Commitlint+ Lint-staged 规范前端工程代码规范

测试项目用的是 vite+vue3 框架

### 代码规范

#### 代码检查工具 - Eslint

- 安装依赖
  `npm i eslint -D`
- init 命令自动生成 .eslintrc.js 文件
  `npx eslint --init`
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-1.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-2.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-3.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-4.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-5.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/eslint-6.png)

- .eslintrc.js 文件内容：

```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: ['eslint:recommended', 'plugin:vue/essential'],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  plugins: ['vue'],
  rules: {}
}
```

### git 规范

> feat：新功能（feature）
> fix：修补 bug
> docs：文档（documentation）
> style：格式（不影响代码运行的变动）
> refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
> perf：提高性能的代码更改
> test：增加测试
> build：影响构建系统或外部依赖项的更改(示例范围:gulp、broccoli、npm)
> ci：对 ci 配置文件和脚本的更改(示例范围:Travis, Circle, BrowserStack, SauceLabs)
> chore：构建过程或辅助工具的变动
> revert: 恢复以前的提交(回退)
