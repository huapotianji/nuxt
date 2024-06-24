<template>
  <header id="header" class="header">
    <!-- logo -->
    <a class="logo" :href="headData.homeUrl">
      <img
        src="/assets/img/logo.svg"
        width="214px"
        alt="mnf5"
      />
    </a>
    <!-- 导航 -->
    <div class="nav-bar">
      <header-new-bar :headData="headData" />
    </div>
    <!-- 搜索 -->
    <div class="search">
      <header-new-search @search="search" :headData="headData" />
    </div>
    <div class="search-mobile">
      <header-new-search-mobile @search="search" :headData="headData" />
    </div>

  </header>
</template>
<script>
export default {
  props: {
    headData: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {};
  },
  methods: {
    mobileNavClick() {},
    // 搜索
    async search(searchValue) {},
  },
};
</script>
<style lang='scss' scoped>
.header {
  width: 100%;
  height: 72px;
  background: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px 0 24px;
  border-bottom: 1px solid #eaeaea;
  z-index: 999;
  position: sticky;
  top: 0;
  box-sizing: border-box;
}
.logo {
  margin-right: 30px;
  img {
    object-fit: contain;
  }
}
.nav-bar {
  height: 100%;
}
.search {
  flex-grow: 1;
  width: auto;
}
.search-mobile {
  display: none;
}
.coupert {
  margin-left: 20px;
}
.lang {
  margin-left: 20px;
  height: 100%;
}
.mobile-nav {
  display: none;
  width: 20px;
  height: 20px;
  margin-left: 12px;
  background-image: url(@/static/img/icon_menu_mobile.png);
}
.mobile-nav-x {
  display: none;
  width: 20px;
  height: 20px;
  margin-left: 12px;
  background-image: url(@/static/img/icon_close.png);
}
@media screen and (max-width: 950px) {
  .logo {
    margin-right: 20px;
  }
  .nav-bar {
    display: none;
  }
  .search {
    display: none;
  }
  .search-mobile {
    display: block;
    flex-grow: 1;
  }
  .coupert {
    display: none;
  }
  .lang {
    display: none;
  }
  .mobile-nav {
    display: block;
  }
  .mobile-nav-x {
    display: block;
  }
  .mobile-nav-dislog {
    width: 100%;
    height: calc(100vh - 60px);
    background: #fff;
    position: absolute;
    top: 59px;
    left: 0;
    overflow-y: scroll;
  }
  .mobile-activty-dialog {
    height: calc(100vh - 99px);
  }
}
@media screen and (max-width: 550px) {
  .logo {
    margin-right: 10px;
    height: 36px;
  }
  .header {
    padding: 0 14px;
    height: 60px;
  }
}
</style>