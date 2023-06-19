<template>
  <div class="box">
    <div class="one">1</div>
    <div class="two">2</div>
    <div class="three">3</div>
    <div class="four">4</div>
    <div class="five">5</div>
  </div>
</template>
<script>
export default {
  props: {},
  data() {
    return {};
  },
};
</script>
<style lang='scss' scoped>
.box{

}
.one{
    width: 500px;
    height: 700px;
    background: rgb(238, 152, 152);
}
.two{
    width: 500px;
    height: 700px;
    background: rgb(154, 196, 151);
}
.three{
    width: 500px;
    height: 700px;
    background: rgb(126, 96, 118);
    position: sticky;
    top: 110px;
}
.four{
    width: 500px;
    height: 700px;
    background: rgb(77, 36, 126);
}
.five{
    width: 500px;
    height: 700px;
    background: rgb(49, 32, 136);
}
</style>