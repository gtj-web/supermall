<template>
  <div class="goods">
     <goods-list-item v-for="item in goods" :goodsItem="item" :key="item.key" class="goods-item"></goods-list-item>
  </div>
</template>

<script>
import GoodsListItem from './GoodsListItem'

export default {
    components:{
        GoodsListItem
    },
    props:{
        goods:{
            type:Array,
            default(){
                return []
            }
        }
    }
};
</script>

<style scoped>
.goods{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}
.goods-item{
    width: 48%;
    /* padding:2px; */
}
</style>