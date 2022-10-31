<template>
  <div>
    内容{{ $route.path }}
    <client-only>
      <mavon-editor
        :autofocus="false"
        ref="md"
        v-model="formData.mdContent"
        @change="getMdHtml"
        @imgAdd="uploadContentImg"
        @imgDel="delContentImg"
      />
    </client-only>
    <client-only>
      <el-button plain>朴素按钮</el-button>
      <el-button type="primary" plain>主要按钮</el-button>
      <el-button type="success" plain>成功按钮</el-button>
      <el-button type="info" plain>信息按钮</el-button>
      <el-button type="warning" plain>警告按钮</el-button>
      <el-button type="danger" plain>危险按钮</el-button>
    </client-only>
  </div>
</template>
<script>
import { GaList } from "./data";
export default {
  async asyncData({
    isDev,
    route,
    store,
    env,
    params,
    query,
    req,
    res,
    redirect,
    error,
    $axios,
  }) {
    
  },
  data() {
    return {
      currentComponents: {
        type: "view-page-test",
        key: "pageTest",
        params: {},
        apiUrl: "/channel_site/",
      },
      formData: {
        mdContent: "",
      },
    };
  },
  mounted() {
    console.log(this.$route, "路由信息");
  },
  methods: {
    getMdHtml() {},
    uploadContentImg() {},
    delContentImg() {},
  },
};
</script>

<style scoped>
</style>