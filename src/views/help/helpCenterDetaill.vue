<template>
  <div>
     <div class="brand-top-nav">
        <router-link tag="a" to="/">首页</router-link>
        <span class="iconfont iconjiantou"></span>
        <router-link tag="a" to="/helpCenter">帮助中心</router-link>
        <span class="iconfont iconjiantou"></span>
        <span class="now-nav">{{list[activeIndex].ClassName}}</span>
    </div>
    <div class="box">
      <div class="select">
        <h3 class="">常见问题  <span class="arrow iconfont iconxiasanjiao"></span></h3>
      </div>
      <div class="box-left">
         <h3>{{list[activeIndex].ClassName}}</h3>
         <ul class="list">
           <li :class="{active:activeIndex == index}" v-for="(item,index) in list" :key="index"><a @click="changeInde(index)">{{item.Title}}</a></li>
         </ul>
      </div>
      <div class="box-right">
         <div class="title">
           {{list[activeIndex].Title}}
         </div>
         <div class="content" v-html="list[activeIndex].Content">
          
         </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      nowName:'',
      classId:'',
      activeIndex:0,
      list:[],
    }
  },
  mounted(){
    this.classId = this.$route.query.id;
    this.getQuestion();
  },
  methods:{
    getQuestion(){
      this.$http.post(this.$api.Q_A_ByClass,{ID:this.classId}).then(res=>{
        if(res.data.Code == 1){
           this.list = res.data.Data
        }
      })
    },
    changeInde(index){
      this.activeIndex = index
    }
  }
}
</script>
<style lang="less" scoped>
.box{
  width:@max-width;
  margin:0 auto 100px;
  .clear();
  .select{
    display: none;
  }
  .box-left{
    float: left;
    width:190px;
    height:100%;
    background:#fff;
    min-height: @persion_height;
    h3{
      line-height: 55px;
      font-size:16px;
      padding:0 25px;
    }
    .list{
      li{
        height:44px;
        padding:0 25px;
        line-height: 44px;
        a{
          font-size:14px;
          color:@persion_left;
          vertical-align: middle;
        }
        &.active{
          background:@persion_active;
        }
        &:hover{
          background:@persion_active;
        }
      }
    }

  }
  .box-right{
    float:right;
    width:990px;
    height:100%;
    background:#fff;
    min-height:@persion_height;
    .title{
      margin:0 30px;
      line-height:88px ;
      font-size:28px;
      border-bottom:1px solid @class_border;
    }
    .content{
      margin:25px 30px 0;
      font-size:12px;
    }
  }
  @media screen and(max-width:@change_width){
    width:100%;
    .select{
      display: block;
      height:58px;
      background:#fff;
      margin:10px auto;
      h3{
        position: relative;
        font-size:20px;
        line-height: 58px;
        margin:0 15px;
        padding-left:14px;
        &::before{
          content:'';
          position: absolute;
          left:0;
          top:19px;
          width:4px;
          height:18px;
          background:@main;
        }
        .iconfont{
          position: absolute;
          top:50%;
          right:0;
          transform: translateY(-50%) scale(.6);
          color:@main;
          font-size:12px;

        }
      }
    }
    .box-left{
      display: none;
    }
    .box-right{
      width:100%;
      height: auto;
      min-height: auto;
      padding-bottom: 60px;
      .title{
        margin:0 15/@p;
        line-height:80/@p ;
        font-size:20/@p;
        text-align: center;
      }
      .content{
        margin:24/@p 15/@p 0;
      }
    }
  }
}
</style>
