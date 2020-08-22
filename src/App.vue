<template>
  <div id="app">
    <VirtualList :dataSource="dataSource" :itemSize="100" :renderItem="renderItem" />
  </div>
</template>

<script>
import VirtualList from "@/components/VirtualList";

export default {
  name: "App",
  components: {
    VirtualList,
  },
  data() {
    return {
      dataSource: new Array(100).fill("").map((item, index) => index),
    };
  },
  methods: {
    renderItem(item) {
      return `<div>${item}</div>`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
html,
body {
  width: 100%;
  height: 100%;
}
#app {
  width: 450px;
  margin: 0 auto;
  background-color: #d9d9d9;
  height: 100%;
  position: relative;
}
</style>
