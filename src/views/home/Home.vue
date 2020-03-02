<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners" />
    <home-recommend :recommends="recommends" />
    <home-feature />
    <tab-control :titles="['流行', '新款', '精选']" class="tab-control"/>
    <goods-list :goodsList="goods['pop'].list"/>
  </div>
</template>

<script>
  import HomeSwiper from "./HomeSwiper";
  import HomeRecommend from "./HomeRecommend";
  import HomeFeature from "./HomeFeature";

  import NavBar from "components/common/navbar/NavBar";
  import TabControl from "components/content/tabControl/TabControl";
  import GoodsList from "components/content/goods/GoodsList";

  import {getHomeMultiData, getHomeGoodsData} from "network/home";

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      HomeRecommend,
      HomeFeature,
      NavBar,
      TabControl,
      GoodsList
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        }
      }
    },
    created() {
      this.getHomeMultiData();
      this.getHomeGoodsData('pop');
      this.getHomeGoodsData('new');
      this.getHomeGoodsData('sell')
    },
    methods: {
      getHomeMultiData() {
        getHomeMultiData().then(res => {
          console.log(res);
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoodsData(type) {
        const page = this.goods[type].page + 1;
        getHomeGoodsData(type, page).then(res => {
          console.log('请求成功啦');
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: white;
    position: fixed;
    z-index: 9;
    left: 0;
    right: 0;
    top: 0;
  }
  .tab-control {
    position: sticky;
    top: 44px;
  }
</style>
