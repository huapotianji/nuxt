<template>
  <div class="nav">
    <div class="nav-item" v-for="(item, index) in navListArr" :key="index">
      <!-- 一级菜单 -->
      <div class="item-name" :class="{ 'item-name-click': item.showChild }">
        <a
          class="nav-name"
          :class="{ 'no-child': item.children && !item.children.length }"
          :href="item.linkUrl"
          @click="getNav(item, index)"
          >{{ item.navName }}</a
        >
        <div
          class="right-content"
          @click="getItem(item, index)"
          v-if="item.children && item.children.length"
        >
          <div class="down-arrow"></div>
        </div>
      </div>
      <!-- 二级菜单 除了stores -->
      <div
        v-if="item.navName != 'Stores'"
        class="item-list-wrap"
        :class="'item-list-wrap-' + index"
      >
        <div class="item-list" :class="'item-list-' + index">
          <a
            class="it"
            :href="it.linkUrl"
            v-for="(it, cindex) in item.children"
            :key="cindex"
            @click.stop="headNavTrack(item, it, cindex)"
          >
            <span>{{ it.chilNavName || it.childNavName }}</span>
          </a>
        </div>
      </div>

      <!-- 二级菜单 stores -->
      <div
        v-if="item.navName == 'Stores'"
        class="item-list-wrap"
        :class="'item-list-wrap-' + index"
      >
        <div class="item-list" :class="'item-list-' + index">
          <div v-for="(it, cindex) in item.children" :key="cindex">
            <div class="it" :class="{ 'it-click': it.showChild }">
              <a :href="it.linkUrl" @click="headNavTrack(item, it, cindex)">{{ it.chilNavName || it.childNavName }}</a>
              <div
                class="right-content"
                v-if="it.subCategory && it.subCategory.list.length"
                @click="getIt(item, index, it, cindex)"
              >
                <div class="down-arrow"></div>
              </div>
            </div>

            <!-- 三级菜单 -->
            <div
              v-if="it.subCategory && it.subCategory.list.length"
              class="it-list-wrap"
              :class="'it-list-wrap-' + cindex"
            >
              <div class="item-list" :class="'it-list-' + cindex">
                <div
                  class="it st"
                  v-for="(st, sindex) in it.subCategory.list"
                  :key="sindex"
                >
                  <a :href="st.linkUrl">{{ st.childNavName }}</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    headData: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      navListArr: this.updateStatus(this.headData.navList) || []
    }
  },
  methods: {
    updateStatus(data) {
      return data?.map(item => {
        const newItem = { ...item, showChild: false }

        if (item.children) {
          newItem.children = this.updateStatus(item.children)
        }

        if (newItem.children && newItem.children.length === 0) {
          delete newItem.children
        }

        return newItem
      })
    },
    getItem(item, index) {
      item.showChild = !item.showChild
      let wrap = document.querySelector('.item-list-wrap-' + index)
      let ele = document.querySelector('.item-list-' + index)
      if (item.showChild) {
        wrap.style.height = ele.clientHeight + 'px'
      } else {
        wrap.style.height = 0 + 'px'
      }
    },
    getIt(item, index, it, cindex) {
      it.showChild = !it.showChild
      let wrapItem = document.querySelector('.item-list-wrap-' + index)
      let eleItem = document.querySelector('.item-list-' + index)
      let wrap = document.querySelector('.it-list-wrap-' + cindex)
      let ele = document.querySelector('.it-list-' + cindex)
      if (it.showChild) {
        wrap.style.height = ele.clientHeight + 'px'
        wrapItem.style.height = 'auto'
      } else {
        wrap.style.height = 0 + 'px'
      }
    },
    getNav(item) {
      if (item.navName == 'Coupons') {
        this.$analytics('/menu/coupons')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/coupons-0,businessType:NonProCon`)
      }
      if (item.navName == 'Deals') {
        this.$analytics('/menu/deals')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/deals,businessType:deals`)
      }
      if (item.navName == 'Product Offers') {
        this.$analytics('/menu/productoffer')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/productoffer,businessType:deals`)
      }
      if (item.pageType == 'holiday_calendar') {
        this.$analytics(`/${this.headData.pageType}/menu/festival`)
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/festival,businessType:NonProCon`)
      }
      if (item.navName == 'Stores') {
        this.$analytics('/menu/stores')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/stores,businessType:NonProCon`)
      }
      if (item.navName == 'Travel') {
        this.$analytics('/menu/travel')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/travel,businessType:NonProCon`)
      }
    },

    headNavTrack(item, subItem, index) {
      if (item.navName == 'Coupons') {
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/coupons-${index+1},businessType:NonProCon`)
      }
      if (item.navName == 'Deals') {
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/${subItem.childNavName},businessType:deals`)
      }
      if (item.navName == 'Product Offers') {
        return
      }
      if (item.navName == 'Stores') {
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/${subItem.childNavName},businessType:NonProCon`)
      }
      if (item.navName == 'Travel') {
        this.$analytics('/menu/travel')
        this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/menu/${subItem.childNavName},businessType:NonProCon`)
      }
    }
  }
}
</script>
<style lang='scss' scoped>
.nav {
  cursor: pointer;
  .nav-item {
    .item-name {
      display: flex;

      font-size: 16px;
      align-items: center;
      justify-content: space-between;
      border-bottom: 2px solid #fff;
      width: 92%;
      margin: 12px auto;
      .nav-name {
        color: #333;
        white-space: nowrap;
      }
      .no-child {
        width: 100%;
      }
      .right-content {
        flex-grow: 1;
        display: flex;
        justify-content: right;
        .down-arrow {
          border: 5px solid transparent;
          border-top-color: #000;
          margin: 5px 0 0 4px;
          width: 5px;
        }
      }
    }
    .item-name-click {
      .nav-name {
        color: #f04f4b;
      }
      .right-content {
        .down-arrow {
          border-top-color: #f04f4b;
          transform: rotate(180deg);
          margin: -5px 0px 0px 4px;
        }
      }
    }
    .item-list-wrap {
      height: 0px;
      overflow: hidden;
      transition: 0.5s all;
    }
    .item-list {
      background: #fafafa;
      padding: 1px 0;
      .it {
        padding: 0 16px 0 26px;
        margin: 20px auto;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .right-content {
          flex-grow: 1;
          display: flex;
          justify-content: right;
          .down-arrow {
            border: 5px solid transparent;
            border-top-color: #000;
            margin: 5px 0 0 4px;
            width: 5px;
          }
        }
      }
      .it-click {
        a {
          color: #f04f4b;
        }
        .right-content {
          .down-arrow {
            border-top-color: #f04f4b;
            transform: rotate(180deg);
            margin: -5px 0px 0px 4px;
          }
        }
      }
    }
    .it-list-wrap {
      height: 0px;
      overflow: hidden;
      transition: 0.5s all;
      padding-left: 12px;
      .st {
        margin-top: 0;
      }
    }
  }
}
</style>