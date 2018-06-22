<template>
  <div class="student_con">

    <mt-button type="primary" @click="qd">课堂签到</mt-button>
    
  </div>
</template>

<script>
 import { MessageBox } from 'mint-ui' 
export default {
  data() {
    return {
      CLASSID: '',
      USERID: '',
      USERNAME:'',
    }
  },
  mounted() {
    

    this.USERID= localStorage.getItem('token');
    this.USERNAME =localStorage.getItem('name');

  },
  methods: {
    qd() {
      
      var date = new Date()

      let qstarttime =this.$route.query.time
      this.CLASSID =this.$route.query.CLASSID
   
      if (qstarttime != null && qstarttime.toString().length > 1) {

       var d1=new Date(qstarttime)    
       var t=parseInt((date-d1)/1000/60)
       if(t>130){ this.$msgbox('提示', '签到失败')}
       else{
        var data = localStorage.getItem('attendtime');  
        var curTime = new Date();
        if(parseInt((curTime-d1)/1000/60)<=130&&parseInt((data-d1)/1000/60)<=130){
          this.$msgbox('提示', '不要重复签到')
        }
 
      else{
           IndexApi.attend({CLASSID:this.CLASSID,
            USERID:this.USERID,
           USERNAME:this.USERNAME
           }, (ret, err) => {
          if (err) {
            console.log(err)
            this.$msgbox('提示', '签到失败')
          }else{
            console.log(ret.data)
          this.$msgbox('提示', '签到成功')
          var curTime = new Date();
         
           localStorage.setItem("attendtime",curTime);
          this.$router.push({path:'/'})
          }
        })
    
      }}


     }
       else{
        this.$msgbox('提示', '签到失败')

        }
    
    },
  },
  components: {

  }
}
</script>


<style>

.student_con{
  width :960px;
  height :220px;
  margin :0 auto;
  padding :20px 0px;
}
</style>
