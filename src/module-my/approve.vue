<template>
  <div class="myOrder">
    <mt-header title="课程审批">
     
    </mt-header>

    <mt-navbar v-model="selected">
      <mt-tab-item id="0">待审批</mt-tab-item>
      <mt-tab-item id="1">已通过</mt-tab-item> 
      <mt-tab-item id="2">未通过</mt-tab-item>
    
    </mt-navbar>
    <!-- tab-container -->
    <!--<mt-tab-container v-model="selected">-->
      <!--<mt-tab-container-item id="1">-->
    <div style="margin-top: 10px;">
      
        <div class="item" v-for="(item, index) in myOrderData" :key="index">

          
          <div class="cont">
            <div class="ico"><img src="../assets/cs1.png" alt=""></div>
            <div class="info">
              <p class="name">{{item.COURSE_NAME}}</p>
              <p class="pic"> ￥{{item.COURSE_PRICE}}</p>

             

            </div>
          </div>
          <div class="active">
            <p>地区: <i class="pic">{{item.AREA_NAME}}</i></p>
            <p class="act">
              <span v-if="selected == 0"><router-link :to="'/approvedetail/'+item.COURSE_ID+'?Aid='+item.ID">去审批</router-link></span>
              <span v-else-if="selected == 1"><router-link :to="'/coursedetail/'+item.COURSE_ID">已通过</router-link></span>
              <span v-else-if="selected== 2"><router-link :to="'/coursedetail/'+item.COURSE_ID">不通过</router-link></span>
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
        selected: '0',
        myOrderData: '',
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
      getMyCourses: function(){
        
       
        Users.approveItem({key:this.selected}, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            this.myOrderData = ret.data
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
