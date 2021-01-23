<template>
  <div id="userinfo">
    <h1>我的信息</h1>
    <img :src="userinfo.headImg" alt="">
    <div>{{ userinfo.nickName }}</div>
    <div>身份:管理员</div>
  </div>
</template>

<script>
export default {
  name: "userinfo",
  data(){
    return {
      userinfo:{}
    }
  },
  mounted() {
    this.getUserInfo()
    this.changeLoginType()
  },
  methods:{
    getUserInfo(){
      console.log("开始获取用户信息")
      // 从vuex中获取存储的全局内容
      this.userinfo = this.$store.state.userinfo
    },
    changeLoginType(){
      this.$emit('changeUI',true)
      this.$emit('hideBox')
    }
  }
}
</script>

<style scoped>
#userinfo {
  margin-bottom: 10px;
}
</style>