<template>
  <div id="login">
      <!-- 内容区 -->
    <div class="login-wrap">
      <ul class="menu-tab">
        <li :class="{'current':item.current}" v-for="item in menuTab" :key="item.id" @click="toggleMneu(item)">{{item.txt}} </li>
        
      </ul>
      <!-- 表单 start -->
     <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form" size="medium">
      <el-form-item prop="username" class="item-from"> 
        <label>邮箱</label>
        <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item  prop="password" class="item-from">
        <label>密码</label>
        <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
      </el-form-item>
      <el-form-item  prop="code" class="item-from">
        <label>验证码</label>
        <el-row :gutter="10">

          <el-col :span="15">
            <el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input>
          </el-col>
          <el-col :span="9">
            <el-button type="success" class="block">获取验证码</el-button>
          </el-col>
       
        </el-row>
         
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block">登陆</el-button>
      
    </el-form-item>
</el-form>
    </div>
  </div>
</template>
<script>
  export default{
    name: "login",
    data () {
      
      //验证用户名
      var validateUsername = (rule, value, callback) => {
        let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
        if (value === '') {

          callback(new Error('请输入用户名'));
        } else if(!reg.test(value)){
          callback(new Error('用户名格式有误'))

        }else {
          callback();
        }
      };
      //验证密码
      var validatePassword = (rule, value, callback) => {
        let reg = /^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/;
        if (value === '') {

          callback(new Error('请输入密码'));
        } else if(!reg.test(value)){
          callback(new Error('密码为6至20位的数字+字母'))

        }else {
          callback();
        }
      };
      //验证验证码
      var checkAge = (rule, value, callback) => {
         let reg = /^[a-z0-9]{6}$/
        if (value === '') {

          callback(new Error('请输入验证码'));
        } else if(!reg.test(value)){
          callback(new Error('验证码格式有误'))

        }else {
          callback();
        }
      };
      return {
       menuTab: [
         {
          txt: '登陆',
          current: true
         },
         {
          txt: '注册',
          current: false
          }
       ],
       isActive: true,
       ruleForm: {
          username: '',
          password: '',
          code: ''
        },
        rules: {
          username: [
            { validator: validateUsername, trigger: 'blur' }
          ],
          password: [
            { validator: validatePassword, trigger: 'blur' }
          ],
          code: [
            { validator: checkAge, trigger: 'blur' }
          ]
        }
      
      }
    },
    created () {
      
    },
    mounted () {
      
      
    },
    methods: {
      //vue数据驱动视图渲染
      toggleMneu(data){
         this.menuTab.forEach(elem => {
           elem.current = false
         })
        data.current = true
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      }
      

      
    }
  }

</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, .1);
  }
}
.login-form {
  margin-top: 29px;
  label {
    text-align: left ;
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
  .item-from { margin-bottom: 13px; }
  .block {
    display: block;
    width: 100%;
  }
  .login-btn { margin-top: 19px; }
}
</style>
<!--
密码加密：
1、在前端预先加密一次
登录的密码：123456（普通字符串）
经过加密后：sha1('123456') == '541216ad5s4f5ds1f5asd4f65asd4' （加密后的字符串）


2、后台加密
接收到字符串：'541216ad5s4f5ds1f5asd4f65asd4'
后台再次加密：md5('541216ad5s4f5ds1f5asd4f65asd4') == '8f9qwersd3g165y4d1sf3s1f6aew4'（最终的加密后的密码）
最终新的字符串写入数据库：8f9qwersd3g165y4d1sf3s1f6aew4


3、登录
用户名与加密后的密码进行匹配，成功则登录，失败则提示
-->