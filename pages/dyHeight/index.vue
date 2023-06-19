<template>
  <div>
    <div
      class="box"
      :style="{ height: currentHeight + 'px' }"
      @click="toggleHeight"
    >
      <div class="title">Click me</div>
      <div class="content">
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
        11111111
        <br />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isExpanded: false, // 初始状态为收起
    };
  },
  computed: {
    currentHeight() {
      return this.isExpanded ? this.calculateHeight() : 22; // 根据状态计算当前高度
    },
  },
  methods: {
    toggleHeight() {
      this.isExpanded = !this.isExpanded; // 切换展开/收起状态
    },
    calculateHeight() {
      // 这里可以根据需求编写计算高度的逻辑，例如获取元素的实际高度
      // 返回一个动态计算的高度值
      let height = document.querySelector(".content").offsetHeight;
      return height + 22;
    },
  },
};
</script>

<style>
.box {
  width: 200px;
  background-color: lightblue;
  transition: height 0.5s;
  overflow: hidden;
}
</style>
