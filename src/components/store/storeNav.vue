<template>
  <div class="nav ">
    <div class="nav-box clear">
       <div class="tip">
         <span class="iconfont iconguangbo"></span>
         <span class="msg" v-for="(item,index) in tip" :key="index" :class="{active:tipActive == index}">{{item.msg}}</span>
      </div>
       <div class="nav-right">
          <!-- <a class=""></a> -->

           <span v-show="isLogin" class="">
             <router-link to="/store" class="userName login-btn">{{storeInfo.Name}}</router-link>

           </span>
          <a href="" class="help">帮助中心</a>
       </div>
      <!-- <a href="" class="regester">注册</a>
      <a class="login">登录</a> -->
      <a  class="loginout" @click="loginOut">退出登录</a>
      <router-link tag="a" to="/store" class="logined"><span class="iconfont iconzh"></span>商家中心</router-link>
    </div>
  </div>
</template>
<script>
import { mapState, mapMutations} from 'vuex' //注册 action 和 state
export default {
  data(){
    return{
      tip:[{
        msg:'优乐兑APP上线了，帮助商家解决客户流量问题'
      },{
        msg:'优乐兑APP上线了，帮助商家解决客户流量问'
      }],
      tipActive:0,
      timer:'',
      isLogin:true,
      userName:''
    }
  },
  mounted(){
    this.loop();
    this.RmbExchangeRate();
    setTimeout(()=>{
      this.getStoreInfo();
    },500)

  },
  computed:{
    ...mapState([
      'storeInfo'
    ])
  },
  methods:{
    ...mapMutations([
      'setStoreInfo',
      'setRate'
    ]),
    loop(){
      clearInterval(this.timer);
      this.timer = setInterval(()=>{
        this.tipActive++;

        if(this.tipActive>this.tip.length-1){
          this.tipActive = 0;
        }
      },4000)
    },
    getStoreInfo(){
      this.$http.storePost(this.$api.MerchanterMerchanter).then(res=>{
        if(res.data.Code == 1){
           this.isLogin = true
           this.userName = res.data.Data.Name
           let userInfo = JSON.stringify(res.data.Data);
           this.setStoreInfo(res.data.Data)
           sessionStorage.setItem('storeUserInfo',userInfo)
        }
      })
    },
    //获取huilv
    RmbExchangeRate(){
      this.$http.storeGet(this.$api.RmbExchangeRate).then(res=>{
        if(res.data.Code == 1){
          this.setRate(res.data.Data)
        }
      })
    },
    loginOut(){
      this.$alert.confirm('是否退出当前登录', '提示', {
        confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http.storeGet(this.$api.MerchanterLoginOut).then(res=>{
              if(res.data.Code == 1){
                  localStorage.setItem('storeToken','')
                  this.$router.push('/login')
              }else{
                  this.$message.error(res.data.Msg)
              }
          })
        }).catch(() => {
                    
        });
    }
  },
  beforeDestroy(){
    clearInterval(this.timer)
  }
}
</script>
<style lang="less" scoped>

.nav{
  width:100%;
  height:32px;
  background:@nav_bg;
  .nav-box{
    width:@max-width;
    height:100%;
    overflow: hidden;
    margin:0 auto;
    background:@nav_bg;
    .tip{
      float: left;
      position: relative;
      width:300px;
      font-size:12px;
      color:#fff;
      line-height:32px;
      padding-left:19px;
      .iconfont{
        position: absolute;
        left:0;
        font-size:12px;
        margin-right:5px;
      }
      .msg{
        position: absolute;
        top:0;
        left:0;
        text-align: left;
        text-indent:19px ;
        width:100%;
        display:block;
        opacity: 0;
        &.active{
           animation: msgMove .5s ease forwards;
        }
      }

    }
    .login{
      display: none;
    }
    .regester{
      display: none;
    }
    .nav-right{
      float: right;
      font-size:0;
      a{
        float: left;
        margin-top:10px;
        height: 12px;
        line-height: 12px;
        font-size: 12px;
        color:#fff;
        padding:0 12px;
        border-left:1px solid @nav-border;
        &.help{
          padding-right:0;
        }
        &.login-btn{
          border-left:0;
        }
      }
    }
  }
}
@keyframes msgMove {
   0%{
     opacity: 0;
     transform: translate3d(0,10px,0);
   }
   100%{
     opacity: 1;
      transform: translate3d(0,0,0);
   }
}
.loginout, .logined {
  display: none;
}
@media screen and(max-width:@change_width){
  .nav{
    width:100%;
    height:40/@p;
    background:@body_color;
    .nav-box{
      box-sizing: border-box;
      width:100%;
      padding:0 15px;
      background:@body_color;
      .tip{
        width:204/@p;
        font-size:12px;
        line-height: 40/@p;
        color:@subtitle_color;
        .msg{
          width:170/@p;
          height:40/@p;
        }
      }
      .nav-right{
        display: none;
      }
      .login,.regester{
        box-sizing: border-box;
        display: block;
        float: right;
        width:44/@p;
        height:24/@p;
        margin-right:6/@p;
        margin-top:8/@p;
        text-align: center;
        line-height: 22/@p;
        color:@main;
        font-size:12px;
        border:1px solid @main;
        border-radius: 4/@p;
      }
      .loginout{
        display: block;
        float: right;
        width:68/@p;
        height:24/@p;
        color:@main;
        line-height: 24/@p;
        text-align: center;
        border:1px solid @main;
        border-radius:4/@p ;
        margin-top:8/@p;
      }
      .logined{
        display: block;
        float: right;
        width:83/@p;
        height:24/@p;
        color:#4A90E2;
        line-height: 24/@p;
        text-align: center;
        border:1px solid #4A90E2;
        border-radius:4/@p ;
        margin-top:8/@p;
        margin-right: 8/@p;
        .iconfont{
          font-size:12px;
          color:#4A90E2;
          margin-right:5px;
        }
      }
    }
  }
}
</style>
