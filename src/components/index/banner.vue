<template>
  <div class="banner-box">
     <el-carousel trigger="click" :height="bannerHeight">
      <el-carousel-item v-for="(item,index) in bannerList" :key="index">
        <a :href="item.clickurl" target="_black">
         <img :src="item.picurl" alt="" draggable="false" class="banner-img">
        </a>
      </el-carousel-item>
    </el-carousel>
    
  </div>
</template>
<script>
export default {
   data(){
     return{
       bannerHeight:'340px',
       bannerList:[],
     }
   },
   mounted(){
     
   
     this.getBanner();
   },
   methods:{
     getBanner(){
       const _this =this;
        this.$http.get(this.$api.GetBannerGoods).then(res=>{
           if(res.data.Code == 1){
             this.bannerList = res.data.Data
             console.log(this.bannerList)
              this.$nextTick(()=>{
                   const img = document.querySelector('.banner-img');
                img.onload = function(){
                    _this.bannerHeight = `${img.height}px`
                }
                window.onresize = function(){
                    _this.bannerHeight = `${img.height}px`
                }
              })
           }        
        })
     }
   },
   destroyed(){
     window.onresize = null
   }
}
</script>
<style lang="less" scoped>
.banner-box{
  width: 100%;
  height:100%;
  img{
    width:100%;
    height:100%;
  }
  @media screen and(max-width:@change_width){
    &{
      height:auto;
      img{
        height:auto;
      }
    }
  }
}
</style>