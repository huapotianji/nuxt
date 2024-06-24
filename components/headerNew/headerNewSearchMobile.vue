<template>
  <div class="search-area">
    <!-- 搜索框 -->
    <div class="search">
      <div class="search-btn-left">
        <img class="iconpark-icon" src="~/assets/img/icon-search.svg" alt="" />
      </div>
      <input
        type="text"
        @focus="getFocus"
        :placeholder="placeholder"
        id="hdSearch"
        autocomplete="off"
      />
    </div>
    <!-- 获取焦点时的遮罩 -->
    <div>
      <header-new-locker-mobile @search="search" :isShow.sync="isShow" :headData="headData" />
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
  computed: {
    placeholder() {
     
    }
  },
  data() {
    return {
      isShow:false,
    }
  },
  methods:{
    getFocus(){
      this.isShow = !this.isShow
    },
    search(searchValue){
      console.log(searchValue,'searchValue')
      this.$emit('search',searchValue)
    }
  }
}
</script>
<style lang="scss" scoped>
.search-area {
  position: relative;
  .search {
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
      color: #999;
      background: #f6f6f6;
      width: 100%;
      border-radius: 4px;
    }
    ::-webkit-input-placeholder {
      font-size: 14px;
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
}
</style>