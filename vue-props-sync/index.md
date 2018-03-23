# Vue 中的 .sync 修饰符有什么用


<!--more-->



## Vue 中的 .sync 修饰符有什么用

>   [官方文档](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6)

+   .sync 是「实现 Vue 组件间传值」的一个语法糖


​	

### 组件间传值的实现

>   props 传值遵循[单向数据流](https://cn.vuejs.org/v2/guide/components-props.html#%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E6%B5%81)原则：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样可以防止子组件意外变更父组件的数据，从而导致你的应用的数据流向难以理解。

#### 思路

+   数据从父组件传入子组件，我们无法在子组件中直接修改 props 传入的外部数据，只能在父组件中进行修改，而更新后的数据会自动流向子组件
+   [props](https://cn.vuejs.org/v2/guide/components-props.html#ad) 加 [vm.$emit](https://cn.vuejs.org/v2/api/#vm-emit) 实现组件传值
    +   props 用于接收父组件的数据
    +   vm.$emit（自定义事件）用于触发父组件事件，并传参

#### 具体实现

>   事件的发布、订阅

[在线演示](https://codesandbox.io/s/objective-cloud-qnzvy?file=/src/Parent.vue)

```vue
<!-- Parent.vue -->
<template>
  <div class="app">
    Parent.vue 现在有 {{ total }}
    <hr />
    <!-- 向 Child 组件传递 money 属性 -->
    <Child :money="total" @update:money="total = $event" />
  </div>
</template>

<script>
import Child from "./Child.vue"
export default {
  data() {
    return {
      total: 10000,
    }
  },
  components: { Child }, 
}
</script>
```

```vue
<!-- Child.vue -->
<template>
  <div class="child">
    Child.vue 现在有 {{ money }}
    <button @click="$emit('update:money', money-100)">花钱</button>
  </div>
</template>

<script>
export default {
  props: ["money"], // 接收到 money 属性
}
</script>
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/传值.gif" alt="传值" style="zoom:80%;" />

​	

​	

### .sync 的使用

```vue
<!-- Parent.vue -->
<Child :属性名.sync="数据" />
```

#### 示例

>   [在线演示](https://codesandbox.io/s/sweet-pine-pij57?file=/src/Parent.vue)

```vue
<!-- Parent.vue -->
<Child :money.sync="total" />
```

👆 会扩展出一个完整的事件绑定 👇

```vue
<!-- Parent.vue -->
<Child :money="total"  @update:money="total = $event" />
```

>   当子组件需要更新「数据 total 」时，它需要显式的触发一个更新事件「`update:属性名`」

```
// Child.vue
this.$emit('update:属性名', 更新数据)
👇
<button @click="$emit('update:money', money-100)">花钱</button>
```

​	

​	

## 参考

https://www.jianshu.com/p/6b062af8cf01

https://segmentfault.com/a/1190000010700521
