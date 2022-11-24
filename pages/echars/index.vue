<template>
  <div id="echarts">
    <div id="myChart"></div>
    <div id="myChart2"></div>
  </div>
</template>
<script type="text/javascript">
export default {
  name: "Echarts",
  data() {
    return {};
  },
  methods: {
    echartsInit() {
      // 找到容器
      let myChart = this.$echarts.init(document.getElementById("myChart"));
      // 开始渲染
      myChart.setOption({
        title: { text: "在Vue中使用echarts" },
        tooltip: {},
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      });
    },
    echartsInit2() {
      let myChart = this.$echarts.init(document.getElementById("myChart2"));
      myChart.setOption({
        title: { text: "echarts 饼图" },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            data: [
              { value: 235, name: "视频广告" },
              { value: 274, name: "联盟广告" },
              { value: 310, name: "邮件营销" },
              { value: 335, name: "直接访问" },
              { value: 400, name: "搜索引擎" },
            ],
          },
        ],
      });
    },
  },
  mounted() {
    console.log(this.$echarts, "this");
    this.echartsInit();
    this.echartsInit2();
  },
};
</script>
<style scoped lang="scss">
#myChart {
  width: 600px;
  height: 300px;
  margin-left: auto;
  margin-right: auto;
  float: left;
}
#myChart2 {
  width: 600px;
  height: 300px;
  margin-left: auto;
  margin-right: auto;
  float: right;
}
</style>