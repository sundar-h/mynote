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



## 开发步骤
* 先定义页面 components
* 页面跳转:路由 router
* mock 数据
* 后端请求请求api



### vite.config.ts
* 主要配置vite服务器的远程请求地址
* 端口号
