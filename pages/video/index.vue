<template>
  <div class="video">
    <video autoplay muted loop :src="require('@/assets/video/one.mp4')"></video>
  </div>
</template>
<script>
export default {
  props: {},
  data() {
    return {};
  },
  head(){
     let metaConfig = {
      link: [
        {
          rel: 'preload',
          href: '@/assets/video/one.mp4',
          as:'script'
        },
      ],
    }
    return metaConfig
  }
};
</script>
<style lang='scss' scoped>
.video{
    width: 100vw;
    height: 100vh;
    background: #fff;
    overflow: hidden;
    video{
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: center center;
        object-fit: fill;
    }
}
</style>