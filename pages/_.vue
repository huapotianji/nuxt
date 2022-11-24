<template>
  <div>
    内容{{ $route.path }}
      <mavon-editor
        :autofocus="false"
        ref="md"
        v-model="formData.mdContent"
        @change="getMdHtml"
        @imgAdd="uploadContentImg"
        @imgDel="delContentImg"
      />

      <el-button plain>朴素按钮</el-button>
      <el-button type="primary" @click="goOut" plain>主要按钮</el-button>
      <el-button type="success" plain>成功按钮</el-button>
      <el-button type="info" plain>信息按钮</el-button>
      <el-button type="warning" plain>警告按钮</el-button>
      <el-button type="danger" plain>危险按钮</el-button>

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
  }) {},
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
  head() {
    let metaConfig = {
      title: "title",
      htmlAttrs: {
        lang: "en",
      },
      meta: [
        { charset: "utf-8" },
        {
          hid: "description",
          name: "description",
          content: "desc",
        },
        {
          hid: "keywords",
          name: "keywords",
          content: "keywords",
        },
        { name: "viewport", content: "width=device-width, initial-scale=1" },
        { name: "AdsBot-Google", content: "noindex" },
      ],
      link: [
        {
          rel: "icon",
          type: "image/x-icon",
          href: `https://www.hotdeals.com/public/images/favicon.ico`,
        },
        {
          rel: "prefetch",
          href: "https://googleads.g.doubleclick.net",
        },
        { rel: "canonical", href: 'hostOrigin' },
      ],
      script: [
        {
          src: `https://www.googletagmanager.com/gtag/js?id=G-59TC1G3H2D`,
          async: "true",
        },
        {
          innerHTML: `window.dataLayer = window.dataLayer || [];
                      function gtag(){dataLayer.push(arguments);}
                      gtag('js', new Date());   
                      gtag('config', 'G-59TC1G3H2D');            
                          `,
          type: "text/javascript",
          charset: "utf-8",
          async: true,
        },
      ],
      __dangerouslyDisableSanitizers: ["script"],
    };
    return metaConfig;
  },
  mounted() {
    console.log(this.$route, "路由信息");
    gtag("event", "impression", {
      event_category: 'test-category',
      event_label: "test-lable",
      non_interaction: true,
      value: 1,
    });
  },
  methods: {
    getMdHtml() {},
    uploadContentImg() {},
    delContentImg() {},
    goOut() {
      console.log("111");
      window.open("http://localhost:5003/coupons/kohls/", "_blank");
    },
  },
};
</script>

<style scoped>
</style>