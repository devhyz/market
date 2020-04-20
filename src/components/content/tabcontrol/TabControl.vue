<template>
    <div class="tab-control">
        <div class="tab-control-item" v-for="(item,index) in titles"
             @click="itemClick(index)"
             :class="{isActive: currentIndex ===index}" :key="index">
            <span>{{item}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TabControl",
        data(){
            return{
                currentIndex:0
            }
        },
        props:{
            titles:{
                type:Array,
                default(){
                    return []
                }
            }
        },
        methods:{
            itemClick(index){
                // console.log(index)
                this.currentIndex = index
                this.$emit('tabBbarItemClick' ,index)
            }
        }
    }
</script>

<style scoped>
    .tab-control{
        display: flex;
        text-align: center;
        font-size: 15px;
        height: 40px;
        line-height: 40px;
        background-color: #fff;
    }
    .tab-control .tab-control-item{
        flex: 1;
    }
    .tab-control span{
        padding: 5px;

    }
    .tab-control .isActive span{
        border-bottom: 3px solid var(--color-tint);
    }
    .isActive{
        /*color: red;*/
        color:var(--color-high-text);
    }

</style>