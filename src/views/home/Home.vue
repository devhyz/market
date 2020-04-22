<template>
  <div class="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control :titles="['流行','新款','精选']"
                 class="tab-control2"
                 @tabBbarItemClick ='tabBbarItemClick'
                 ref="tabControl2" v-show="false"></tab-control>
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="pullingUp">
      <home-swiper :banners="banners"
                   @imageLoad="imageLoad">

      </home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control :titles="['流行','新款','精选']"
                   class="tab-control"
                   @tabBbarItemClick ='tabBbarItemClick'
                   ref="tabControl1"></tab-control>
      <good-list :goods="showGoods"></good-list>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
  import {getHomeMultidata,getHomeGoods} from "network/home";

  import {debounce} from "common/utils";

  import NavBar from "components/common/navbar/NavBar";
  import TabControl from "components/content/tabcontrol/TabControl";

  import HomeSwiper from "./chilcomps/HomeSwiper";
  import RecommendView from "./chilcomps/RecommendView";
  import FeatureView from "./chilcomps/FeatureView";
  import GoodList from "components/content/goods/GoodList";
  import Scroll from "components/common/scroll/Scroll";
  import BackTop from "components/content/backTop/BackTop";
  export default {
    name: "Home",
    data(){
      return{
        banners:[],
        recommends:[],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType:'pop',
        isShowBackTop :false,
        tabOffsetTop: 0,
        isTabFixed: false,
        saveY: 0
      }
    },
    components:{
      NavBar,
      TabControl,
      HomeSwiper,
      RecommendView,
      FeatureView,
      GoodList,
      Scroll,
      BackTop
    },
    methods:{
      // 点击小按钮回到顶部
      backClick(){
        // console.log(this.$refs.scroll.scroll)
        this.$refs.scroll.scrollTo(0,0,500)
      },
      //显示或隐藏返回顶部按钮
      contentScroll(position){
        //显示或隐藏返回顶部按钮
        if (-position.y > 1000){
          this.isShowBackTop = true
        }else {
          this.isShowBackTop = false
        }
        //tabcontrol吸顶
        this.isTabFixed = (-position.y) > this.isShowBackTop

      },
      pullingUp(){
        // console.log('上拉加载更多')
        this.getHomeGoodsHome(this.currentType)
        this.$refs.scroll.finishPullUp()
      },
      //点击tabbarcontol切换数据
      tabBbarItemClick(index){
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
      //获得tabcontrol的offset值
      imageLoad(){

        this.tabOffsetTop = this.$refs.tabControl1.$el.offsetTop
        // console.log(this.tabOffsetTop)
      },
      //网络请求
      //swiper和recommend的数据
      getHomeMultidataHome(){
        getHomeMultidata().then(res =>{
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      //获得商品列表的数据
      getHomeGoodsHome(type){
        var page = this.goods[type].page + 1
        getHomeGoods(type,page).then(res =>{
          // console.log(res.data.list)
          this.goods[type].list.push(...res.data.list)
        })
        this.goods[type].page+=1
        // console.log(this.$refs.scroll)
        this.$refs.scroll && this.$refs.scroll.finishPullUp()
      }
    },
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      this.$refs.scroll.refresh()
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
    },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
    created() {
      //swiper和recommend的数据
      this.getHomeMultidataHome()
      //获得第一页商品列表的数据
      this.getHomeGoodsHome('pop')
      this.getHomeGoodsHome('new')
      this.getHomeGoodsHome('sell')
    },
    mounted() {
      const refresh =  debounce(this.$refs.scroll.refresh,500)
      this.$bus.$on('itemImageLoad',() =>{
        //这里执行很频繁要进行防抖操作
        // this.$refs.scroll && this.$refs.scroll.refresh()
        refresh()
      })



    }
  }
</script>

<style scoped>
  .home{
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;
    /*position: fixed;*/
    /*top: 0;*/
    /*right: 0;*/
    /*left: 0;*/
    /*z-index: 10;*/
  }
  .tab-control{
    /*position: sticky;*/
    /*top: 44px;*/
    /*z-index: 9;*/
  }
  tab-control2{
    position: relative;
    z-index: 9;
    background-color: #eeeeee;
  }
  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
