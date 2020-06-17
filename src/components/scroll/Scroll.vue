<template>
  <div class="wrapper" ref="wrapper">
      <div class="content">
        <slot></slot>
      </div> 
  </div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
    props:{
        probeType:{
            type:Number,
            default(){
                return 0;
            }
        },
        pullUpLoad:{
            type:Boolean,
            default(){
                return false
            }
        }
    },
    data() {
        return {
            scroll:null
        }
    },
    mounted() {
        this.scroll = new BScroll(this.$refs.wrapper,{
            click:true,
            probeType:this.probeType,//启动坐标监听
            pullUpLoad:this.pullUpLoad,//是否启动上拉加载
        })

        this.scroll.on('scroll',(position) => {
            // 子向父组件传递数据
            this.$emit('scroll',position);
            // console.log(position);
            
        })

        // 监听上拉加载
        this.scroll.on('pullingUp',() => {
            // console.log("上拉加载更多");
            // 发送给父组件事件
            this.$emit('pullingUp')
            this.scroll.finishPullUp()
        })

        
    },
    methods: {
        scrollTo(x,y,time=300){
            this.scroll.scrollTo(x,y,time);
        },
        refresh(){
            console.log('---');
            
            this.scroll && this.scroll.refresh();
        }
    },
};
</script>

<style scoped>
</style>