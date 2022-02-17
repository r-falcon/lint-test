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

#### 代码风格工具 - prettier

- 安装依赖

`npm i prettier eslint-config-prettier eslint-plugin-prettier -D`

- 在.eslintrc 中,extend 中添加 "prettier" 解决 eslint 和 prettier 的冲突[因为 eslint-config-prettier 新版本更新之后,只需要写一个 "prettierr" 即可,无需多言指令]

- .eslintrc.js 文件内容改动：

```js
  extends: ['eslint:recommended', 'plugin:vue/essential', 'prettier']
```

- 创建 .prettierrc

```js
{
  "semi": false,
  "tabWidth": 2,
  "trailingComma": "none",
  "singleQuote": true,
  "arrowParens": "avoid"
}
```

### git 规范

- git 规范内容：

```html
<div>
  <p>feat：新功能（feature）</p>
  <p>fix：修补 bug</p>
  <p>docs：文档（documentation）</p>
  <p>style：格式（不影响代码运行的变动）</p>
  <p>refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）</p>
  <p>perf：提高性能的代码更改</p>
  <p>test：增加测试</p>
  <p>build：影响构建系统或外部依赖项的更改(示例范围:gulp、broccoli、npm)</p>
  <p>
    ci：对 ci 配置文件和脚本的更改(示例范围:Travis, Circle, BrowserStack,
    SauceLabs)
  </p>
  <p>chore：构建过程或辅助工具的变动</p>
  <p>revert: 恢复以前的提交(回退)</p>
</div>
```

- git 常用钩子
  提交的代码规范 pre-commit
  提交的信息规范 commit-msg

- 工具
  husky[操作`git`钩子的工具]
  lint-staged[本地暂存代码检查工具]
  commitlint[`commit`信息校验工具]
  commitizen[辅助`commit`信息]

- 安装流程：

* 安装代码校验依赖

  - 依赖安装：`npm i lint-staged husky -D`
    在 package.json 中添加脚本：`npm set-script prepare "husky install"`
    ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/husky-1.png)

  - 初始化 husky：
    `npx husky add .husky/pre-commit "npx lint-staged"`
    ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/husky-2.png)

  - 根目录创建 .lintstagedrc.json 文件控制检查和操作方式

  ```json
  {
    "*.{js,jsx,ts,tsx}": ["prettier --write", "eslint --fix"],
    "*.md": ["prettier --write"]
  }
  ```

* 安装提交信息依赖

  - 依赖安装：
    `npm i commitlint @commitlint/config-conventional -D`
  - husky 目录：
    `npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'`
    ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/husky-3.png)

* 安装辅助提交信息
  - 依赖安装：
    `npm i commitizen -D`
  - 编辑 commit 指令[初始化命令行选项信息,不然没有`git`提交类别选项]
    `npx commitizen init cz-conventional-changelog --save-dev --save-exact`
  - 此时 package.json 的 commit 配置项为：
    ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/husky-4.png)

### 测试

- git commit 随便写个内容：
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/test-1.png)
- git 正确提交：[一定要注意在类别冒号后面有空格，且`commit`用双引号]
