<template>
  <div id="Home">
    <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
  </div>
</template>

<script>
  import NavBar from "../../components/common/navbar/NavBar";
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";
  import {getHomeMultidata} from "../../network/home"
    export default {
        name: "Home",
        components: {
            RecommendView,
            NavBar,
            HomeSwiper,
            FeatureView
        },
        data() {
            return {
                banners: [],
                recommends: [],
            }
        },
        created() {
            getHomeMultidata().then((res) => {
                this.banners = res.data.banner.list;
                console.log(this.banners)
                this.recommends = res.data.recommend.list;
            })
        }
    }
</script>

<style scoped>
  .home-nav {
    background-color: var(--color-tint);
    color: white;
  }

</style>
