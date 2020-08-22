<template>
  <div class="virtual__list__container" @scroll="scrollHandle" ref="virtual__list__container">
    <div class="virtual__list__fill" :style="{height:count*itemSize+'px'}"></div>
    <ul class="virtual__list" :style="{top:offsetTop+'px'}">
      <li
        class="virtual__list__item"
        v-for="item in visibleData"
        :key="item"
        :style="{height:(item===actived?itemSize*5:itemSize)+'px'}"
        @click="clickHandle(item)"
        v-html="renderItem(item)"
      ></li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["dataSource", "itemSize", "renderItem"],
  data() {
    return {
      start: 0,
      scrollTop: 0,
      visibleCount: 0,
      offsetTop: 0,
      scale: 1,
      actived: -1,
    };
  },
  computed: {
    beforeCount() {
      return Math.min(this.start, this.visibleCount * this.scale);
    },
    afterCount() {
      return Math.min(this.count - this.end, this.visibleCount * this.scale);
    },
    end() {
      return Math.min(this.start + this.visibleCount, this.count);
    },
    visibleData() {
      return this.dataSource.slice(
        this.start - this.beforeCount,
        this.end + this.afterCount
      );
    },
    count() {
      return this.dataSource.length;
    },
  },
  mounted() {
    this.visibleCount = Math.ceil(
      this.$refs["virtual__list__container"].clientHeight / this.itemSize
    );
  },
  methods: {
    scrollHandle() {
      this.scrollTop = this.$refs["virtual__list__container"].scrollTop;
      this.start = Math.floor(this.scrollTop / this.itemSize);
      if (this.start - 1 > this.actived) {
        this.actived = -1;
      }
      this.offsetTop =
        this.scrollTop -
        this.beforeCount * this.itemSize -
        (this.scrollTop % this.itemSize);
    },
    clickHandle(index) {
      if (this.actived === index) {
        this.actived = -1;
        return;
      }
      this.actived = index;
    },
  },
};
</script>

<style>
.virtual__list__container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow-y: auto;
}
.virtual__list {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
}
.virtual__list__item {
  transition: all 0.3s;
}
</style>