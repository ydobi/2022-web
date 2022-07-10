# Vue 基础使用
## 1、不同构建包有何不同？（代码）
> https://cn.vuejs.org/v2/guide/installation.html#%E5%AF%B9%E4%B8%8D%E5%90%8C%E6%9E%84%E5%BB%BA%E7%89%88%E6%9C%AC%E7%9A%84%E8%A7%A3%E9%87%8A

- 开发版：包含完整的警告和调试模式
  - vue.js
  - vue.runtime.js
- 生产版本：删除了警告
  - vue.min.js
  - vue.runtime.min.js


### 知识补充
- Vue 
  - 组件化：提高复用率、维护性
  - 声明式：无需直接操作DOM
  - 虚拟DOM+diff算法：增加DOM复用率
- [UMD](https://github.com/umdjs/umd) (Universal Module Definition): patterns for JavaScript modules that work everywhere.

## 2、简单介绍下双向绑定。（口述）