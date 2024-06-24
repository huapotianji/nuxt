<template>
  <div class="wrap">
    <div
      class="cooupert"
      :class="{ 'all-coupert': isShowAll, 'rotate-coupert': !isShowAll }"
      @click="goplugIn"
      @mouseenter="isShowAll = true"
      @mouseleave="isShowAll = false"
    >
      <img class="coupert-icon" src="@/static/img/icon_extension.png" alt="" />
      <div class="text" v-html="coupert + headData.add_text"></div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    headData: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      coupert: 'Coupert-',
      isShowAll: true,
      timer: null
    }
  },
  mounted() {
    this.timer = setTimeout(() => {
      this.isShowAll = false
      clearTimeout(this.timer)
      this.timer = null
    }, 5000)
  },
  methods: {
    // 跳转到插件
    goplugIn() {
      this.coupertUrl =
        this.headData.countryCode == ''
          ? `https://www.svtrk.com/adServe/aff?p=${encodeURIComponent(
              `https://www.coupert.com/extension?use=hd-${this.headData.countryCode.toLowerCase()}_seo_header-button_${
                location.pathname
              }_${this.headData.DomainUrl}&uct=EXTENSION&ucn=${
                location.origin
              }&merchant=${this.headData.termName}&umm=${
                this.headData.countryCode
              }&lang=${this.headData.lang}`
            )}`
          : `https://www.coupert.com/extension?utm_source=hd-${this.headData.countryCode.toLowerCase()}_seo_header-button_${encodeURIComponent(
              location.pathname
            )}_${encodeURIComponent(
              this.headData.DomainUrl
            )}&utm_content=EXTENSION&utm_campaign=${encodeURIComponent(
              location.origin
            )}&merchant=${this.headData.termName}&utm_medium=${
              this.headData.countryCode
            }&lang=${this.headData.lang}`
      window.open(this.coupertUrl, '_blank')
    }
  }
}
</script>
<style lang='scss' scoped>
.wrap {
  background: linear-gradient(135deg, #fef5f5 0%, #fce8e7 100%);
  border-radius: 12px;
  padding: 10px;
  overflow: hidden;
  height: 40px;
  display: flex;
  align-items: center;
}
.cooupert {
  max-width: 20px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  transition: all 0.8s ease-in-out;
  cursor: pointer;
  position: relative;
  &::before {
    content: '';
    position: absolute;
    width: 12px;
    height: 12px;
    background: #ffffff;
    box-shadow: 0px 1px 8px 0px rgba(241, 81, 76, 0.59);
    border-radius: 50%;
    left: 14px;
    bottom: 7px;
    background-image: url(@/static/img/icon_download.png);
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
  }
  &:hover {
    max-width: 250px;
    .text {
      opacity: 1;
    }
  }
  .coupert-icon {
    width: 20px;
    height: 20px;
  }
  .text {
    padding-left: 16px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    opacity: 0;
    transition: all 0.8s ease-in-out;
    font-size: 14px;
    font-weight: 500;
    color: #222222;
  }
}
.all-coupert {
  max-width: 250px;
  .text {
    opacity: 1;
  }
}
.rotate-coupert {
  animation: rotation 3s infinite;
  animation-delay: 0.5s;
}
@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(360deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>