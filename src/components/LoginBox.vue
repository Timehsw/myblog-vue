<template>
  <div id="login">
    <div id="loginbox">
      <div class="form">

        <div class="item">
          <div class="span">
            用户名:
          </div>
          <input v-model="username" type="text" placeholder="输入用户名">
        </div>

        <div class="item">
          <div class="span">
            密码:
          </div>
          <input v-model="password" type="text" placeholder="输入密码">
        </div>

        <button @click="toLogin">登录</button>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "LoginBox",
  data(){
    return {
      username:"",
      password:""
    }
  },
  methods:{
    toLogin(){
      console.log(this.username,this.password)
    }
  }
}
</script>

<style scoped>
#login {
  position: absolute;
  top: 0;
  width: 700px;
  height: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
#loginbox {
  width: 300px;
  height: 300px;
  border: 1px solid #000;
  background: #00000090;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
}
#loginbox .form .item {
  font-weight: 700;
  margin: 10px auto;
}
#loginbox .form .item .span {
  float: left;
  width: 80px;
}
</style>