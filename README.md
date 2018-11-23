# vue standard

*编写简洁漂亮，可维护性高的vue代码*

## 目录

- [前言](#前言)
- [目录命名](#目录命名)
- [全局方法和组件的扩展](#全局方法和组件的扩展)
- [Props 非空检测](#Props 非空检测)


---
## 前言
前端框架的普及和标准的混乱很难确定出一套规范，各企业也都随着项目制定自己技术团队所认可的标准和规范，本指南旨在vue本身的标准及eslint之外建立的符合团队认可的一套标准规范，以增加代码一致性和可读性，降低维护成本。

## 目录命名
**基于vue-cli规范src目录的统一性，使目录的可读性更高。**

```
├── src // 源代码
│   ├── api // 接口请求函数
│   ├── components // 项目公用组件及展示组件
│   ├── utils // 功能函数
│   ├── containers // 路由对应的组件（容器组件）
│   ├── router // 项目路由信息
│   ├── store // vuex数据
│   ├── style // css or sass
│   ├── images // 图片资源
│   ├── main.js //项目入口文件
```

## 全局方法和组件的扩展

**为实现各组件中调用公用方法的便利故使用以下方式进行全局方法和组件的扩展**

```javascript
// utils/tools.js
import fetcher from './fetcher'
import message from '@/base/tips'
export default {
  install (Vue) {
    Object.defineProperty(Vue.prototype, '$http', {
      enumerable: false,
      configurable: false,
      writable: false,
      value: fetcher
    })
    Object.defineProperty(Vue.prototype, '$message', {
      enumerable: false,
      configurable: false,
      writable: false,
      value: message
    })
  }
}

// main.js
import tools from '@/utils/tools'
Vue.use(tools)
```
**Vue.prototype.$http方式会导致在组件中误操作写入后续访问出问题，故使用Object.defineProperty设置属性的配置项**

## Props 非空检测
**对于props 必须对应设置 isRequired，避免再增加 if 分支带来的负担**