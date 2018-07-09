# vue非父子组件之间通信

### Event Bus

首先, 新建一个 `bus.js`, 使用一个空的 `Vue` 实例作为事件总线

```
import Vue from 'vue'
export default new Vue()
```

再新建两个组件：

> `a` 组件

```
<template>
  <div class="aa">
	{{msg}}
	<button @click="toBus">子组件传给兄弟组件</button>
  </div>
</template>

<script>
import Bus from "../bus";// 引入bus.js
export default {
  name: "aa",
  data() {
    return {
      msg: "this is aa component"
    };
  },
  methods: {
    toBus() {
      Bus.$emit("on", "data from aa component");// 发送 `on` 事件
    }
  }
};
</script>

<style scoped>
</style>
```

> `b` 组件

```
<template>
  <div class="bb">
      {{msg}}
  </div>
</template>

<script>
import Bus from "../bus";// 引入bus.js
export default {
  name: "bb",
  data() {
    return {
      msg: "this is bb component"
    };
  },
  mounted() {
    Bus.$on("on", msg => { // 监听 `on` 事件
      this.msg = msg;
    });
  }
};
</script>

<style scoped>
</style>
```

最后将 `a` , `b` 组件展示出来

```
<template>
  <div class="hello">
    <A></A>
    <B></B>
  </div>
</template>

<script>
import A from './a.vue'
import B from './b.vue'
export default {
  name: 'HelloWorld',
  components:{
    A,
    B
  }
}
</script>

<style scoped>
</style>
```

此时点击 `a` 组件中的按钮，就会发现 `b` 中的 `msg` 变成了 `data from aa component`
