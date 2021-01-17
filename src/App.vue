<template>
  <div id="home">
    <div>
      <button v-if="loginType==false" @click="showLoginRegisterBox(1)">登录</button>
      <button v-if="loginType==false" @click="showLoginRegisterBox(2)">注册</button>
      <button v-if="loginType" @click="toHome">首页</button>
      <button v-if="loginType" @click="toUserInfo">个人中心</button>
      <button v-if="loginType" @click="showLoginRegisterBox(3)">修改</button>
    </div>
    <div class="header">
      <h1>{{ siteinfo.sitename }}</h1>
      <img :src="siteinfo.logo" alt=""/>
    </div>
    <div class="content">
      <div class="menu">

        <div v-for="item in menuList" :key="item.id" class="item">
          <div v-if="item.id==choosed" style="background: #777;color: #fff">
            <a style="color:#fff">{{ item.text }}</a>
          </div>
          <div v-else @click="chooseMenu(item.id)">
            <a style="color:#000">{{ item.text }}</a>
          </div>
        </div>

      </div>

      <div class="userlist">
        <p>{{ choosed_text }}</p>
        <hr/>
        <router-view/>
      </div>
    </div>
    <hr/>
    <LoginBox v-if="boxtarget" :target="boxtarget" @hideBox="hideLoginRegisterBox"></LoginBox>
    <div class="footer">
      Copyright © 2020 - 2021 Thinking.H
    </div>
  </div>
</template>

<script>
import axios from "axios";
import LoginBox from "@/components/LoginBox";

export default {
  components: {
    LoginBox
  },
  data() {
    return {
      menuList: [],
      choosed: 1,
      choosed_text: 'Django后端',
      boxtarget: 0,
      siteinfo: {},
      loginType: false
    }
  },
  mounted() {

    try {
      if (window.localStorage.getItem('token').length > 0) {
        this.loginType = true
      }
    } catch (e) {
      console.log(e)
    }
    this.getMenuList();
  },
  methods: {
    toUserInfo(){
      this.$router.push({path:'/userinfo'})
    },
    toHome(){
      this.$router.push({path:'/'})
    },
    // 获取分类列表
    getMenuList() {
      axios({
        url: 'http://127.0.0.1:9000/get-menu-list',
        type: 'json',
        method: 'get'
      }).then((res) => {
        this.menuList = res.data.menu_data
        this.siteinfo = res.data.site_info
      })
    },
    chooseMenu(id) {
      // console.log(id)
      this.choosed = id
      this.choosed_text = this.menuList[id - 1].text
      // 进行传参跳转
      this.$router.push({path: '/', query: {'menuId': id}})
    },
    // 展示登录注册框体
    showLoginRegisterBox(value) {
      this.boxtarget = value
    },
    // 隐藏父组件
    hideLoginRegisterBox() {
      this.boxtarget = 0
    }
  }
}
</script>
<style>
</style>
