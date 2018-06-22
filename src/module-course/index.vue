<template>
  <div class="learingCourse">
     <mt-header title="我的班级">
    
    </mt-header>
    <div class="courseItem" >
      <div class="item" v-for="(item, index) in MyCoursesData" :key="index">
        <div class="ico"><img :src="imgBaseUrl + item.COURSE_COVER" alt=""></div>
        <div class="info">
          <p class="tit">{{item.COURSE_NAME}}</p>
          <p class="speed" v-if="item.CLASSS_STATE=='0'">
            <mt-progress :value="30" :barHeight="10">
              <div slot="end"> 进行中</div>
            </mt-progress>
            <router-link :to="'/play/'+item.CLASS_ID">查看详情</router-link>
          </p>
          <p class="speed" v-else-if="item.CLASSS_STATE=='1'">
            <mt-progress :value="100" :barHeight="10">
              <div slot="end"> 已结课</div>
            </mt-progress>
            <router-link :to="'/play/'+item.CLASS_ID">查看详情</router-link>
          </p>
          <p class="speed"v-else-if="item.CLASSS_STATE=='2'">
            <mt-progress :value="0" :barHeight="10">
              <div slot="end"> 待审查</div>
            </mt-progress>
            <router-link :to="'/play/'+item.CLASS_ID">查看详情</router-link>
          </p>
        </div>
      </div>
    </div>
    <learingFooter></learingFooter>
  </div>
</template>
<script>
  import learingFooter from './../components/footer.vue'
  import IndexApi from '../api/learingInd.js'
  import cfg from './../utils/config'

  export default {
    name: 'learingCourse',
    data () {
      return {
        imgBaseUrl:cfg.imgBaseUrl,
        searchValue: '',
        MyCoursesData:'',
        param:{}
      }
    },
    methods:{
      init: function() {

        this.getMyCourses()
      },
      // 搜索
      goSeach: function() {
        this.$router.push({path:'/search/'})
      },
      getMyCourses: function(){
       console.log('ddfafe')
        IndexApi.myCourses(this.param,(ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            this.MyCoursesData = ret.data
          }
        })
      }
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
  .learingCourse{
    margin-top:25px;
    .courseItem{
      .item{
        display: flex;
        padding: 15px 15px 0px;

        .ico{
          flex:1;
          margin-right: 10px;
        }
        .info{
          flex: 2;
          position: relative;
          .tit{
            text-align: left;
            height: 40px;
            line-height: 40px;
            overflow: hidden;
          }
          .speed{
            display: flex;
            font-size: 14px;
            line-height: 30px;
            margin-top: 5px;
            width: 100%;
            a{
              flex: 2;
              color:$cl0;
              text-align: right;
              font-weight: 500;
              i:before{
                position: relative;
                top:1px;
              }
            }
            .mt-progress{
              flex:3;
              color:$cl9;
            }
          }

        }
      }
    }
  }
</style>
