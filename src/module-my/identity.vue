<template>
  <div class="register">
    <mt-header title="身份" style="background:#26a2ff;color:#fff;">
      
    </mt-header>
    <div class="formItem">

   
    
      <div class="subBut" @click="registerSub1">我是学生</div> 
      <div class="subBut1" @click="registerSub">我是审批人员</div>
     
    </div>
  </div>
</template>
<script>
  import logApi from '../api/users'
  import { MessageBox } from 'mint-ui';
import Users from '../api/users.js'
  export default {
    name: 'register',
    data () {
      return {
       
        myInfoData: []
      }
    },
    methods:{
      //提交注册
      registerSub: function() {
        // if (this.phonStat == 'success' ) {
          
          Users.validate(this.myInfoData,(ret, err) => {
            if (err) {
              alert('登录失败！请稍后重试！')
            }else{
              if(ret.data=='null'){
                   this.$msgbox('提示','用户名错误')
              }
                else{
                  if(ret.data.PASSWORD==this.myInfoData.PASSWORD){
                    localStorage.setItem("token",ret.data.Id)
                    localStorage.setItem("area",ret.data.AREAID)
                      this.$router.push('/approve')
                  }
             else   this.$msgbox('提示','密码错误')
            }}
          })
        // } else {
        //   MessageBox('提示', '请输入正确的信息后重试！');
        // }
      },
       registerSub1: function() {
        // if (this.phonStat == 'success' ) {
          
        
      },
      // 发送验证码
     
    },
   
    mounted:function(){

    },
    components: {
    }
  }
</script>

<style lang="scss">
  @import "../assets/baseScss";
  .register{
    margin-top:20px;
    .formItem{
      font-size: 18px;
      margin: 0 30px;
      p{position: relative}
      .mint-cell{
        background: transparent;
      }
      .mint-cell-wrapper{
        background: transparent;
        padding: 0;
        border-bottom: solid 1px $cl14;
      }
      .subBut{
       
        background: $cl0;
        color:$cl1;
        text-align: center;
        line-height: 40px;
        margin-top: 200px ;
      }
      .subBut1{
       
        background: $cl0;
        color:$cl1;
        text-align: center;
        line-height: 40px;
        margin: 20px 0;
      }
      .more{
        position: relative;
        width: 100%;
        left: 0px;
        top: 40px;
        text-align: center;
        i{
          color: $cl0;
        }
        .forgetPass{
          color:$cl9;
        }
      }
      .sendCode{position: absolute;right: 0px;top:6px;display: inline-block;font-size: 14px;color:$cl0;border:solid 1px $cl0;padding: 5px;}
    }
  }
</style>
