<template>
  <div>
    <!-- 搜索历史记录 -->
    <div v-if="searchValueList && searchValueList.length">
      <h2 class="deals-title">Recent Search</h2>
     <div class="search-keyword">
        <a
          class="search-keyword-item"
          v-for="(item, index) in searchValueList"
          :key="index"
          :href="item.url"
        >
          <span class="content" v-html="item" @click="historyVal(item)"></span>
        </a>
      </div>
    </div>
    <!-- 热词 -->
    <div
      v-if="headData.hotSearch && headData.hotSearch.length && headData.nav_content.hot_search_title"
    >
      <h2 class="deals-title">{{ headData.nav_content.hot_search_title }}</h2>
      <div class="search-keyword">
        <a
          class="search-keyword-item"
          v-for="(item, index) in headData.hotSearch"
          :key="index"
          :href="item.url"
          @click="headWordTrack(item, index)"
        >
          <span class="index" :class="{ index1: index < 3 }">{{
            index + 1
          }}</span>
          <span class="content" v-html="item.text"></span>
        </a>
      </div>
    </div>
    <!-- deals导流 -->
    <div v-if="headData.hotDeals && headData.hotDeals.length && headData.nav_content.hot_deal_title && !searchValueList.length">
      <h2 class="deals-title">{{ headData.nav_content.hot_deal_title }}</h2>
      <div class="search-deals">
        <div
          class="search-deals-item"
          v-for="(item, index) in headData.hotDeals"
          :key="index"
        >
          <a
            class="logo"
            :href="item.RequestPath"
            rel="nofollow"
            @click="imageGatrack(index)"
            target="_blank"
          >
            <img v-lazy="item.hd_img" :alt="item.Title" />
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
              @click="storeGatrack(item, index)"
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
  </div>
</template>
<script>
export default {
  props: {
    headData: {
      type: Object,
      default() {}
    }
  },
  data() {
    return {
      searchValueList:[]
    }
  },
  mounted(){
     this.searchValueList = JSON.parse(localStorage.getItem('searchValueList'))?.reverse() || []
  },
  methods: {
    imageGatrack(index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/search/deals_pic-${index + 1},businessType:deals`)
    },
    storeGatrack(index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/search/deals_store-${index + 1},businessType:deals`)
    },
    historyVal(item){
      this.$emit('search', item)
    },
    headWordTrack(item, index) {
      this.$apiGaTrack(`eventAction:click,device:${this.headData.isMobile ? 'mobile' : 'pc'},refer:${document.referrer},country:US,dealId:,pageUrl:${window.location.href},outLink:,gaText:/${this.headData.pageType}/search/hot-${index + 1},businessType:NonProCon`)
    },
  }
}
</script>
<style lang='scss' scoped>
.deals-title {
  font-size: 16px;
  font-weight: 600;
  color: #222222;
  width: 100%;
  padding-left: 10px;
}
.search-deals {
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
    margin: 8px 0 4px 0;
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
.search-keyword {
  padding: 10px 0 10px 10px;
  display: flex;
  flex-wrap: wrap;
}
.search-keyword-item {
  line-height: 1.2;
  padding: 5px 16px;
  height: auto;
  display: flex;
  background: #f6f6f6;
  border-radius: 15px;
  margin: 0 10px 10px 0;
  align-items: center;
  font-size: 14px;
  cursor: pointer;
  &:hover {
    background: #ececec;
  }
  .index {
    margin-right: 6px;
    color: #999999;
    font-style: italic;
  }
  .index1 {
    color: #f1514c;
  }
  .content {
    color: #222;
  }
}
</style>