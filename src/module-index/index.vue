<template>
  <div class="learingIndex">
    <mt-tab-container v-model="selected" swipeable="swipeable">
      <mt-tab-container-item id="index">
        <LearingHeader></LearingHeader>
        <ClassItem v-if="rcomData.length > 0" :title="'精品推荐'" :dataes="rcomData"></ClassItem>
        <ClassItem v-if="courseData.length > 0" :title="'热门课程'" :labs="'true'" :dataes="courseData"></ClassItem>
      </mt-tab-container-item>
      <mt-tab-container-item id="list">
        <listModular></listModular>
      </mt-tab-container-item>
      <mt-tab-container-item id="course">
        <courseModular></courseModular>
      </mt-tab-container-item>
      <mt-tab-container-item id="my">
        <myModular></myModular>
      </mt-tab-container-item>
    </mt-tab-container>

    <div class="learingFooter">
      <mt-tabbar v-model="selected" :fixed="true">
        <mt-tab-item id="index" >
          <i class="icon icon-ind"></i>
          首页
        </mt-tab-item>
        <mt-tab-item id="list" >
          <i class="icon icon-class"></i>
          分类
        </mt-tab-item>
        <mt-tab-item id="course">
          <i class="icon icon-course"></i>
          班级
        </mt-tab-item>
        <mt-tab-item id="my">
          <i class="icon icon-my"></i>
          我的
        </mt-tab-item>
      </mt-tabbar>
    </div>
  </div>
</template>
<script>
  import httpCt from '../api/httpClient'
  import LearingHeader from './../components/index-header.vue'
  import ClassItem from './../components/class-Item.vue'
  import listModular from './../module-list/index.vue'
  import courseModular from './../module-course/index.vue'
  import myModular from './../module-my/index.vue'
  import learingFooter from './../components/footer.vue'
  import IndexApi from '../api/learingInd.js'
  import cfg from './../utils/config'
 import Users from '../api/users.js'
  export default {
    name: 'learingIndex',
    data () {
      return {
        rcomData: [],       // 推荐课程
        courseData: [],     // 全部（首页数据分析）课程
        swipeable: 'false',
        selected: 'index',
        imgBaseUrl:cfg.imgBaseUrl,
        param:{}
      }
    },
    methods:{
      adduser:function(){
        Users.addUser({USEROPENID:this.$openid},(ret, err) => {
            if (err) {
              alert('注册失败！请稍后重试！')
            }else{
              localStorage.setItem('token',ret.data.Id)
                localStorage.setItem('name',ret.data.USERNAME)
            }
          })
      },

      init: function() {
        let sel = this.$route.path

         Users.getUser({query:'USEROPENID:'+this.$openid},(ret, err) => {
            if (err) {
              alert(err)
            }else{
              if(ret.data==null){
          this.adduser()
       this.$msgbox('提示','第一次登录，注意修改个人资料')
              }
              else {
                localStorage.setItem('token',ret.data[0].Id)
               
                localStorage.setItem('name',ret.data[0].USERNAME) 
                localStorage.setItem('PHONE',ret.data[0].PHONENUMBER) 
                localStorage.setItem('AREA',ret.data[0].ADRESS) 
              
                  
            }
            }
          })
       
        if (sel.slice(1)){
          this.selected = sel.slice(1)
        }
        // 精品推荐
        this.getCourseItem('rcom')
        // 全部分类
        this.getCourseItem()
      },
      getCourseItem: function(obj){

        if(obj){
          //this.param.rcom = true
          this.param.limit = 3
        }else{
          //this.param.rcom = ''
          this.param.limit = 10
        }
        // var area = localStorage.getItem('area')
        // this.param.query = 'AREAID:'+area
        IndexApi.coursesItem(this.param,(ret, err) => {
          if (err) {
            console.log(err)
          }else{
            console.log(ret.data)
            if(obj){
              this.rcomData = ret.data
            }else{
              this.courseData = ret.data
            }
          }
        })
      }
    },
    watch:{
      selected(val, oldVal){
        this.$router.push(val)
      }
    },
    mounted:function(){
      this.init()
    },
    components: {
      LearingHeader,
      ClassItem,
      listModular,
      courseModular,
      myModular,
      learingFooter
    },

  }
</script>

<style lang="scss">
  @import "../assets/baseScss";
.learingIndex{
  text-align: center;
}
.learingFooter{
  color:$cl7;
  margin-top: 51px;
.icon{
  display: block;
  font-size: 22px;
  margin-bottom: 3px;
}
}
</style>
