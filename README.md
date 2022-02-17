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
  - 创建 commitlint.config.js 文件,内容如下：
    ```js
    module.exports = {
      extends: ['@commitlint/config-conventional'],
      rules: {
        'type-enum': [
          2,
          'always',
          [
            'feat',
            'fix',
            'docs',
            'style',
            'refactor',
            'test',
            'chore',
            'revert'
          ]
        ],
        'subject-full-stop': [0, 'never'],
        'subject-case': [0, 'never']
      }
    }
    ```

### 测试

- git commit 随便写个内容：
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/test-1.png)
- git 正确提交：[一定要注意在类别冒号后面有空格，***并且***`commit`用双引号]
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/test-2.png)
  ![示例图片](https://github.com/r-falcon/lint-test/blob/main/src/assets/demo/test-3.png)

### 自定义规范

- 安装依赖
  `npm i -D commitlint-config-cz cz-customizable`
- 变更 commitlint.config.js,变更后内容如下：

  ```js
  module.exports = {
    extends: ['cz'],
    rules: {
      // 自定义规则
    }
  }
  ```

- 安装指令和命令行的展示信息
  `npm set-script commit "git-cz"`
  package.json 文件变动如下：

  ```json
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "prepare": "husky install",
    "commit": "git-cz"
  }
  ```

- 增加 .cz-config.js 文件,内容如下：

```js
'use strict'
module.exports = {
  types: [
    { value: '✨特性', name: '特性:一个新的特性' },
    { value: '🐛修复', name: '修复:修复一个Bug' },
    { value: '📝文档', name: '文档:变更的只有文档' },
    { value: '💄格式', name: '格式:空格, 分号等格式修复' },
    { value: '♻️重构', name: '重构:代码重构，注意和特性、修复区分开' },
    { value: '⚡️性能', name: '性能:提升性能' },
    { value: '✅测试', name: '测试:添加一个测试' },
    { value: '🔧工具', name: '工具:开发工具变动(构建、脚手架工具等)' },
    { value: '⏪回滚', name: '回滚:代码回退' }
  ],
  scopes: [
    { name: '模块1' },
    { name: '模块2' },
    { name: '模块3' },
    { name: '模块4' }
  ],
  // it needs to match the value for field type. Eg.: 'fix'
  /*  scopeOverrides: {
    fix: [
      {name: 'merge'},
      {name: 'style'},
      {name: 'e2eTest'},
      {name: 'unitTest'}
    ]
  },  */
  // override the messages, defaults are as follows
  messages: {
    type: '选择一种你的提交类型:',
    scope: '选择一个scope (可选):',
    // used if allowCustomScopes is true
    customScope: 'Denote the SCOPE of this change:',
    subject: '短说明:\n',
    body: '长说明，使用"|"换行(可选)：\n',
    breaking: '非兼容性说明 (可选):\n',
    footer: '关联关闭的issue，例如：#31, #34(可选):\n',
    confirmCommit: '确定提交说明?(yes/no)'
  },
  allowCustomScopes: true,
  allowBreakingChanges: ['特性', '修复'],
  // limit subject length
  subjectLimit: 100
}
```

- 修改 package.json 中的 commit 配置

```json
"config": {
  "commitizen": {
    "path": "node_modules/cz-customizable"
  }
}
```
