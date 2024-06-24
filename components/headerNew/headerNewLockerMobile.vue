<template>
  <div class="drawer" :style="{ left: isShow ? 0 : '-100%' }">
    <div class="content">
      <!-- header -->
      <div class="header">
        <div class="left" @click="getBack">
          <img src="@/static/img/icon_arrow_right_grey.png" alt="" />
        </div>
        <div class="search">
          <div class="search-btn-left">
            <img
              class="iconpark-icon"
              src="~/assets/img/icon-search.svg"
              alt=""
            />
          </div>
          <input
            type="text"
            @focus="getFocus"
            @input="fetchData"
            v-model="searchValue"
            id="hdSearch"
            autocomplete="off"
          />
        </div>
        <div class="btn" @click="searchBtn">Search</div>
      </div>
      <!--默认 显示的内容 -->
      <div v-if="isShowHotKey">
        <header-new-hotkey :headData="headData" @search="search" />
      </div>
      <!-- 搜索时的内容 -->
      <!-- 搜索时，显示的商家 -->
      <div
        class="search-list"
        v-if="
          list.length && searchValue && searchValue.length > 0 && isShowList
        "
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
    },
    isShow: {
      type: Boolean,
      default() {
        return false
      }
    }
  },
  data() {
    return {
      list: [],
      isShowHotKey: true,
      searchValue: '',
      isShowList: true,
      timer: null
    }
  },
  methods: {
    search(item) {
      this.$emit('search', item)
    },
    searchBtn() {
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
        localStorage.setItem('searchValueList', JSON.stringify(searchValueList))
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
    getFocus() {
      if (!this.searchValue) {
        this.isShowHotKey = true
        this.isShowList = false
      } else {
        this.isShowHotKey = false
        this.isShowList = true
      }
    },
    getBack() {
      this.$emit('update:isShow', false)
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
.drawer {
  display: block;
  position: relative;
  background-color: #fff;
  font-size: 16px;
  overflow: hidden;
  text-align: left;
  -webkit-transition: left 0.3s;
  transition: left 0.3s;
  width: 100%;
  position: fixed;
  bottom: 0;
  top: 0;
  z-index: 1000;
  overflow-y: scroll;
}
.content {
  width: 92%;
  margin: 0 auto;
}
.header {
  width: 100%;
  background: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 999;
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  height: 60px;
  .left {
    margin-right: 8px;
    height: 100%;
    display: flex;
    align-items: center;
    padding-left: 4px;
    img {
      transform: rotate(180deg);
    }
  }
  .search {
    flex-grow: 1;
    display: flex;
    justify-content: space-between;
    height: 34px;
    background: #f6f6f6;
    border-radius: 4px;
    border: 1px solid #eaeaea;
    input {
      outline: none;
      padding: 0.7em;
      border: none;
      font-size: 0.9375rem;
      color: #222;
      background: #f6f6f6;
      width: 100%;
      border-radius: 4px;
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
  }
  .btn {
    height: 100%;
    line-height: 60px;
    white-space: nowrap;
    font-size: 14px;
    font-weight: 400;
    color: #222222;
    margin-left: 16px;
    cursor: pointer;
  }
}
.search-list {
  height: auto;
  position: absolute;
  width: 92%;
  background: #fff;
  z-index: 10;
  max-width: 750px;

  .search-keyword {
    height: 48px;
    line-height: 48px;
    border-bottom: 1px solid #eaeaea;
    cursor: pointer;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    text-align: left;
    font-size: 14px;

    .name {
      color: #222;
    }
    .link {
      color: #aaa;
      margin-left: 15px;
    }
  }
}
</style>