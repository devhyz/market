<template>
  <div class="category">
    <nav-bar class="category-nav-bar">
      <div slot="center">购物车</div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from "../../components/common/navbar/NavBar";
  export default {
    name: "Cart",
    components:{
      NavBar
    }
  }
</script>

<style scoped>
  .category{
    color: #eeeeee;
  }
  .category-nav-bar{
    background-color: var(--color-tint);
  }
</style>