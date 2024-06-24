<template >
  <div class="nav" v-if="headData.navList">
    <div
      class="item"
      v-for="(item, index) in headData.navList"
      :key="index"
      @click="getNav(item, index)"
      @mouseenter="mouseenterItem(item)"
      @mouseleave="mouseleaveItem(item)"
    >
      <!-- 一级菜单 -->
      <a
        :class="item.navName"
        class="item-name"
        v-if="item.linkUrl"
        :href="item.linkUrl"
      >
        <div>{{ item.navName }}</div>
        <div
          class="down-arrow"
          v-if="item.children && item.children.length"
        ></div>
        <div class="icon" v-if="item.navName == 'Back-to-School'">Hot</div>
      </a>
      <div class="item-name" v-else :href="item.linkUrl">
        <div>{{ item.navName }}</div>
        <div
          class="down-arrow"
          v-if="item.children && item.children.length"
        ></div>
      </div>
      <!-- 二级菜单 除了stores -->
      <div
        class="item-list"
        v-show="
          item.children &&
          item.children.length &&
          item.showChild &&
          item.navName != 'Stores'
        "
      >
        <a
          class="it"
          :href="it.linkUrl"
          v-for="(it, cindex) in item.children"
          :key="cindex"
          @click.stop="headNavTrack(item, it, cindex)"
        >
          <span>{{ it.chilNavName || it.childNavName }}</span>
          <div></div>
        </a>
      </div>
      <!-- 二级菜单 stores -->
      <div
        class="item-list-stores"
        :class="{
          'item-list-stores-old':
            !chooseData.subCategory &&
            chooseData.childNavName != 'All Categories'
        }"
        v-show="
          item.children &&
          item.children.length &&
          item.showChild &&
          item.navName == 'Stores'
        "
      >
        <!-- left -->
        <div class="left">
          <a
            class="it"
            :href="it.linkUrl"
            v-for="(it, cindex) in item.children"
            :key="cindex"
            :class="{ showChildIt: it.childNavName == chooseData.childNavName }"
            @mouseenter="mouseenterIt(it)"
            @mouseleave="mouseleaveIt(it)"
            @click.stop="headNavTrack(item, it, cindex)"
          >
            <span>{{ it.chilNavName || it.childNavName }}</span>
            <div></div>
          </a>
        </div>
        <div
          class="right"
          @mouseenter="mouseenterRight"
          @mouseleave="mouseleaveRight"
          v-if="
            chooseData.childNavName != 'All Categories' &&
            chooseData.showChild &&
            chooseData.subCategory
          "
        >
          <!-- 移入进来的数据 -->
          <!-- 1.subCategory -->
          <header-new-bar-stores-right
            :headData="headData"
            :chooseData="chooseData"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'navBar',
  props: {
    headData: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      chooseData: {}
    }
  },
  methods: {
    mouseenterItem(item) {
      item.showChild = true
      if (item.navName == 'Stores') {
        this.chooseData = item.children[0]
        item.children[0].showChild = true
      }
    },
    mouseleaveItem(item) {
      item.showChild = false
    },
    mouseenterIt(it) {
      it.showChild = true
      this.chooseData = it
    },
    mouseleaveIt(it) {
      // it.showChild = false
      // this.chooseData = it
    },
    mouseenterRight() {
      let item = this.chooseData || {}
      item.showChild = true
    },
    mouseleaveRight() {
      let item = this.chooseData || {}
      item.showChild = false
    },
    getNav(item) {
      if (item.navName == 'Coupons') {
        this.$analytics('/menu/coupons')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${
            this.headData.pageType
          }/menu/coupons-0,businessType:NonProCon`
        )
      }
      if (item.navName == 'Deals') {
        this.$analytics('/menu/deals')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${
            this.headData.pageType
          }/menu/deals,businessType:deals`
        )
      }
      if (item.navName == 'Product Offers') {
        this.$analytics('/menu/productoffer')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${
            this.headData.pageType
          }/menu/productoffer,businessType:deals`
        )
      }
      if (item.pageType == 'holiday_calendar') {
        this.$analytics(`/${this.headData.pageType}/menu/festival`)
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${this.headData.pageType}/menu/festival,businessType:NonProCon`
        )
      }
      if (item.navName == 'Stores') {
        this.$analytics('/menu/stores')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${
            this.headData.pageType
          }/menu/stores,businessType:NonProCon`
        )
      }
      if (item.navName == 'Travel') {
        this.$analytics('/menu/travel')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${
            this.headData.pageType
          }/menu/travel,businessType:NonProCon`
        )
      }
    },

    headNavTrack(item, subItem, index) {
      if (item.navName == 'Coupons') {
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${this.headData.pageType}/menu/coupons-${
            index + 1
          },businessType:NonProCon`
        )
      }
      if (item.navName == 'Deals') {
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${this.headData.pageType}/menu/${
            subItem.childNavName
          },businessType:deals`
        )
      }
      if (item.navName == 'Product Offers') {
        return
      }
      if (item.navName == 'Stores') {
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${this.headData.pageType}/menu/${
            subItem.childNavName
          },businessType:NonProCon`
        )
      }
      if (item.navName == 'Travel') {
        this.$analytics('/menu/travel')
        this.$apiGaTrack(
          `eventAction:click,device:${
            this.headData.isMobile ? 'mobile' : 'pc'
          },refer:${document.referrer},country:US,dealId:,pageUrl:${
            window.location.href
          },outLink:,gaText:/${this.headData.pageType}/menu/${
            subItem.childNavName
          },businessType:NonProCon`
        )
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.nav {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  cursor: pointer;
  position: relative;
  height: 100%;
  .item {
    margin-right: 24px;
    height: 100%;
    display: flex;
    align-items: center;
    .item-name {
      display: flex;
      color: #333;
      font-size: 16px;
      align-items: center;
      border-bottom: 2px solid #fff;
      div {
        white-space: nowrap;
      }
      .down-arrow {
        border: 5px solid transparent;
        border-top-color: #000;
        margin: 5px 0 0 4px;
      }
    }
    .Back-to-School {
      position: relative;

      .icon {
        content: 'Hot';
        color: #fff;
        font-size: 14px;
        width: 36px;
        height: 18px;
        line-height: 18px;
        border-radius: 6px;
        position: absolute;
        right: -22px;
        top: -15px;
        background-color: rgba(238, 103, 98, 1);
        text-align: center;
        font-weight: 600;
        animation: hot 1.8s infinite;
        &::before {
          content: '';
          width: 0;
          height: 0;
          position: absolute;
          left: 2px;
          bottom: -11px;
          border: 6px solid transparent;
          border-top-color: #ee6762;
        }
      }
      @keyframes hot {
        0% {
          transform: translateY(1px) scale(0.8);
        }
        50% {
          transform: translateY(-1px) scale(0.8);
        }
        100% {
          transform: translateY(1px) scale(0.8);
        }
      }
    }
    &:hover {
      .item-name {
        color: #f04f4b;
      }
      .down-arrow {
        border-top-color: #f04f4b;
        transform: rotate(180deg);
        margin: -5px 0px 0px 4px;
      }
    }
    .item-list {
      position: absolute;
      background: #fff;
      z-index: 100;
      box-shadow: 0px 10px 12px 0px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      border: 1px solid #eaeaea;
      top: 71px;
      z-index: 99;
      padding: 4px 10px 4px 10px;

      .it {
        width: 100%;
        display: block;
        padding: 9px 12px;
        color: #666;
        white-space: nowrap;
        font-size: 14px;
        border-radius: 8px;
        margin-bottom: 4px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        div {
          width: 16px;
          height: 16px;
          background-position: center;
          background-size: cover;
        }
        &:hover {
          background-color: #fff0ef;
          color: #f04f4a;
          div {
            background-image: url(@/assets/img/icon-right-red.svg);
          }
        }
      }
    }

    // stores
    .item-list-stores {
      display: flex;
      position: absolute;
      background: #fff;
      z-index: 100;
      box-shadow: 0px 10px 12px 0px rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      border: 1px solid #eaeaea;
      top: 71px;
      left: 0;
      z-index: 99;
      padding: 4px 10px 4px 10px;

      .it {
        width: 100%;
        display: block;
        padding: 9px 12px;
        color: #666;
        white-space: nowrap;
        font-size: 14px;
        border-radius: 8px;
        margin-bottom: 4px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        div {
          width: 16px;
          height: 16px;
          background-position: center;
          background-size: cover;
        }
        &:hover {
          background-color: #fff0ef;
          color: #f04f4a;
          div {
            background-image: url(@/assets/img/icon-right-red.svg);
          }
        }
      }
      .showChildIt {
        background-color: #fff0ef;
        color: #f04f4a;
        div {
          background-image: url(@/assets/img/icon-right-red.svg);
        }
      }

      .left {
        height: 590px;
        overflow: auto;
        padding-right: 10px;
        overscroll-behavior: none;
        &:hover {
          padding-right: 6px;
          &::-webkit-scrollbar {
            width: 4px;
          }
        }
        &::-webkit-scrollbar {
          width: 0;
        }
        /* 定义滚动条的轨道 */
        &::-webkit-scrollbar-track {
          background: #f1f1f1;
        }
        /* 定义滚动条滑块 */
        &::-webkit-scrollbar-thumb {
          background: #c4c2c2;
          border-radius: 2px;
        }

        /* 定义滚动条滑块悬停时的样式 */
        &::-webkit-scrollbar-thumb:hover {
          background: #8f8d8d;
        }
      }
      .right {
        width: 810px;
        height: 590px;
        overflow-y: auto;
        border-left: 1px solid #eaeaea;
        overscroll-behavior: none;
        padding-right: 4px;
        &:hover {
          padding-right: 0;
          &::-webkit-scrollbar {
            width: 4px;
          }
        }
        &::-webkit-scrollbar {
          width: 0;
        }
        /* 定义滚动条的轨道 */
        &::-webkit-scrollbar-track {
          background: #f1f1f1;
        }
        /* 定义滚动条滑块 */
        &::-webkit-scrollbar-thumb {
          background: #c4c2c2;
          border-radius: 2px;
        }

        /* 定义滚动条滑块悬停时的样式 */
        &::-webkit-scrollbar-thumb:hover {
          background: #8f8d8d;
        }
      }
    }
    .item-list-stores-old {
      left: auto;
    }
  }
}
@media screen and (max-width: 1200px) {
  .nav .item .item-list-stores .right {
    width: 600px;
  }
  .nav {
    .item {
      margin-right: 16px;
    }
  }
}
</style>
