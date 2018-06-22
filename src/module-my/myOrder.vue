<template>
  <div class="myOrder">
    <mt-header title="我的订单">
      <router-link to="/my" slot="left">
        <mt-button icon="back"></mt-button>
      </router-link>
    </mt-header>

    <mt-navbar v-model="selected">
      <mt-tab-item id="5">未支付</mt-tab-item>
      <mt-tab-item id="0">待开班</mt-tab-item> 
      <mt-tab-item id="1">已开班</mt-tab-item>
    
    </mt-navbar>
    <!-- tab-container -->
    <!--<mt-tab-container v-model="selected">-->
      <!--<mt-tab-container-item id="1">-->
    <div style="margin-top: 10px;">
        <div class="prompt" v-show="prompt && selected == 5"><i class="icon-prompt"></i>订单号24小时内有效，过期将自动关闭 <span class="icon-close" @click="promptClose"></span></div>
        <div class="item" v-for="(item, index) in myOrderData" :key="index">

          <div class="tit">订单编号：{{item.Id}}

          </div>
          <div class="cont">
            <div class="ico"><img src="../assets/cs1.png" alt=""></div>
            <div class="info">
              <p class="name">{{item.COURSENAME}}</p>
              <p class="pic">￥{{item.CLASSPRICE}}</p>
            </div>
          </div>
          <div class="active">
            <p>总价: <i class="pic">￥{{item.CLASSPRICE}}</i></p>
            <p class="act">
              <span v-if="selected == 5"><router-link :to="'/pay/'+item.COURSEID">去支付</router-link></span>
              <span v-if="selected == 0">
              <router-link :to="'/message/'+item.Id" v-if="myOrderData1[index]!=0">未读消息<mt-badge type="error">{{myOrderData1[index]}}</mt-badge></router-link>
              </span>
              <span v-if="selected== 1">已开班</span>
            </p>
          </div>
        </div>
    </div>
      <!--</mt-tab-container-item>-->
      <!--<mt-tab-container-item id="2">-->
        <!--<div class="item">-->
          <!--<div class="tit">订单编号：201803020453</div>-->
          <!--<div class="cont">-->
            <!--<div class="ico"><img src="../assets/cs1.png" alt=""></div>-->
            <!--<div class="info">-->
              <!--<p class="name">Think PHP 5.0 博客系统实战项目</p>-->
              <!--<p class="pic">￥299</p>-->
            <!--</div>-->
          <!--</div>-->
          <!--<div class="active">-->
            <!--<p>总价: <i class="pic">￥299</i></p>-->
            <!--<p class="act"><span>已完成</span></p>-->
          <!--</div>-->
        <!--</div>-->
        <!--<div class="item">-->
          <!--<div class="tit">订单编号：201803020453</div>-->
          <!--<div class="cont">-->
            <!--<div class="ico"><img src="../assets/cs2.png" alt=""></div>-->
            <!--<div class="info">-->
              <!--<p class="name">Think PHP 5.0 博客系统实战项目</p>-->
              <!--<p class="pic">￥299</p>-->
            <!--</div>-->
          <!--</div>-->
          <!--<div class="active">-->
            <!--<p>总价: <i class="pic">￥299</i></p>-->
            <!--<p class="act"><span>已完成</span></p>-->
          <!--</div>-->
        <!--</div>-->
      <!--</mt-tab-container-item>-->
    <!--</mt-tab-container>-->
  </div>
</template>
<script>
  import learingFooter from './../components/footer.vue'
  import Users from '../api/users.js'
  import cfg from './../utils/config'
  import exp from './../utils/exp'
  export default {
    name: 'myOrder',
    data () {
      return {
            myOrderData: '',
            myOrderData1: [],
        selected: '0',
        
        prompt:true
      }
    },
    methods:{
      init: function() {
        this.getMyCourses()

      },
      //提示关闭
      promptClose:function(){
        this.prompt = false;
      },
      //订单获取
      addnum(id){
        var scope=this
   Users.myOrder2({query:'DETAILID:'+id+',READ:0'}, (ret, err) => {
  
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            if(ret.data)
           scope.myOrderData1.push(ret.data.length)  
           else scope.myOrderData1.push(0)
              

          }
        })


      },
      getMyCourses: function(){
        var scope=this
        if(this.selected!=5){
        var token = localStorage.getItem('token')
        Users.myOrder({query:'USERID:'+token+',CLASSSTATE:'+ this.selected}, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            scope.myOrderData=ret.data

            scope.myOrderData.forEach(function(e){  
     scope.addnum(e.Id)
})
          
          }
        })
      }
      else{ console.log(exp.auto())
        this.myOrderData=exp.auto().filter(item=>item.flag==false).map(item=>item.trade);
          console.log(this.myOrderData)
      }}
    },

    mounted:function(){
      this.init()
    },
    watch:{
      selected(newVal, oldVal){
        this.selected = newVal
        this.getMyCourses()
      }
    },
    components: {
      learingFooter
    }
  }
</script>

<style lang="scss">
  @import "../assets/baseScss";
  .myOrder{
     margin-top:20px;
    .mint-navbar{
      a:first-child{
        .mint-tab-item-label{
          border-right: solid 1px $cl9;
        }
      }
      .mint-tab-item-label{
        font-size: 14px;
      }
    }
    .is-selected{
      margin-bottom: 0px !important;
    }
    .prompt{
      background: $cl12;
      color:$cl1;
      line-height: 40px;
      padding:0 15px;
      margin-bottom: 3px;
      position: relative;
      top:-10px;
      i{
        position: relative;
        margin-right: 5px;
        top:1px;
      }
      span{
        position: absolute;
        right: 15px;
        top:11px;
      }
    }
    .item{
      margin: 0 15px;
      margin-bottom: 10px;
      color: $cl9;
      background: $cl1;
      .tit{
        border-bottom: solid 1px $cl5;
        line-height: 40px;
        padding:0 15px;
      }
      .cont{
        display: flex;
        padding:15px 0;
        margin:0 15px;
        border-bottom: solid 1px $cl5;
        .ico{flex: 1;margin-right: 10px;}
        .info{
          flex: 3;
          font-size: 14px;
          line-height: 33px;
          .pic{
            font-size: 18px;
            color:$cl13;
          }
        }
      }
      .active{
        display: flex;
        padding:15px;
        p{
          flex: 1;
          .pic{
            font-size: 18px;
            color:$cl13;
          }
        }
        .act{
          text-align: right;
          a{
            color: $cl0;
          }
        }
      }
    }
    .mint-tab-container{
      margin-top: 10px;
    }
  }
</style>
