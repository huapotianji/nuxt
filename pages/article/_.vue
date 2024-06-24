<template>
  <div>
    <header-new :headData='headData'/>
    <component
      :is="currentComponents.type"
      :key="currentComponents.key"
      :params="currentComponents.params"
      :pageData="pageData"
    />
    <article-footer />
  </div>
</template>
<script>
import { headData } from "./data";
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
        type: "view-article-inside",
        key: "articleInside",
      },
      headData:headData,
      pageData:{}
    };
  },
};
</script>
<style lang='scss' scoped>
</style>