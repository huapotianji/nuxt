<template>
  <div>
    <div>
      <el-input
        placeholder="请输入ip地址"
        v-model="inputValue"
        class="input-with-select"
      >
        <el-button
          slot="append"
          icon="el-icon-search"
          @click="getWeather"
        ></el-button>
      </el-input>
    </div>
    <div id="container"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      map: "",
      weather: "",
      inputValue: "",
    };
  },
  mounted() {
    this.map = new AMap.Map("container", {
      zoom: 11, //级别
      center: [116.397428, 39.90923], //中心点坐标
      viewMode: "3D", //使用3D视图
    });
    //实时路况图层
    let trafficLayer = new AMap.TileLayer.Traffic({
      zIndex: 10,
    });
    this.map.add(trafficLayer); //添加图层到地图
    this.map.getCenter(); //

    // 添加插件
    AMap.plugin(
      ["AMap.ToolBar", "AMap.Driving", "AMap.Geolocation", "AMap.Weather"],
      () => {
        //异步同时加载多个插件
        let toolbar = new AMap.ToolBar();
        this.map.addControl(toolbar);
        let driving = new AMap.Driving(); //驾车路线规划
        driving.search(/*参数*/);
        let geolocal = new AMap.Geolocation(); //定位
        this.map.addControl(geolocal);
        //创建天气查询实例
        let weather = new AMap.Weather();
        weather.getLive("上海市", function (err, data) {
          console.log(err, data);
        });
      }
    );
  },
  methods: {
    getWeather() {
      console.log(this.weather, "this.weather");
      //执行实时天气信息查询
      this.weather.getLive("上海市", function (err, data) {
        console.log(err, data);
      });
    },
  },
  head() {
    let metaConfig = {
      title: "title",
      htmlAttrs: {
        lang: "en",
      },
      script: [
        {
          src: `https://webapi.amap.com/maps?v=2.0&key=2a273e265d278b7ce2f513b2ff93d1f7`,
          type: "text/javascript",
          charset: "utf-8",
          async: true,
        },
      ],
      __dangerouslyDisableSanitizers: ["script"],
    };
    return metaConfig;
  },
};
</script>
<style lang='scss' scoped>
#container {
  width: 100vw;
  height: 100vh;
}
</style>