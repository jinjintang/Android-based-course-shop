<template>
  <div class="learingIndexPay">
    <!--<div class="top"><span class="icon-back">  </span> 收银台 </div>-->
    <mt-header title="收银台">
      <router-link to="/" slot="left">
        <mt-button icon="back"></mt-button>
      </router-link>
    </mt-header>
    <div class="courseInfo">
      <div class="tit">商品信息 : <span></span></div>
      <div class="cont">
        <div class="ico"><img :src="imgBaseUrl + payCoursesData.COURSECOVER" alt=""></div>
        <div class="info">
          <p class="name">{{payCoursesData.COURSENAME}}</p>
          <p class="pic">￥{{payCoursesData.COURSEPRICE}}</p>
        </div>
      </div>
    </div>
    <div class="payWay">
      <div class="tit">支付方式</div>
      <div class="cont">
        <mt-radio
          align="right"
          v-model="value"
          :options="options">
        </mt-radio>
      </div>
    </div>
    <div class="payCourse" >￥{{payCoursesData.COURSEPRICE}}<span @click="submit">立即付款</span></div>
  </div>
</template>
<script>
  import learingFooter from './../components/footer.vue'
  import IndexApi from '../api/learingInd.js'
  import cfg from './../utils/config'
  import exp from './../utils/exp'

  export default {
    name: 'learingIndexPay',
    data () {
      return {
        value: 'wx',
        imgBaseUrl: cfg.imgBaseUrl,
        payCoursesData: [],
        options: [
          {
            label: '微信',
            value: 'wx',
          },
          {
            label: '支付宝',
            value: 'zfb'
          }]
      }
    },
    methods:{
      init: function() {

        this.getCourseItem(this.$route.params.classId)
       
      },
      getCourseItem: function(obj){
        IndexApi.payCourses({id:obj}, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            this.payCoursesData = ret.data
             this.submit2()
          }
        })
      },
      submit: function() {
        var token = localStorage.getItem('token')
           IndexApi.submitorder({COURSEID:this.payCoursesData.Id+"",
            COURSENAME:this.payCoursesData.COURSENAME,
            CLASSPRICE:this.payCoursesData.COURSEPRICE,
            COURSECOVER:this.payCoursesData.COURSECOVER,
            USERNAME:localStorage.getItem('name'),
            USERPHONE:localStorage.getItem('PHONE'), 
           AREANAME:localStorage.getItem('AREA'),

            USERID:token
           }, (ret, err) => {
          if (err) {
            console.log(err)
          }else{
              var trade={COURSEID:this.payCoursesData.Id+"",
            COURSENAME:this.payCoursesData.COURSENAME,
            CLASSPRICE:this.payCoursesData.COURSEPRICE,
            COURSECOVER:this.payCoursesData.COURSECOVER,

            USERID:token
           }
            exp.rm(trade)
            console.log(ret.data)
           this.$router.go(-1)
          }
        })

      },
      submit2: function() {
        var token = localStorage.getItem('token')
         console.log("payCoursesData",this.payCoursesData)
           var trade={COURSEID:this.payCoursesData.Id+"",
            COURSENAME:this.payCoursesData.COURSENAME,
            CLASSPRICE:this.payCoursesData.COURSEPRICE,
            COURSECOVER:this.payCoursesData.COURSECOVER,

            USERID:token
           }
           exp.set(trade)

console.log(exp.auto())

      },
    },
    mounted:function(){
      this.init()
    },
    components: {
      learingFooter
    }
  }
</script>

<style lang="scss">
  @import "../assets/baseScss";
  .learingIndexPay{
    text-align: center;
    magin-top:40px;
    .top{
      background: $cl1;
      position: relative;
      line-height: 40px;
      color:$cl7;
      font-size: 16;
      span:before{font-size: 15px;}
      span:nth-child(1){
        position: absolute;
        top:10px;
        left:15px;
      }
      span:nth-child(2){
        position: absolute;
        top:10px;
        right:15px;
      }
    }
    .courseInfo{
      text-align: left;
      background: $cl1;
      margin: 10px 0;
      padding:10px;
      .tit{
        line-height: 30px;
        border-bottom: solid 1px $cl5;
        margin-bottom: 10px;
      }
      .cont{
        display: flex;
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
    }
    .courseInfo{
      text-align: left;
      background: $cl1;
      margin: 10px 0;
      padding:10px;
      .tit{
        line-height: 30px;
        border-bottom: solid 1px $cl5;
        margin-bottom: 10px;
      }
      .cont{
        display: flex;
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
    }
    .payWay{
      text-align: left;
      background: $cl1;
      margin: 10px 0;
      padding:10px;
      .tit{
        line-height: 30px;
      }
      .cont{
        .mint-cell-wrapper{padding:0px;}
      }
    }
    .payCourse{
      position: fixed;
      bottom:0px;
      width: 100%;
      text-align: left;
      background: $cl1;
      padding-left: 30px;
      font-size: 18px;
      line-height: 50px;
      color:$cl13;
      span{
        position: absolute;
        right: 0px;
        width: 50%;
        background: $cl0;
        color:$cl1;
        text-align: center;
      }
    }
  }
</style>
