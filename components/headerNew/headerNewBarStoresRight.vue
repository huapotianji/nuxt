<template>
  <div class="stores-right">
    <!-- subCategory -->
    <div
      class="category-title"
      v-if="chooseData.subCategory && chooseData.subCategory.list.length"
    >
      <h2>{{ chooseData.subCategory.title }}</h2>
      <a class="right-icon" :href="chooseData.subCategory.linkUrl">
        <span>{{ headData.nav_content.view_more }}</span>
        <img src="@/assets/img/icon-right-gray.svg" alt="" />
      </a>
    </div>
    <div
      class="category-body"
      v-if="chooseData.subCategory && chooseData.subCategory.list.length"
    >
      <a
        class="site-item"
        v-for="(item, index) in chooseData.subCategory.list"
        :key="index"
        :href="item.linkUrl"
        @click="headCategoryTrack(item, index)"
      >
        <div v-if="item" class="logo-wrap">
          <div
            class="logo"
            :style="{ backgroundImage: 'url(' + item.image + ')' }"
          ></div>
        </div>
        <div v-if="item" class="name">
          {{ item.childNavName }}
        </div>
      </a>
    </div>
    <!-- recommendStore -->
    <div
      class="category-title recommend-title"
      v-if="chooseData.recommendStore && chooseData.recommendStore.list.length"
    >
      <h2>{{ chooseData.recommendStore.title }}</h2>
    </div>
    <div
      class="recommend-body"
      v-if="chooseData.recommendStore && chooseData.recommendStore.list.length"
    >
      <div
        class="store-item"
        v-for="(item, index) in chooseData.recommendStore.list"
        :key="index"
        @click="headStoreTrack(item, index)"
      >
        <a
          v-if="item"
          class="store-logo"
          :href="item.RequestPath"
          :title="item.Name"
        >
          <nuxt-img
            v-if="item.Image"
            :src="item.Image"
            :alt="item.Name"
            loading="lazy"
          />
          <div v-else>
            {{ item.Name }}
          </div>
        </a>
        <div v-if="item" class="store-right">
          <div class="name" @click.prevent="goWebSite(item)">
            <div :title="item.Name">{{ item.Name }}</div>
            <img src="@/assets/img/icon-outbound.svg" :alt="item.Name" />
          </div>
          <a :href="item.RequestPath" class="coupon" v-if="item.code_num">
            <img src="@/assets/img/icon-coupon.svg" :alt="item.Name" />
            <div>{{ item.code_num + ' Coupons' }}</div>
          </a>
          <a :href="item.RequestPath" class="deal" v-if="item.deal_num">
            <img src="@/assets/img/icon-deals.svg" :alt="item.Name" />
            <div>{{ item.deal_num + ' Deals' }}</div>
          </a>
        </div>
      </div>
    </div>
    <!-- recommendDeal -->
    <div
      class="category-title recommend-title recommendDeal-title"
      v-if="chooseData.recommendDeal && chooseData.recommendDeal.list.length"
    >
      <h2>{{ chooseData.recommendDeal.title }}</h2>
      <a class="right-icon" :href="chooseData.recommendDeal.linkUrl">
        <span>{{ headData.nav_content.view_more }}</span>
        <img src="@/assets/img/icon-right-gray.svg" alt="" />
      </a>
    </div>
    <div
      class="recommendDeal-body"
      v-if="chooseData.recommendDeal && chooseData.recommendDeal.list.length"
    >
      <div
        class="search-deals-item"
        v-for="(item, index) in chooseData.recommendDeal.list"
        :key="index"
        @click="headDealTrack(item, index)"
      >
        <!-- logo -->
        <a class="logo" :href="item.RequestPath" rel="nofollow" target="_blank">
          <img :src="item.hd_img" :alt="item.Title" />
        </a>
        <!-- price -->
        <div class="footer-num">
          <span
            class="number"
            v-if="item.Current_price && item.Current_price != 0"
            >{{ item.CoinType + item.Current_price }}</span
          >
          <span
            class="num"
            v-if="item.Original_price && item.Original_price != 0"
            >{{ item.CoinType + item.Original_price }}</span
          >
        </div>
        <!-- name -->
        <div class="right-Component">
          <a
            class="name"
            v-if="item.TermName && item.termLink"
            target="_blank"
            :href="item.termLink"
            ><span :title="item.TermName + ' Deals'">{{
              item.TermName + ' Deals'
            }}</span></a
          >
          <div class="name" v-else-if="item.TermName && !item.termLink">
            <span :title="item.TermName + ' Deals'">{{
              item.TermName + ' Deals'
            }}</span>
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
    },
    chooseData: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {}
  },
  methods:{
    goWebSite(item){
       let outHost = window.location.origin
      window.open(
        `${outHost}${item.out_link}&page_value=${encodeURIComponent(
          window.location.pathname
        )}&refer=${encodeURIComponent(
          window.location.origin
        )}${encodeURIComponent(window.location.pathname)}`,
        '_blank'
      )
    },
    headCategoryTrack(item, index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/${this.chooseData.childNavName}/${item.childNavName},businessType:NonProCon`)
    },
    headStoreTrack(item, index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/${this.chooseData.childNavName}/${item.Name},businessType:NonProCon`)
    },
    headDealTrack(item, index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/${this.chooseData.childNavName}/deals-${index + 1},businessType:deals`)
    }
  }
}
</script>
<style lang='scss' scoped>
.stores-right {
  padding: 20px;
  .category-title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    h2 {
      font-size: 16px;
      font-weight: 600;
      color: #222222;
      line-height: 25px;
      cursor: auto;
    }
    .right-icon {
      display: flex;
      justify-content: space-between;
      align-items: center;
      span {
        font-size: 12px;
        font-weight: 400;
        color: #666666;
        line-height: 18px;
        margin-right: 4px;
      }
      img {
        width: 14px;
        height: 14px;
      }
    }
  }
  .category-body {
    .site-item {
      width: 90px;
      height: 100%;
      box-sizing: border-box;
      position: relative;
      vertical-align: top;
      display: inline-block;
      padding: 12px 0 10px 0px;
      text-align: center;
      cursor: pointer;
      &:hover {
        .logo-wrap {
          border: 2px solid #f04f4b;
        }
        .logo {
          &::before {
            content: '';
            position: absolute;
            background-color: #f04f4b;
            opacity: 0.2;
            width: 100%;
            height: 100%;
            left: 0;
            border-radius: 50%;
          }
          &::after {
            content: '';
            position: absolute;
            background-image: url('@/static/img/icon_arrow_red.png');
            background-position: center;
            background-repeat: no-repeat;
            background-size: contain;
            width: 20px;
            height: 20px;
            right: 0px;
            bottom: 0px;
          }
        }
        .name {
          color: #f04f4b;
        }
      }
      .logo-wrap {
        width: 72px;
        height: 72px;
        border-radius: 50%;
        margin: 0 auto;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 8px;
      }
      .logo {
        width: 64px;
        height: 64px;
        border-radius: 50%;
        background-position: center;
        background-size: cover;
        background-repeat: no-repeat;
        margin: 0 auto;
        position: relative;
      }
      .name {
        width: 91px;
        margin: 0 auto;
        text-align: center;
        font-size: 14px;
        font-weight: 500;
        color: #222222;
        line-height: 20px;
        white-space: normal;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        overflow: hidden;
        padding: 0 4px;
      }
    }
  }
  .recommend-title {
    margin-top: 20px;
    margin-bottom: 4px;
  }
  .recommend-body {
    display: flex;
    flex-wrap: wrap;
    .store-item {
      width: calc(100% / 2);
      padding: 10px;
      display: flex;
      justify-content: space-between;
      height: 120px;
      cursor: pointer;
      &:hover {
        background: #ffffff;
        box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
      }
      .store-logo {
        display: block;
        width: 100px;
        height: 100px;
        background: #ffffff;
        border-radius: 12px;
        border: 1px solid #eaeaea;
        img {
          width: 100%;
          height: 100%;
          object-fit: contain;
          padding: 12px;
        }
        div {
          color: #333;
          font-size: 14px;
          white-space: normal;
          word-break: break-word;
          word-wrap: break-word;
          font-weight: 600;
          display: flex;
          justify-content: center;
          align-items: center;
          text-align: center;
          height: 100%;
          overflow: hidden;
        }
      }
      .store-right {
        width: calc(100% - 100px);
        padding-left: 16px;
        .name {
          display: flex;
          margin: 7px 0;
          div {
            display: inline-block;
            font-size: 16px;
            font-weight: 500;
            color: #333333;
            margin-right: 6px;
            text-overflow: ellipsis;
            overflow: hidden;
            white-space: nowrap;
          }
          img {
            vertical-align: middle;
          }
        }
        .coupon,
        .deal {
          margin: 7px 0;
          display: flex;
          // height: 21px;
          img {
            margin-right: 4px;
          }
        }
        .coupon {
          div {
            font-size: 14px;
            font-weight: 400;
            color: #f04f4b;
          }
        }
        .deal {
          div {
            font-size: 14px;
            font-weight: 400;
            color: #0089ff;
          }
        }
      }
    }
  }
  .recommendDeal-body {
    border-radius: 10px;
    text-align: center;
    display: flex;
    flex-wrap: wrap;
    .search-deals-item {
      padding: 10px;
      &:hover {
        box-shadow: 0px 2px 16px 0px rgba(0, 0, 0, 0.1);
        border-radius: 12px;
      }
    }
    .logo {
      display: block;
      padding: 22px;
      width: 150px;
      height: 150px;
      background: #f6f6f6;
      border-radius: 8px;
      img {
        object-fit: contain;
        width: 100%;
        height: 100%;
      }
    }
    .footer-num {
      text-align: left;
      white-space: nowrap;
      overflow: hidden;
      padding-top: 8px;
      .number {
        font-size: 18px;
        color: #222;
        font-weight: 700;
        margin-right: 4px;
      }
      .num {
        font-size: 18px;
        color: #999;
        text-decoration: line-through;
      }
    }
    .name {
      color: #666;
      font-size: 12px;
      text-align: left;
      display: block;
      span {
        display: -webkit-box;
        -webkit-line-clamp: 1;
        -webkit-box-orient: vertical;
        overflow: hidden;
        width: 150px;
      }
    }
  }
}
@media screen and (max-width: 1200px) {
  .stores-right {
    .recommend-body {
      .store-item {
        width: calc(100% / 2);
      }
    }
  }
}
</style>