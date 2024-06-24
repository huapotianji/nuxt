<template>
  <div class="search-area">
    <!-- 搜索框 -->
    <div class="search" :class="{ 'search-fou': isShowRightBtn }">
      <div class="search-btn-left" @click="$emit('search', searchValue)">
        <img class="iconpark-icon" src="~/assets/img/icon-search.svg" alt="" />
      </div>
      <input
        type="text"
        @focus="getFocus"
        @blur="getBluer"
        @input="fetchData"
        v-model="searchValue"
        id="hdSearch"
        autocomplete="off"
      />
      <div
        class="search-btn-right"
        :class="{ 'search-btn-right-fou': isShowRightBtn }"
        @click="searchRightBtn()"
      >
        <img
          v-if="isShowRightBtn"
          class="iconpark-icon"
          src="~/assets/img/icon-search-white.svg"
          alt=""
        />
      </div>
    </div>
    <!-- 搜索时，显示的商家 -->
    <div
      class="search-list"
      @mouseenter="moveListEnter"
      @mouseleave="moveListLeave"
      v-if="list && list.length && searchValue.length > 0 && isShowList"
    >
      <a
        class="search-keyword"
        v-for="(item, index) in list"
        :key="index"
        :data-href="item.DefaultUrl"
        :data-url="item.RequestPath"
        :href="item.RequestPath"
      >
        <span class="name">{{ item.Name }}</span>
        <span class="link">{{ item.DomainUrl }}</span>
      </a>
    </div>

    <!-- 获取焦点时，显示的热词 -->
    <div
      class="search-list-hot"
      @mouseenter="moveEnter"
      @mouseleave="moveLeave"
      v-if="
        isShowHotKey &&
        ((headData.hotSearch &&
          headData.hotSearch.length &&
          headData.nav_content.hot_search_title) ||
          (headData.hotDeals &&
            headData.hotDeals.length &&
            headData.nav_content.hot_deal_title))
      "
    >
      <header-new-hotkey :headData="headData" @search="search" />
    </div>
  </div>
</template>
<script>
export default {
  name: 'search',
  props: {
    searchData: {
      type: String,
      default() {
        return ''
      }
    },
    headData: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      isShowList: true,
      isShowHotKey: false,
      isBlur: false,
      isBlurList: false,
      isGetBlur: false,
      searchValue: '',
      list: [],
      timer: null,
      isShowRightBtn: false,
      placeholder: () => {
        if (this.page_type) {
          return this.headData.searchPlaceholder
        }
      }
    }
  },
  methods: {
    search(item) {
      this.$emit('search', item)
    },
    searchRightBtn() {
      // 将 searchValue存储起来
      let searchValueList =
        JSON.parse(localStorage.getItem('searchValueList')) || []
      if (this.searchValue) {
        searchValueList.push(this.searchValue)
      }
      // 去重
      searchValueList = Array.from(new Set(searchValueList))
      // 存储
      if (searchValueList.length <= 10) {
        localStorage.setItem(
          'searchValueList',
          JSON.stringify(searchValueList)
        )
      } else {
        let searchValueListNew = searchValueList.slice(
          searchValueList.length - 10,
          searchValueList.length
        )
        localStorage.setItem(
          'searchValueList',
          JSON.stringify(searchValueListNew)
        )
      }

      this.$emit('search', this.searchValue)
    },
    moveEnter() {
      this.isBlur = true
    },
    moveLeave() {
      this.isBlur = false
      if (this.isGetBlur) {
        this.isShowHotKey = false
      }
    },
    moveListEnter() {
      this.isBlurList = true
    },
    moveListLeave() {
      this.isBlurList = false
    },
    getFocus() {
      this.isGetBlur = false
      if (!this.searchValue) {
        this.isShowHotKey = true
        this.isShowRightBtn = true
      } else {
        this.isShowHotKey = false
        this.isShowList = true
      }
    },
    getBluer() {
      this.isGetBlur = true
      if (!this.isBlur && !this.searchValue) {
        this.isShowHotKey = false
        this.isShowRightBtn = false
      }
      if (!this.isBlurList) {
        this.isShowList = false
      }
    },
    async fetchData() {
    
    }
  },
  destroyed() {
    clearTimeout(this.timer)
    this.timer = null
  }
}
</script>
<style lang="scss" scoped>
.search-area {
  position: relative;
  .search {
    display: flex;
    justify-content: space-between;
    height: 40px;
    background: #f6f6f6;
    border-radius: 12px;
    border: 1px solid #eaeaea;
    &:hover {
      box-shadow: 0 0 0 2px rgba(234, 76, 137, 0.1);
      background: #fff;
      input {
        background: #fff;
      }
    }
    input {
      outline: none;
      padding: 0.7em;
      border: none;
      font-size: 0.9375rem;
      color: #222;
      background: #f6f6f6;
      width: 100%;
    }
    ::-webkit-input-placeholder {
      font-size: 14px;
      color: #999;
    }
    .search-btn-left {
      display: flex;
      align-items: center;
      margin-left: 12px;
      .iconpark-icon {
        width: 16px;
        height: 16px;
      }
    }

    .search-btn-right {
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      width: 40px;
      box-shadow: 0px 0px 0px 0px rgba(241, 81, 76, 0.28);
      border-radius: 0px 10px 10px 0px;
      .iconpark-icon {
        width: 18px;
        height: 18px;
      }
    }
    .search-btn-right-fou {
      background: #f1514c;
      transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
    }
  }
  .search-fou {
    box-shadow: 0 0 0 2px rgba(234, 76, 137, 0.1);
    border-radius: 10px;
    border: 1px solid #f1514c;
    background: #fff;
    input {
      background: #fff;
    }
  }
  .search-list {
    height: auto;
    position: absolute;
    width: 100%;
    background: #fff;
    z-index: 10;
    // max-width: 750px;
    box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    border: 1px solid #eaeaea;
    top: 56px;
    .search-keyword {
      height: 40px;
      line-height: 40px;
      padding-left: 18px;
      cursor: pointer;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      text-align: left;

      .name {
        color: #222;
        font-size: 14px;
        font-weight: 400;
      }
      .link {
        color: #999;
        margin-left: 15px;
        font-size: 14px;
      }
      &:hover {
        background: #ececec;
      }
    }
  }
  .search-list-hot {
    height: auto;
    position: absolute;
    width: 100%;
    background: #fff;
    z-index: 10;
    padding: 10px;
    padding-top: 20px;
    max-width: 1050px;
    top: 55px;
    background: #ffffff;
    box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    border: 1px solid #eaeaea;
  }
}
@media screen and (max-width: 820px) and (min-width: 768px) {
  .search-area {
    .search-list {
      position: absolute;
      top: 100%;
      width: 705px;
    }
  }
}
@media screen and (max-width: 767px) {
  .search-area {
    .search-list {
      position: absolute;
      top: 100%;
      width: 364px;
    }
  }
}
</style>