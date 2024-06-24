<template>
  <div class="select" @mouseenter="isShowSelect = true" @mouseleave="isShowSelect = false">
    <div class="select-box">
      <img
        class="coutry-icon"
        :src="chooseData.coutryIcon"
        :alt="chooseData.alt"
        :title="chooseData.title"
      />
      <span class="coutry-name">{{ chooseData.lang }}</span>
      <span class="coutry-right"></span>
      <div class="select-list" v-if="isShowSelect">
        <a
          :href="item.coutryLink"
          class="coutry-box"
          v-for="(item, index) in coutryList"
          :key="index"
          @click="chooseCoutry(item)"
        >
          <img
            v-lazy="item.coutryIcon"
            :alt="item.alt"
            :title="item.title"
          />&nbsp;<span class="coutry-name">{{ item.coutryName }}</span>
        </a>
      </div>
    </div>
  </div>
</template>
<script>
const url = 'https://www.hotdeals.com/'
export default {
  name: 'chooseCountry',
  data() {
    return {
      isShowSelect: false,
      chooseData: {
        coutryIcon: require('~/assets/img/flagus.png'),
        coutryLink: `${url}`,
        coutryName: 'United States',
        coutryId: 1,
        alt: 'HotDeals US',
        title: 'HotDeals US',
        lang: 'US'
      },
      coutryList: [
        {
          coutryIcon: require('~/assets/img/flagus.png'),
          coutryLink: `${url}`,
          coutryName: 'United States',
          coutryId: 1,
          alt: 'HotDeals US',
          title: 'HotDeals US'
        },
        {
          coutryIcon: require('~/assets/img/flagde.png'),
          coutryLink: `https://de.hotdeals.com`,
          coutryName: 'Deutschland',
          coutryId: 6,
          alt: 'HotDeals Deutschland',
          title: 'HotDeals Deutschland'
        },
        {
          coutryIcon: require('~/assets/img/flagfr.png'),
          coutryLink: `https://fr.hotdeals.com/`,
          coutryName: 'France',
          coutryId: 5,
          alt: 'HotDeals France',
          title: 'HotDeals France'
        },
        {
          coutryIcon: require('~/assets/img/flaguk.png'),
          coutryLink: 'https://uk.hotdeals.com/',
          coutryName: 'United Kingdom',
          coutryId: 2,
          alt: 'HotDeals United Kingdom',
          title: 'HotDeals United Kingdom'
        },
        {
          coutryIcon: require('~/assets/img/flagnl.png'),
          coutryLink: `https://nl.hotdeals.com`,
          coutryName: 'Nederland',
          coutryId: 10,
          alt: 'HotDeals Nederland',
          title: 'HotDeals Nederland'
        },
        {
          coutryIcon: require('~/assets/img/flagbr.png'),
          coutryLink: `https://br.hotdeals.com`,
          coutryName: 'Brasil',
          coutryId: 9,
          alt: 'HotDeals Brasil',
          title: 'HotDeals Brasil'
        },
        // it
        {
          coutryIcon: require('~/assets/img/flages.png'),
          coutryLink: `https://es.hotdeals.com`,
          coutryName: 'España',
          coutryId: 18,
          alt: 'HotDeals España',
          title: 'HotDeals España'
        },
        // ru
        {
          coutryIcon: require('~/assets/img/flagja1.png'),
          coutryLink: 'https://ja.hotdeals.com/',
          coutryName: '日本',
          coutryId: 7,
          alt: 'HotDeals 日本',
          title: 'HotDeals 日本'
        },
        // se
        {
          coutryIcon: require('~/assets/img/flagpl.png'),
          coutryLink: `https://pl.hotdeals.com`,
          coutryName: 'Polska',
          coutryId: 17,
          alt: 'HotDeals Polska',
          title: 'HotDeals Polska'
        },
        {
          coutryIcon: require('~/assets/img/flagau.png'),
          coutryLink: 'https://au.hotdeals.com/',
          coutryName: 'Australia',
          coutryId: 1,
          alt: 'HotDeals Australia',
          title: 'HotDeals Australia'
        },
        {
          coutryIcon: require('~/assets/img/flagin.png'),
          coutryLink: `https://in.hotdeals.com`,
          coutryName: 'India',
          coutryId: 2,
          alt: 'HotDeals India',
          title: 'HotDeals India'
        },
        {
          coutryIcon: require('~/assets/img/flagtw.png'),
          coutryLink: 'https://tw.hotdeals.com',
          coutryName: '台灣',
          coutryId: 4,
          alt: 'HotDeals 台灣',
          title: 'HotDeals 台灣'
        },
        {
          coutryIcon: require('~/assets/img/flagtr.png'),
          coutryLink: `https://tr.hotdeals.com`,
          coutryName: 'Türkiye',
          coutryId: 5,
          alt: 'HotDeals Türkiye',
          title: 'HotDeals Türkiye'
        },
        {
          coutryIcon: require('~/assets/img/flagro.png'),
          coutryLink: `https://ro.hotdeals.com`,
          coutryName: 'România',
          coutryId: 6,
          alt: 'HotDeals România',
          title: 'HotDeals RO'
        },
        {
          coutryIcon: require('~/assets/img/flagfi.png'),
          coutryLink: `https://fi.hotdeals.com`,
          coutryName: 'Suomi',
          coutryId: 7,
          alt: 'HotDeals Suomi',
          title: 'HotDeals Suomi'
        },
        {
          coutryIcon: require('~/assets/img/flagmx.png'),
          coutryLink: `https://mx.hotdeals.com`,
          coutryName: 'México',
          coutryId: 8,
          alt: 'HotDeals México',
          title: 'HotDeals México'
        },
        {
          coutryIcon: require('~/assets/img/flagdk.png'),
          coutryLink: `https://da.hotdeals.com/`,
          coutryName: 'Danmark',
          coutryId: 9,
          alt: 'HotDeals Danmark',
          title: 'HotDeals Danmark'
        },
        {
          coutryIcon: require('~/assets/img/flaghu.png'),
          coutryLink: `https://hu.hotdeals.com`,
          coutryName: 'Magyarország',
          coutryId: 10,
          alt: 'HotDeals Magyarország',
          title: 'HotDeals Magyarország'
        },
        {
          coutryIcon: require('~/assets/img/flagid.png'),
          coutryLink: `https://id.hotdeals.com`,
          coutryName: 'Indonesia',
          coutryId: 11,
          alt: 'HotDeals Indonesia',
          title: 'HotDeals Indonesia'
        },
        {
          coutryIcon: require('~/assets/img/flagko.png'),
          coutryLink: `https://kr.hotdeals.com/`,
          coutryName: '대한민국',
          coutryId: 12,
          alt: 'HotDeals 대한민국',
          title: 'HotDeals 대한민국'
        },
        {
          coutryIcon: require('~/assets/img/flagch.png'),
          coutryLink: 'https://ch.hotdeals.com/',
          coutryName: 'Schweiz',
          coutryId: 13,
          alt: 'HotDeals Schweiz',
          title: 'HotDeals Schweiz'
        },
        {
          coutryIcon: require('~/assets/img/flagat.png'),
          coutryLink: 'https://at.hotdeals.com/',
          coutryName: 'Österreich',
          coutryId: 14,
          alt: 'HotDeals Österreich',
          title: 'HotDeals Österreich'
        }
      ]
    }
  },
  mounted() {
    if (location.origin == 'https://ja.hotdeals.com') {
      this.coutryList = this.coutryList.filter(item => {
        if (item.coutryLink != 'https://jp.hotdeals.com/') {
          return item
        }
      })
    }
  },
  methods: {
    chooseCoutry(item) {
      this.isShowSelect = false
      this.chooseData.coutryIcon = item.coutryIcon
      this.chooseData.coutryName = item.coutryName
      this.chooseData.coutryId = item.coutryId
      this.chooseData.alt = item.alt
      this.chooseData.title = item.title
      window.location.href = item.coutryLink
    }
  }
}
</script>
<style lang="scss" scoped>
.select {
  width: 77px;
  height: 100%;
  background: #ffff;

  display: flex;
  align-items: center;

  .select-box {
    display: flex;
    padding: 10px;
    align-items: center;
    color: #999;
    cursor: pointer;
    position: relative;
    border-radius: 12px;
    &:hover {
      background: #f1f1f1;
      .coutry-right {
        transform: rotate(180deg);
        margin: -5px 0px 0px 4px;
      }
    }
    .coutry-icon {
      width: 24px;
      height: 24px;
    }
    .coutry-name {
      margin-left: 6px;
      font-size: 14px;
      font-weight: 400;
      color: #222222;
    }
    .coutry-right {
      border: 5px solid transparent;
      border-top-color: #000;
      margin: 5px 0 0 6px;
    }
    .select-list {
      width: 300px;
      background: #fff;
      min-height: 50px;
      position: absolute;
      display: flex;
      flex-wrap: wrap;
      border-radius: 4px;
      z-index: 99;
      top: 57px;
      right: 0px;
      padding: 10px;
      box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
      border: 1px solid #eaeaea;
      .coutry-box {
        display: flex;
        padding: 10px;
        align-items: center;
        color: #999;
        cursor: pointer;
        width: 50%;

        img {
          width: 24px;
          height: 24px;
          -o-object-fit: contain;
          object-fit: contain;
          border-radius: 50%;
        }

        .coutry-name {
          display: flex;
          align-items: center;
          font-size: 12px;
          color: #999;
        }

        &:hover {
          background: #f1f1f1;
          border-radius: 8px;
        }
      }
    }
  }
}

@media screen and (max-width: 812px) {
  .select-list {
    left: -123px !important;
  }
}

@media screen and (max-width: 720px) {
  .select {
    width: 90%;
    position: relative;

    .select-box {
      display: flex;
      justify-content: space-between;
      padding: 10px;
      align-items: center;
      color: #999;
      cursor: pointer;
      font-size: 14px;
      width: 270px;

      .coutry-icon {
        width: 30px;
        height: 30px;
      }

      .coutry-name {
        display: flex;
        align-items: center;
        font-size: 14px;
      }
    }
  }
}
</style>
