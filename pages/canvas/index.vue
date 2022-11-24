<template>
  <div>
    <canvas ref="myCanvas"></canvas>
  </div>
</template>
<script>
export default {
  props: {},
  data() {
    return {};
  },
  mounted() {
    let myCanvas = this.$refs.myCanvas;
    let ctx = myCanvas.getContext("2d");
    // 1.斜线
    // ctx.moveTo(0, 0);
    // ctx.lineTo(200, 100);
    // ctx.stroke();
    // 2.圆形
    // ctx.beginPath();
    // ctx.arc(95, 50, 40, 0, 2 * Math.PI);
    // ctx.stroke();
    // 3.实心
    // ctx.font = "30px Arial";
    // ctx.fillText("Hello World", 10, 50);
    // 4.空心
    ctx.font = "30px Arial";
    ctx.strokeText("Hello World", 10, 50);
  },
};
</script>
<style lang='scss' scoped>
</style>