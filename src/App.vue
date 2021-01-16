<template>
  <div id="home">
    <button>登录</button>
    <button>注册</button>
    <div class="header">
      <h1>Time渐行渐远</h1>
      <img src="./assets/logo.png" alt=""/>
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
    <LoginBox></LoginBox>
    <div class="footer">
      Copyright © 2020 - 2021 Thinking.H
    </div>
  </div>
</template>

<script>
import axios from "axios";
import LoginBox from "@/components/LoginBox";

export default {
  components:{
    LoginBox
  },
  data() {
    return {
      menuList: [],
      choosed: 1,
      choosed_text: 'Django后端'
    }
  },
  mounted() {
    this.getMenuList()
  },
  methods: {
    // 获取分类列表
    getMenuList() {
      axios({
        url: 'http://127.0.0.1:9000/get-menu-list',
        type: 'json',
        method: 'get'
      }).then((res) => {
        this.menuList = res.data
      })
    },
    chooseMenu(id) {
      // console.log(id)
      this.choosed = id
      this.choosed_text = this.menuList[id - 1].text
      // 进行传参跳转
      this.$router.push({path:'/',query:{'menuId':id}})
    }
  }
}
</script>
<style>
</style>
