<template>
    <div  class="good-list">
        <good-list-item v-for="(item,index) in goods"
                        :key="index"
                        :good-items="item"></good-list-item>
        <good-list-item></good-list-item>
    </div>
</template>

<script>
    import GoodListItem from "./GoodListItem";
    export default {
        name: "GoodList",
        props:{
            goods:{
                type:Array,
                default(){
                    return []
                }
            }
        },
        components:{
            GoodListItem
        }
    }
</script>

<style scoped>
    .good-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;

        padding: 2px;
    }
</style>