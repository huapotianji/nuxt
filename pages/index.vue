<template>
  <div>
    <div>首页</div>
    <div class="link-body">
      <a target="_blank" href="/canvas">canvas</a>
      <a target="_blank" href="/chatgpt">chatgpt</a>
      <a target="_blank" href="/echars">echars</a>
      <a target="_blank" href="/highMap">highMap</a>
      <a target="_blank" href="/pagesOne">pagesOne</a>
      <a target="_blank" href="/siteMap">siteMap</a>
      <a target="_blank" href="/test">test</a>
      <a target="_blank" href="/three">three</a>
      <a target="_blank" href="/users">users</a>
      <a target="_blank" href="/video">video</a>
    </div>
    <Tutorial />
    <test />
  </div>
</template>

<script>
export default {
  name: "IndexPage",
};
</script>
<style lang="scss" scoped>
.link-body {
  margin: 10px 0;
  display: flex;
  justify-content: center;
  a {
    padding: 0 10px;
    border: 1px solid rgb(199, 193, 193);
    margin-right: 10px;
    color: #000;
    text-decoration: none;
    display: block;
    height: 40px;
    line-height: 40px;
    &:hover {
      background: #e1c3c3;
    }
  }
}
</style>
