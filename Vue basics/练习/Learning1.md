# Vue 基础使用
[TOC]
## 1、不同构建包有何不同？（代码）
> https://cn.vuejs.org/v2/guide/installation.html#%E5%AF%B9%E4%B8%8D%E5%90%8C%E6%9E%84%E5%BB%BA%E7%89%88%E6%9C%AC%E7%9A%84%E8%A7%A3%E9%87%8A

- 开发版：包含完整的警告和调试模式
  - vue.js
  - vue.runtime.js
- 生产版本：删除了警告
  - vue.min.js
  - vue.runtime.min.js

```javaScript
// 需要编辑器
new Vue({
  template: '<div>{{ hello }}<div>'
})

// 不需要编译器
new Vue({
  render(h) {
    return h('div','hello')
  }
})
```

### 知识补充
- Vue 
  - 组件化：提高复用率、维护性
  - 声明式：无需直接操作DOM
  - 虚拟DOM+diff算法：增加DOM复用率
- [UMD](https://github.com/umdjs/umd) (Universal Module Definition): patterns for JavaScript modules that work everywhere.

## 2、简单介绍下双向绑定。（口述）
- v-model
  - text 和 textarea 元素使用 `value` property 和 `input` 事件；
  - checkbox 和 radio 使用 `checked` property 和 `change` 事件；
  - select 字段将 `value` 作为 prop 并将 `change` 作为事件。

## 3.如何不让data中的数据进行双向绑定？（代码）
- data中数据如何进行双向绑定
  1. Vue 将data数据拷贝一份到`_data`属性中
  2. 将 `_data` 属性提到 `vm` 上，通过 `Object.defineProperty()` 实现数据代理
  3. 通过 `getter/setter` 操作数据操作，实现响应式

- 如何阻止双向绑定
  1. `Object.freeze` 一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。

### 知识补充
- Object.defineProperty 方法
```javaScript
let number = 18
let person = {
  name: '张三',
  sex: '男',
}

Object.defineProperty(person, 'age', {
  // value:18,
  // enumerable:true,		// 控制属性是否可以枚举，默认值是false (for...in 或 Object.keys 方法)
  // writable:true,			// 控制属性是否可以被修改，默认值是false
  // configurable:true	// 控制属性是否可以被删除，默认值是false

  // 当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
  get() {
    console.log('有人读取age属性了')
    return number
  },

  // 当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
  set(value) {
    console.log('有人修改了age属性，且值是', value)
    number = value
  }

})
```
## 5、动态绑定不同的事件（代码）
> [Demo5](Learning1-demo/demo05.html)

## 6、computed和watch的区别（代码）
> [Demo6](Learning1-demo/demo06.html)
- computed能做到的watch一定能做到，
- computed是声明式的，watch是命令式
- computed 不能执行异步函数

## 7、computed和函数的区别？（代码）
> [Demo7](Learning1-demo/demo07.html)
- computed 能够缓存
  - `getter` 触发时机：（1）初次读取 （2）当依赖的数据发生改变

## 8、用computed完成双向绑定（代码）


## 9、$watch和watch的区别？（代码）
> [Demo9](Learning1-demo/demo09.html)
- $watch vm api
- watch 配置项

## 10、补充删除方法（代码）
> [Demo10](https://codesandbox.io/s/veevalidate-components-element-ui-veevalidate-3-0-forked-i7p6xb?file=/src/App.vue)

## 13、如何给事件增加自定义参数（代码）
> [Demo13](Learning1-demo/demo13.html)
- 在DOM事件的回调函数中传入参数`$event`，可以获取到该事件的事件对象
- 在子组件中通过`$emit`注册事件，将数据作为参数传入，在父组件中通过`$event`接收

## 14、自定义组件的v-model（代码）
> [Demo14](Learning1-demo/demo14.html)

## 18、 写一个keep-alive的示例（代码）
> [Demo18](https://codesandbox.io/s/veevalidate-components-element-ui-veevalidate-3-0-forked-i7p6xb?file=/src/App.vue)

## 19、 介绍nextTick、$set、$parent、$root、mixin的使用场景（口述）

## 20、写一个在线代码运行器(代码)