<template>
    <div class="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
        <scroll class="content" ref="scroll">
            <detail-swiper :top-images="topImages"></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
            <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"></detail-goods-info>
            <detail-param-info :param-info="paramInfo"></detail-param-info>
        </scroll>
    </div>
</template>

<script>
    import {getDetail,Goods,Shop,GoodsParam} from "network/detail";
    import Scroll from "components/common/scroll/Scroll";

    import DetailNavBar from "./childcomps/DetailNavBar";
    import DetailSwiper from "./childcomps/DetailSwiper";
    import DetailBaseInfo from "./childcomps/DetailBaseInfo";
    import DetailShopInfo from "./childcomps/DetailShopInfo";
    import DetailGoodsInfo from "./childcomps/DetailGoodsInfo";
    import DetailParamInfo from "./childcomps/DetailParamInfo";

    export default {
        name: "Detail",
        data(){
            return {
                iid:null,
                topImages: [],
                goods:{},
                shop:{},
                detailInfo:{},
                paramInfo:{}
            }
        },
        components:{
            DetailNavBar,
            DetailSwiper,
            DetailBaseInfo,
            DetailShopInfo,
            Scroll,
            DetailGoodsInfo,
            DetailParamInfo
        },
        methods:{
            imageLoad(){
                // console.log(this.$refs.scroll.refresh)
                this.$refs.scroll.refresh()
            }
        },
        created() {
            this.iid = this.$route.params.iid
            getDetail(this.iid)
                .then((res) =>{
                    const data = res.result
                    //详情页轮播图数据
                    this.topImages = data.itemInfo.topImages
                    //获取商品基本信息
                    this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
                    //获得商铺信息
                    this.shop = new Shop(data.shopInfo)
                    // 保存商品的详情数据
                    this.detailInfo = data.detailInfo;
                    // 获取参数的信息
                    this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

                })
        }
    }
</script>

<style scoped>
    .detail{
        position: relative;
        z-index: 9;
        background-color: #fff;
        height: 100vh;
    }
    .detail-nav {
        position: relative;
        z-index: 9;
        background-color: #fff;
    }
    .content{
        height: calc(100% - 44px);
        /*padding-top: 44px;*/
        /*height: 300px;*/
        /*overflow: hidden;*/
    }

</style>