<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
    <scroll class="content">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view/>
      <tab-control class="tab-control" :titles="['流行','新款','精选']" @tabClick="tabClick"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
  </div>
</template>

<script>
  import NavBar from "../../components/common/navbar/NavBar";
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";

  import TabControl from "../../components/content/tabControl/TabControl";
  import GoodsList from "../../components/content/goods/GoodsList";
  import Scroll from "../../components/common/scroll/Stroll";

  import {getHomeMultidata, getHomeGoods} from "../../network/home"

    export default {
        name: "Home",
        components: {
            RecommendView,
            NavBar,
            HomeSwiper,
            FeatureView,
            TabControl,
            GoodsList,
            Scroll
        },
        data() {
            return {
                banners: [],
                recommends: [],
                goods: {
                    'pop': {page: 0, list: []},
                    'new': {page: 0, list: []},
                    'sell': {page: 0, list: []},
                },
                currentType: 'pop'
            }
        },
        computed: {
            showGoods() {
                return this.goods[this.currentType].list;
            }
        },

        created() {
            this.getHomeMultidataList();
            this.getHomeGoodsList('pop');
            this.getHomeGoodsList('new');
            this.getHomeGoodsList('sell');
        },
        methods: {

            tabClick(index) {
                switch (index) {
                    case 0:
                        this.currentType = 'pop'
                        break
                    case 1:
                        this.currentType = 'new'
                        break
                    case 2:
                        this.currentType = 'sell'
                        break
                }
            },

            getHomeMultidataList() {
                getHomeMultidata().then((res) => {
                    this.banners = res.data.banner.list;
                    this.recommends = res.data.recommend.list;
                });
            },

            getHomeGoodsList(type) {
                const page = this.goods[type].page + 1;
                getHomeGoods(type,page).then((res) => {
                    console.log(res)
                    this.goods[type].list.push(...res.data.list);
                    this.goods[type].page += 1;
                })

            }
        }
    }
</script>

<style scoped>
  /*#home {*/
  /*  padding-top: 44px;*/
  /*  height: 100vh;*/
  /*  position: relative;*/
  /*}*/

  /*.home-nav {*/
  /*  background-color: var(--color-tint);*/
  /*  color: #fff;*/

  /*  position: fixed;*/
  /*  left: 0;*/
  /*  right: 0;*/
  /*  top: 0;*/
  /*  z-index: 9;*/
  /*}*/


  /*.tab-control {*/
  /*  position: sticky;*/
  /*  top: 44px;*/
  /*  z-index: 9;*/
  /*}*/

  /*.content {*/
  /*  overflow: hidden;*/

  /*  position: absolute;*/
  /*  top: 44px;*/
  /*  bottom: 49px;*/
  /*  left: 0;*/
  /*  right: 0;*/
  /*}*/



  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /*在使用浏览器原生滚动时, 为了让导航不跟随一起滚动*/
    /*position: fixed;*/
    /*left: 0;*/
    /*right: 0;*/
    /*top: 0;*/
    /*z-index: 9;*/
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }

  /*.content {*/
  /*height: calc(100% - 93px);*/
  /*overflow: hidden;*/
  /*margin-top: 44px;*/
  /*}*/

</style>
