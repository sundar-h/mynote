# 创建
```
// 创建vue typescript 项目
yarn create vite lambda-admin --template vue-ts
// 引入 vue-route-next
yarn add vue-router@4
// 引入element-plus
yarn add element-plus
// 引入vuex
yarn add vuex@next
// 引入eslint
yarn add -D eslint eslint-plugin-vue @vue/eslint-config-typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
// 引入Prettier
yarn add -D prettier eslint-plugin-prettier @vue/eslint-config-prettier
// 全局样式
yarn add node-sass sass-loader sass
// 运行
yarn dev
```

* vue-next
* vue-router-next
* typescript



## Vscode IDE
vscode内置的typscript language server不好用, 改用这个
* JavaScript and TypeScript Nightly



* **一步一步走、用到再添加**
## 引入依赖
* 响应式框架 vue3
* 路由 router
* 状态管理 vuex
* 优秀组件库 element-plus
* 静态检查 统一配置 ESlint
* 全局环境变量 配置 全局样式配置sass
* [vue pipeline](https://jinfang134.github.io/vue-pipeline/)



## 开发步骤
* 先定义页面 components
* 页面跳转:路由 router
* mock 数据
* 后端请求请求api



### vite.config.ts
* 主要配置vite服务器的远程请求地址
* 端口号



### [吐槽](https://github.com/justwiner/vue3-tsx)
 各种配置太多了 
 * 集成TS: tsconfig.json
 * 使用vite工具: vite.config.ts
 * 集成eslint代码校验: .eslintrc
 * 集成stylelint代码校验: .stylelintrc
 * 集成pritter代码格式化工具: .prettierrc
 * 集成vue-router: router/index.ts
 * 集成vuex: store/index.ts
 * Pre-commit Hook 在代码commit前，对处于staged状态下的文件进行一次格式化，避免提交的格式不符合要求。


### [ESlint JS代码检查](https://cn.eslint.org/docs/user-guide/configuring)
```
yarn global add eslint --dev
eslint --init # 生成.eslintrc文件
eslint lib/** # run eslint
```
``` .eslintrc
{
   "extends": "eslint:recommended"
}
```

### [stylelint is designed for CSS](https://stylelint.io/user-guide/configure)
```
yarn global add --dev stylelint stylelint-config-standard  stylelint-config-standard-scss
# run stylelint
npx stylelint "**/*.css"
```

```.stylelintrc
{
  "extends": "stylelint-config-standard"
}
  # "extends": "stylelint-config-standard-scss"
```


### [tsconfig.json  编译项目的选项: tsc命令配置](https://typescript.bootcss.com/tsconfig-json.html)


### [vite.config.ts vite命令配置 运行项目](https://cn.vitejs.dev/config/)


### [pritter 代码格式话工具.prettierrc](https://www.prettier.cn/docs/configuration.html)
```
yarn global add --dev --exact prettier
```

```.prettierignore 
build
coverage
```

### husky and lint-staged: Pre-commit Hook
```
yarn add -D husky lint-staged

```

```package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,vue}": "eslint --fix"
  }
}
```



## 引用
[Building a Vue3 Typescript Environment with Vite](https://miyauchi.dev/posts/vite-vue3-typescript/)
[element-plus-admin](https://github.com/hsiangleev/element-plus-admin)
[vite-vue3-template](https://github.com/TomokiMiyauci/vite-vue3-template)
[vue-next-admin](https://github.com/lyt-Top/vue-next-admin)
