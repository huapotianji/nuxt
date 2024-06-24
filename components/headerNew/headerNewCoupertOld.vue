<template>
  <div class="coupert-old" @click="goplugIn">
    <img class="coupert-icon" src="@/static/img/coupert-trem.png" alt="" />
    <div class="text" v-html="coupert + headData.add_text"></div>
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
      coupert: 'Coupert-'
    }
  },

  methods: {
    // 跳转到插件
    goplugIn() {
      this.coupertUrl = this.headData._cpsrc ? `https://www.coupert.com/extension?${this.headData._cpsrc}`
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
.coupert-old {
  height: 40px;
  border-radius: 12px;
  background: #f1514c;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  font-size: 14px;
  font-weight: 700;
  padding: 7.5px 16px;
  &:hover {
    background-color: #d92a25;
  }
  .coupert-icon {
    width: 58px;
  }
  .text{
    white-space: nowrap;
  }
}
</style>