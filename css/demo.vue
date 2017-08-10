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
  import Child from './Child.vue'
  export default {
    data() {
      return {
        total: 10000
      }
    },
    components: {Child}
  }
</script>