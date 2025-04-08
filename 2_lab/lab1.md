# lab 1

## Git协作开发

lab 1 : part 1 基础md提交

### CodeArts的SSH密钥设置 : 引出SSH中config设置原理

1. `.ssh/config`文件一般结构

    ```config
    Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa
    ```

    补充 : 代理设置

    ```config
        ProxyCommand nc -x IP:Port -X 5 %h %p 
    ```

2. 上传密钥到服务器的快速方法

    ```bash
    ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
    ```

### 一般流程

1. `git clone` : 克隆仓库
2. `git branch` : 查看分支
3. `git checkout -b branch_name` : 创建并切换到新分支
4. `git add .` : 添加所有文件
5. `git commit -m "message"` : 提交
6. `git push origin branch_name` : 推送到远程仓库

### git commit message规范

参考:  
[约定式提交](https://www.conventionalcommits.org/zh-hans/v1.0.0/#%e7%ba%a6%e5%ae%9a%e5%bc%8f%e6%8f%90%e4%ba%a4%e8%a7%84%e8%8c%83)

## 前后端分离开发实践

lab 1 : part 2 前后端分离Demo

### 基础概念

- **API** : Application Programming Interface (应用程序接口)
- **REST** ： Representational State Transfer (表述性状态转移)
- **RESTful API** : 一种API设计风格，`REST API`的一种实现  

### 工具

1. **Apifox** : API设计与管理
2. **Vue3** : Web前端开发框架  
3. **Node.js** : Node.js® 是一个免费、开源、跨平台的 JavaScript 运行时环境, 它让开发人员能够创建服务器 Web 应用、命令行工具和脚本  
4. **Spring Boot** : Web后端开发框架  

## 具体了解部分

### RESTful API

- REST(Representational State Transfer)
    是一种软件架构风格，是一种设计风格而不是标准，只是提供了一组设计原则和约束条件  
    - RE(representational) : 资源表现层，资源是具体的实体，即URI  
    - ST(state transfer) : 状态转化，即客户端通过HTTP协议操作服务器资源  
    - 通俗解释REST：数据在网络中以某种表现形式进行状态转移  
- RESTful API是REST架构风格的Web API  
    相关最基本元素：  
    - 资源  
    - URI(Uniform Resource Identifier, 统一资源标识符)，标识某一互联网资源的字符串  
    - HTTP动词(方法)  
        - GET : 获取资源
        - POST : 新建资源
        - PUT : 更新资源
        - DELETE : 删除资源
        - PATCH : 更新资源的部分内容


### Apifox

官网：[Apifox](https://apifox.com/)  

### Vue3

系统平台:Windows

**安装**  

```powershell
winget install Schniz.fnm
fnm install 22
```

**创建项目、安装依赖库、启动项目**  

```powershell
npm create vite@latest
npm install
npm run dev
```

**项目文件结构**  

```txt
<project_name>/
├──node_modules
├──public
├──src
├──package.json
├──package-lock.json
├──README.md
└──vite.config.js
```

**Vue组件**  

`.vue`文件结构  

1. `<template>` : HTML or Vue模板语法定义的组件UI
2. `<script>` : JS脚本 or TS脚本
3. `<style>` : CSS样式

    - 链接模块  

        ```vue
        <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>
        ```

        - href: 这个属性指定了链接的目标 URL。在你的代码中，href="https://cli.vuejs.org" 表示点击这个链接会打开 Vue CLI 的官方网站。
        - target: 这个属性指定了链接的打开方式。target="_blank" 表示点击链接后会在一个新的浏览器标签页或窗口中打开目标 URL。
        - rel: 这个属性定义了当前文档与目标文档之间的关系。rel="noopener" 是一个安全属性，表示新打开的页面不会获得对原始页面的 window.opener 对象的访问权限。这有助于防止新页面对原始页面进行潜在的恶意操作。
