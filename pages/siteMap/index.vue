<template>
  <div>
    <client-only>
      <el-button plain @click="changeMap(1)">地球模式</el-button>
      <el-button plain @click="changeMap(2)">标准模式</el-button>
      <el-button plain @click="changeMap(3)">卫星地图</el-button>
    </client-only>
    <div id="container"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      map: "",
    };
  },
  methods: {
    changeMap(item) {
      console.log(item, "item");
      if (item == 1) {
        this.map.setMapType(BMAP_EARTH_MAP);
      } else if (item == 2) {
        this.map.setMapType(BMAP_NORMAL_MAP);
      } else if (item == 3) {
        this.map.setMapType(BMAP_SATELLITE_MAP);
      }
    },
  },
  mounted() {
    this.map = new BMapGL.Map("container");
    let point = new BMapGL.Point(116.404, 39.915);
    this.map.centerAndZoom(point, 15);
    this.map.enableScrollWheelZoom(true); //开启滚轮缩放
    this.map.setHeading(64.5); //设置地图旋转角度
    this.map.setTilt(75); //设置地图的倾斜角度
    // 比例尺控件坐标
    let opts = {
      offset: new BMapGL.Size(150, 0),
    }
    var scaleCtrl = new BMapGL.ScaleControl(opts); // 添加比例尺控件
    this.map.addControl(scaleCtrl);
    let zoomCtrl = new BMapGL.ZoomControl(); // 添加缩放控件
    this.map.addControl(zoomCtrl);
    var cityCtrl = new BMapGL.CityListControl(); // 添加城市列表控件
    this.map.addControl(cityCtrl);
  },
  head() {
    let metaConfig = {
      title: "title",
      htmlAttrs: {
        lang: "en",
      },
      script: [
        {
          src: `https://api.map.baidu.com/api?v=1.0&type=webgl&ak=GaLIxHTYbpyUpE4Rf8Q5i2U6A5Q5qpRM&callback=initialize`,
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
  width: 100%;
  height: calc(100vh - 40px);
}
</style>