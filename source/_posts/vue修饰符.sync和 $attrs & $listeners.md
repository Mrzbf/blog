---
title: vue修饰符.sync和 $attrs & $listeners
date: 2019-09-27 16:40:00
tags:
- vue
categories:
- javaScript
---
### vue修饰符.sync
[.sync 修饰符官网介绍](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6)，多用于父子组件数据双向绑定

用法demo dialog例子
```
// 父组件
<template>
  <div>
    <button @click="show = !show" class="btn-switch">开关</button>
    <c-dialog :show.sync="show"></c-dialog>
  </div>
</template>

<script>
import dialog from "./components/dialog";
export default {
  data() {
    return {
      show: true
    };
  },
  components: {
    "c-dialog": dialog
  }
};
</script>

<style lang="less" scoped>
.btn-switch {
  margin-bottom: 10px;
}
</style>

//子组件
<template>
  <div>
    <div class="box" v-show="visible">
      展示
      <button class="btn-close" @click="visible = !visible">X</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    visible: {
      get() {
        return this.show;
      },
      set(val) {
        this.$emit("update:show", val);
      }
    }
  }
};
</script>

<style lang="less" scoped>
.box {
  position: relative;
  margin: 0 auto;
  width: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  border: 1px solid red;
}
.btn-close {
  position: absolute;
  right: 5px;
  top: 5px;
}
</style>

```
### $attrs & $listeners
[$attrs官网介绍](https://cn.vuejs.org/v2/api/?#vm-attrs)，[$listeners官网介绍](https://cn.vuejs.org/v2/api/?#vm-listeners),两者多用于创建高层次的组件

demo如下
```
//爷爷组件
<template>
  <div>
    这是爷爷组件，这是爷爷收到的消息:{{ text }}
    <c-children :msg="msg" @changeText="handleTextChange"></c-children>
  </div>
</template>

<script>
import children from "./components/children";
export default {
  data() {
    return {
      msg: "我是爷爷啊",
      text: "未收到消息"
    };
  },
  components: {
    "c-children": children
  },
  methods: {
    handleTextChange(val) {
      this.text = val;
    }
  }
};
</script>


//父组件

<template>
  <div>
    这是儿子组件
    <c-grandson v-bind="$attrs" v-on="$listeners"></c-grandson>
  </div>
</template>

<script>
import grandson from "./grandson";
export default {
  components: {
    "c-grandson": grandson
  }
};
</script>

//子组件
<template>
  <div>
    这是孙子组件，收到的消息:{{ msg }}
    <button @click="$emit('changeText', '我是孙子啊')">给爷爷发消息</button>
  </div>
</template>

<script>
export default {
  props: {
    msg: {
      type: String,
      default: "1"
    }
  }
};
</script>

<style lang="scss" scoped></style>

```
