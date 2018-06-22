<template>
  <div class="learingMy">
    <div class="head"><img :src="IMGURL" alt="">我</div>
    <div class="item">
      <mt-cell title="个人资料" to="/personalData">
        <span class="icon-go"></span>
        <i slot="icon" class="icon icon-my"></i>
      </mt-cell>
      <mt-cell title="我的订单" to="/myOrder"><mt-badge type="error" v-if="num!=0" >{{num}}</mt-badge>
        <span class="icon-go"></span>
        <i slot="icon" class="icon icon-order"></i>
      </mt-cell>
   
      <!--<mt-cell title="我的消息">-->
        <!--<span class="icon-go"></span>-->
        <!--<i slot="icon" class="icon icon-mess"></i>-->
      <!--</mt-cell>-->
      <!--<mt-cell title="我的收藏">-->
        <!--<span class="icon-go"></span>-->
        <!--<i slot="icon" class="icon icon-col"></i>-->
      <!--</mt-cell>-->
      <!--<mt-cell title="设置">-->
        <!--<span class="icon-go"></span>-->
        <!--<i slot="icon" class="icon icon-set"></i>-->
      <!--</mt-cell>-->
      <span @click="scan">
        <mt-cell title="扫一扫签到">
            <span class="icon-go"></span>
            <i slot="icon" class="icon icon-out"></i>
        </mt-cell>
      </span>
       <span @click="logOut">
        <mt-cell title="退出">
            <span class="icon-go"></span>
            <i slot="icon" class="icon icon-out"></i>
        </mt-cell>
      </span>
    </div>

  </div>
</template>
<script>
  import Users from '../api/users.js'
var token = localStorage.getItem('token')

  export default {
    name: 'learingMy',
    data () {
      return {
        IMGURL:'',
        name:'',
       num:0,
      }
    },
    methods:{
      logOut: function() {
     location.href='../index.html'
      },
      scan: function() {
      location.href='../plus/barcode.html'
      },
  init: function() {
var token = localStorage.getItem('token')
        Users.myOrder2({query:'USERID:'+token+',READ:0'}, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            if(ret.data)
              this.num=ret.data.length
          }
        })
      },
    },

    mounted:function(){
     this.name=localStorage.getItem('name')
      this.IMGURL=localStorage.getItem('IMG')
       this.init()

    }
  }
</script>

<style lang="scss">
  @import "../assets/baseScss";
  .learingMy{
    margin-top:15px;
    text-align: left;
    .head{
      background: $cl1;
      padding:30px 20px;
      line-height: 50px;
      color:$cl3;
      font-size: 20px;
      margin-bottom: 15px;
      img{
        width: 50px;
        height: 50px;
        border-radius: 50%;
        margin-right: 10px;
        float: left;
      }
    }
    .item{
      background: $cl1;
      padding:0 15px;
      li {
        line-height: 40px;
        border-bottom:solid 1px $cl5;
        position: relative;
        i{
          position: absolute;
          right: 0px;
        }
      }
      .icon{
        position: absolute;
        left: 0px;
        top:15px;
        font-size: 18px;
      }
      .mint-cell-wrapper{
        position: relative;
        padding: 0px 0px 0px 25px ;
      }
    }
  }
</style>
