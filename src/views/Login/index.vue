<template>
  <div id="login">
      <!-- 内容区 -->
    <div class="login-wrap">
      <ul class="menu-tab">
        <li :class="{'current':item.current}" v-for="item in menuTab" :key="item.id" @click="toggleMneu(item)">{{item.txt}} </li>
        
      </ul>
    </div>
  </div>
</template>
<script>
  export default{
    name: "login",
    data () {
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
       isActive: true
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
  height: 330px;
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