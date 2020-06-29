# vue 中 debounce（防抖）实现

## debounce 组件封装

- DebounceInput.vue（子组件）

```vue
<template>
  <a-input placeholder="Basic usage" @change="handleChange" />
</template>

<script>
export default {
  data() {
    return {
      timeout: null,
    };
  },
  methods: {
    handleChange(e) {
      if (this.timeout !== null) clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.$emit("inputChange", e.target.value);
      }, 5000);
    },
  },
};
</script>
```

## 组件适用

- 父组件

```vue
<template>
  <DebounceInput @inputChange="inputChange"></DebounceInput>
</template>
<script>
···
methods: {
  inputChange(data) {
      console.log(data)
    }
}
</script>
```
