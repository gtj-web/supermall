<template>
  <div class="recommend">
      <div v-for="item in recommends" :key="item.index" class="item">
          <a :href="item.link">
              <img :src="item.image" alt="">
              <div>{{item.title}}</div>
          </a>
      </div>
  </div>
</template>

<script>
export default {
    props:{
        recommends:{
            type:Array,
            default(){
                return []
            }
        }
    }
};
</script>

<style scoped>
.recommend{
    display: flex;
    width: 100%;
    font-size: 12px;
    text-align: center;
    padding: 10px 0 20px;
    border-bottom:10px solid #eee;
}
.item{
    flex: 1;
    
}
.item img{
    width: 75px;
    margin-bottom: 10px;
}
</style>