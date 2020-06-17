<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
    
    <scroll class="content" 
    ref="scroll" 
    :probe-type="3" 
    @scroll="contentScroll"
    :pull-up-load="true"
    @pullingUp="loadMore">
      <home-swiper :banners="banners" class="home-swiper"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control class="tab-control"
      :titles="titles" 
      ref="tabControl"
      @tabClick="tabClick"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
   
  </div>
</template>

<script>

import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import {getHomeMultidata,getHomeGoods} from 'network/home'
import {debounce} from 'commen/utils'

export default {
  components:{
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners:[],
      recommends:[],
      titles:['流行','新款','时尚'],
      goods:{
        "pop":{page:0,list:[]},
        "new":{page:0,list:[]},
        "sell":{page:0,list:[]}
      },
      currentType:'pop',
      scroll:null,
      isShowBackTop:false,
      tabOffsetTop:0,
    }
  },
  computed: {
    showGoods(){
      return this.goods[this.currentType].list
    }
  },
  created() {
    // 1.请求多个数据
    this.getHomeMultidata();

    // 2.请求商品数据
    this.getHomeGoods('pop');
    this.getHomeGoods('new');
    this.getHomeGoods('sell');

    
  },
  mounted() {

    // 1.图片加载完成的事件监听
    const refresh = debounce(this.$refs.scroll.refresh,200)
    // 3.监听item中图片加载完成
    this.$bus.$on('itemImageLoad',() => {
      // console.log('---');
      refresh();
    })

    // 2.获取tabControl 的 offsetTop
    this.tabOffsetTop = this.$refs.tabControl.offsetTop;
    console.log(this.$refs.tabControl.$el.offsetTop);
    
  },
  methods: {

    /**
     * 事件监听相关的方法
     */
    tabClick(index){
      console.log(index);
      switch(index){
        case 0 :
          this.currentType = 'pop';
          break;
        case 1:
          this.currentType = 'new';
          break;
        case 2:
          this.currentType = 'sell';
      }
    },
    backClick(){
      console.log(this.$refs.scroll);
      this.$refs.scroll.scrollTo(0,0)
    },
    contentScroll(position){
      // console.log(position);
      if(position.y<-1000){
        this.isShowBackTop=true;
      }
      else{
        this.isShowBackTop=false;
      }
    },
    //上拉加载更多
    loadMore(){
      console.log('ss');
      this.getHomeGoods(this.currentType);
    },

    /**
     * 网络请求相关的方法
     */
    getHomeMultidata(){
      getHomeMultidata().then(res => {
        // console.log(res);
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1;
      getHomeGoods(type,page).then(res => {
        // console.log(res);
        this.goods[type].list.push(...res.data.list);
        // 页码加一
        this.goods[type].page++;
      })
    }
  },
};
</script>
<style scoped>
#home{
  height:100vh;
}
.home-nav{
  background-color: var(--color-tint);
  color: #ffffff;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 10;
}
.home-swiper{
  margin-top: 44px;
}
.tab-control{
  /* position: sticky; */
  top:44px;
  background-color: #fff;
  z-index: 9;
}
.content{
  height: calc(100% - 93px);
  overflow: hidden;
  margin-top: 44px;
}
</style>