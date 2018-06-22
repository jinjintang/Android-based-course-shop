<template>
  <div class="myOrder">
    <mt-header title="我的消息">
      <router-link to="/myOrder" slot="left">
        <mt-button icon="back"></mt-button>
      </router-link>
    </mt-header>

    <mt-navbar v-model="selected">
      <mt-tab-item id="0">未读消息</mt-tab-item>
      <mt-tab-item id="1">已读消息</mt-tab-item> 
      
    
    </mt-navbar>
    <!-- tab-container -->
    <!--<mt-tab-container v-model="selected">-->
      <!--<mt-tab-container-item id="1">-->
    <div style="margin-top: 10px;">
     
        <div class="item" v-for="(item, index) in myOrderData" :key="index">

         
          
          <div class="active">
            <p>{{item.MESSAGE}}</p>
            <p class="act">
              <span v-if="selected == 0">
              <mt-button type='primary' @click="pass(item)">已读</mt-button></span>
              
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
      pass(item){
     item.READ=1;


        Users.myOrder3(item, (ret, err) => {
          if (err) {
            console.log(err)
          }else{

          
          }
        })




      },
      init: function() {
         this. getMyCourses(this.$route.params.detailid,this.selected)
      },
      //提示关闭
      promptClose:function(){
        this.prompt = false;
      },
      
      getMyCourses: function(ID,RE){
        var scope=this
        console.log(ID)
      
        Users.myOrder2({query:'DETAILID:'+ID+',READ:'+RE}, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            scope.myOrderData=ret.data

          
          }
        })
     
     }
    },

    mounted:function(){
      this.init()
    },
    watch:{
      selected(newVal, oldVal){
        this.selected = newVal
        this.init()
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
