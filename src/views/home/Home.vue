<template>
  <div class="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"></home-swiper>
  </div>
</template>

<script>
  import {getHomeMultidata} from "network/home";

  import NavBar from "components/common/navbar/NavBar";
  import HomeSwiper from "./chilcomps/HomeSwiper";
  export default {
    name: "Home",
    data(){
      return{
        banners:[],
        recommends:[]
      }
    },
    components:{
      NavBar,
      HomeSwiper
    },
    methods:{
    },
    created() {
      getHomeMultidata().then(res =>{
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    }
  }
</script>

<style scoped>
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;
  }
</style>
