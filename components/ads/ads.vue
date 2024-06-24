<template>
  <div v-show="adsStyle.isShowAds === 'YES'">
    <div class="advertisement" v-if="adsData.advertisement && showAds">
      {{ adsData.advertisement }}
    </div>
    <div
      class="afc-wrapper"
      :class="[classId, showAds ? '' : 'hidden-ads']"
      :style="{ height: adsStyle.height }"
    >
      <template v-if="adsStyle.isShowAds === 'YES'">
        <Adsense
          :data-ad-client="adsData.capubId"
          :ins-style="`display:block;width:100%;height: ${adsStyle.height};`"
          :data-ad-slot="adsData.adStot"
        >
        </Adsense>
      </template>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    classId: {
      type: String,
      default() {
        return ''
      }
    },
    adsData: {
      type: Object,
      default() {
        return {
          capubId: 'ca-pub-8170686022786770',
          adStot: '3804256845'
        }
      }
    },
    adsStyle: {
      type: Object,
      default() {
        return {}
      }
    },
    extraData: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      showAds: true
    }
  },
  mounted() {
    if (process.client) {
      this.$nextTick(() => {
        this.needHiddenAfc()
      })
    }
  },
  methods: {
    needHiddenAfc() {
      const targetNode = document.querySelector(
        `.${this.classId} ins.adsbygoogle`
      )
      if (!targetNode) {
        return
      }
      const config = { attributes: true, childList: true, subtree: true }
      const callback = (mutationList, observer) => {
        for (const mutation of mutationList) {
          if (
            mutation.attributeName === 'data-ad-status' &&
            mutation.target.dataset.adStatus === 'unfilled'
          ) {
            this.showAds = false
            this.$apiGaTrack(
              `eventAction:click,device:${navigator?.userAgent?.replace(/,/g, '?')},refer:${document.referrer},country:${this.extraData.countryCode},dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.extraData.page_type}/${this.classId}/unfilled,businessType:AFC`
            )
          }
        }
      }
      const observer = new MutationObserver(callback)
      observer.observe(targetNode, config)
    }
    // 检测客户端是否安装广告屏蔽插件
    // detectAdBlock() {
    //   let importFAB = document.createElement('script')
    //   importFAB.onload = () => {
    //     // alert("无ads屏蔽插件");
    //     this.hasAdBlock = false
    //   }
    //   importFAB.onerror = () => {
    //     // alert("有ads屏蔽插件");
    //     this.hasAdBlock = true
    //   }
    //   importFAB.defer = 1
    //   importFAB.crossOrigin = 'anonymous'
    //   importFAB.src =
    //     'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js'
    //   importFAB.id = 'used for detect AdBlock'
    //   document.body.appendChild(importFAB)
    // }
  }
}
</script>

<style lang="scss" scoped>
.afc-wrapper {
  width: 100%;
  margin: 0 auto 16px;
  overflow: hidden;
  box-sizing: border-box;
  background: #fafafa;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.04);
  border-radius: 12px;
  border: 1px solid #eaeaea;
  position: relative;
}
.advertisement {
  display: inline-block;
  color: #222;
  height: 22px;
  z-index: 1;
  font-size: 12px;
  font-weight: 500;
  line-height: 20px;
  text-align: center;
  text-align: center;
  margin-bottom: 2px;
  background: #eaeaea;
  border-radius: 8px;
  padding: 0px 6px;
  line-height: 22px;
}
.hidden-ads {
  height: 0 !important;
  overflow: hidden;
  transition: 0.5s all;
  border: 0;
  margin: 0;
  padding: 0;
}
</style>
