# ppxoj-frontend
## 皮皮翔OJ在线判题系统 (前端代码仓库)

基于Spring Boot + Spring Cloud 微服务+ Docker (+ Vue 3 + Arco Design)的编程题目在线评测系统。

Spring Boot + Spring Cloud Microservices + Docker (+ Vue 3 + Arco Design)

在系统前台，管理员可以创建、管理题目;用户可以自由搜索题目、阅读题目、编写并提交代码。

在系统后端，能够根据管理员设定的题目测试用例在自主实现的代码沙箱中对代码进行编译、运行、判断输出是否正确。其中，代码沙箱可以作为独立服务，提供给其他开发者使用。

Frontend: Administrators can create and manage problems, while users can freely search for problems, read problem statements, write and submit code.

Backend: The system can compile, run, and judge the correctness of the code based on test cases set by the administrators in a self-implemented code sandbox. This code sandbox can also be provided as an independent service for other developers to use.

![主页图片](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/mainPage.jpg)
![浏览题目](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/questionView.jpg)
![浏览提交题目](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/questionSubmitView.jpg)
![创建题目](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/createQuestionView.jpg)
![管理题目](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/manageQuestionView.jpg)
![做题页面](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/doQuestion.jpg)
![关于](https://github.com/pipixiangz/ppxoj-frontend/blob/main/imgs/about.jpg)

### 前端

1.基于Vue3 + Arco Design组件库，自主实现了在线做题、题目检索和管理、提交列表、用户登录等页面。

2.使用Vue-CLI脚手架初始化项目，并自行开发了全局页面布局和通用前端项目模板，便于后续复用。

3.使用TypeScript + ESLint + Prettier + Husky保证项目编码和提交规范，提高项目的质量。

4.全局导航生成:基于Vue Router的路由配置文件自动生成导航菜单，并通过给路由的meta 属性增加 hidden字段实现集中控制页面的显隐。

5.全局权限管理:通过给 Vue Router路由的meta 属性增加 access 字段来定义页面权限，然后通过 beforeEach 全局路由守卫集中校验用户进入页面的权限，并进一步将权限管理相关代码统一封装为 access.ts模块，简化用户使用。

6.全局状态管理:基于Vuex定义User Module实现了对登录用户的状态存储，并通过组合式 API (useStore)在页面中访问用户信息。

7.前后端联调:使用 openapi-typescript-codegen 工具根据后端 Swagger 接口文档自动生成请求后端的代码，大幅提高开发效率。

8.选用 ByteMD 开源 Markdown 文本编辑器组件，引入gfm 插件(支持表格语法)并进一步自行封装了可复用的Editor 和 Viewer，实现了题目内容及答案的编辑功能。

9.使用 Arco Design的Table组件实现了题目检索页面，并通过自定义插槽将后端返回的JSON数据解析为美观的格式。

### Frontend

Based on Vue 3 + Arco Design Component Library, self-implemented pages for online problem-solving, problem searching and management, submission lists, and user login.

Initialized the project using Vue-CLI scaffold and developed a global page layout and common frontend project template for easy reuse.

Used TypeScript + ESLint + Prettier + Husky to ensure coding and submission standards, improving project quality.

Global Navigation Generation: Automatically generated navigation menus based on Vue Router's route configuration file and implemented centralized control of page visibility by adding a hidden field to the route's meta attribute.

Global Permission Management: Defined page permissions by adding an access field to the Vue Router route's meta attribute and implemented centralized verification of user access to pages through the beforeEach global route guard. Further encapsulated permission management related code into an access.ts module to simplify usage.

Global State Management: Defined a User Module based on Vuex for storing the logged-in user's state and accessed user information in pages through the Composition API (useStore).

Frontend-Backend Integration: Used the openapi-typescript-codegen tool to automatically generate backend request code based on backend Swagger interface documentation, significantly improving development efficiency.

Selected ByteMD Open Source Markdown Text Editor Component, introduced the gfm plugin (supports table syntax), and further encapsulated reusable Editor and Viewer, enabling the editing of problem content and answers.

Used Arco Design's Table Component to implement the problem search page and parsed the JSON data returned by the backend into a visually appealing format through custom slots.

根据后台backend生成代码

```
openapi --input http://localhost:8121/api/v2/api-docs --output ./generated --client axios
```

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

