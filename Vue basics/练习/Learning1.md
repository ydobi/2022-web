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

## 4、下面代码在vue3、vue2中的差异如何？为什么？如果是数组呢？如何解决？(代码)
> [Demo04](Learning1-demo/demo04.html)
- Vue2 
  - 对象： `Object.defineProperty` get和set
  - 数组： 重写了 `push()` `pop()` `unshift()` `shift()` `splice()` `sort()` `reverse()`这几个方法
- Vue3 Proxy（代理） 和 Reflect（反射）
  ```javaScript
    new Proxy(data,{
      // 拦截读取属性值
      get(target,prop){
        return Reflect.set(target,prop)
      },
      // 拦截设置或添加属性值
      set(target,prop,value){
        return Reflect.set(target,prop,value)
      },
      // 拦截删除属性值
      deleteProperty(target,prop){
        return Reflect.deleteProperty(target,prop)
      },
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
> [Demo8](Learning1-demo/demo08.html)


## 9、$watch和watch的区别？（代码）
> [Demo9](Learning1-demo/demo09.html)
- $watch vm api
- watch 配置项

## 10、补充删除方法（代码）
> [Demo10](https://codesandbox.io/s/veevalidate-components-element-ui-veevalidate-3-0-forked-i7p6xb?file=/src/App.vue)

## 11、如何在父组件中监听到子组件的生命周期？（代码）
> [Demo11](Learning1-demo/demo11.html)

## 12、使用异步组件、动态组件写一个列表（代码）
> [Demo12](https://codesandbox.io/s/veevalidate-components-element-ui-veevalidate-3-0-forked-i7p6xb?file=/src/views/Demo14.vue)

## 13、如何给事件增加自定义参数（代码）
> [Demo13](Learning1-demo/demo13.html)
- 在DOM事件的回调函数中传入参数`$event`，可以获取到该事件的事件对象
- 在子组件中通过`$emit`注册事件，将数据作为参数传入，在父组件中通过`$event`接收

## 14、自定义组件的v-model（代码）
> [Demo14](Learning1-demo/demo14.html)

## 15、写一个组件使用.sync的示例，并口述他和v-model使用场景上的区别？（代码）
> [Demo15](Learning1-demo/demo15.html)
- `v-model` = `:value` 和 `@input`, 一个组件只能有一个，通常用于表单类组件，作单一值的修改
- `v-bind.sync`, 可以有多个

## 16、 包装一个mtd-select组件，并能够在爷爷组件中使用mtd-select所有的方法和props（代码）
> [Demo16](Learning1-demo/demo16.html)

## 17、 写一个clickoutside的自定义指令（代码）
> [Demo17](Learning1-demo/demo17.html)

## 18、 写一个keep-alive的示例（代码）
> [Demo18](https://codesandbox.io/s/veevalidate-components-element-ui-veevalidate-3-0-forked-i7p6xb?file=/src/App.vue)

## 19、 介绍nextTick、$set、$parent、$root、mixin的使用场景（口述）
- nextTick
  - 功能：在下一次DOM更新结束后执行其指定的回调
  - 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

- $set
  - 功能：向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的

- $parent
  - 功能：访问父实例

- $root
  - 功能：当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。

- mixin
  - 功能：可以把多个组件公用的配置提取为一个混入对象
  - 混入规则：
    - 数据对象以组件为主
    - 生命周期钩子混入对象前置

## 20、写一个在线代码运行器(代码)
> [Demo20](Learning1-demo/demo20.html)