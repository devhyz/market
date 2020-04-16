<template>
    <div class="home-swiper">
        <swiper>
            <swiper-item v-for="item in banners">
                <a :href="item.link"></a>
                <img :src="item.image" alt="">
            </swiper-item>
        </swiper>
    </div>
</template>

<script>
    import {Swiper,SwiperItem} from "components/common/swiper"
    export default {
        name: "HomeSwiper",
        props:{
            banners:{
                type:Array,
                default(){
                    return []
                }
            }
        },
        components:{
            SwiperItem,
            Swiper
        }
    }
</script>

<style scoped>

</style>