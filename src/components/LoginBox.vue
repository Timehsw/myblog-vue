<template>
  <div id="login" @click.self="hideSelf">
    <div id="loginbox">
      <div class="form">

        <div v-if="target==1 || target==2" class="item">
          <div class="span">
            用户名:
          </div>
          <input v-model="username" type="text" placeholder="输入用户名">
        </div>


        <div v-if="target==1 || target==2" class="item">
          <div class="span">
            密码:
          </div>
          <input v-model="password" type="text" placeholder="输入密码">
        </div>
        <div v-if="target==2" class="item">
          <div class="span">
            重复密码:
          </div>
          <input v-model="password2" type="text" placeholder="再次输入密码">
        </div>

        <div v-if="target==3" class="item">
          <div class="span">
            网站名称:
          </div>
          <input v-model="sitename" type="text" placeholder="输入网站名称">
        </div>
        <div v-if="target==3" class="item">
          <div class="span">
            图片上传:
          </div>
          <input id="uploadlogo" @change="uploadImg($event)" type="file" style="width: 50px" placeholder="图片上传">
        </div>

        <div v-if="target==3" class="item">
          <img :src="tmplogo" alt="">
        </div>

        <button v-if="target==1" @click="toLogin">登录</button>
        <button v-if="target==2" @click="toRegister">注册</button>
        <button v-if="target==3" @click="toUpload">确定</button>

      </div>
    </div>
  </div>

</template>

<script>
import axios from "axios";
import Qs from "qs"

export default {
  name: "LoginBox",
  props: ['target'],
  data() {
    return {
      username: "",
      password: "",
      password2: "",
      sitename: "",
      tmplogo: ""
    }
  },
  mounted() {
    console.log(this.target)
  },
  methods: {
    hideSelf() {
      this.$emit('hideBox')
    },
    // 修改网站名称和logo
    toUpload() {
      var sitename = this.sitename
      var logo = this.tmplogo
      if (sitename.length > 0 || this.tmplogo.length > 0) {
        axios({
          url: 'http://127.0.0.1:9000/upload-logo/',
          method: 'put',
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          },
          data: Qs.stringify({
            sitename,
            logo
          })
        }).then((res) => {
          console.log(res)
          if (res.data == 'ok') {
            window.location.reload();// 刷新页面
          }
        })
      } else {
        alert("没有新的标题或者图片")
      }
    },
    // 上传图片
    uploadImg(e) {
      var logo = e.target.files[0]
      console.log(logo)
      var Img = new FormData()
      Img.append('logo', logo)
      axios({
        url: 'http://127.0.0.1:9000/upload-logo/',
        method: 'post',
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        },
        data: Img
      }).then((res) => {
        console.log(res)
        if (res.data) {
          this.tmplogo = "http://127.0.0.1:9000/upload/" + res.data.img
        }
      })

    },
    //登录
    toLogin() {
      console.log(this.username, this.password)
      var username = this.username
      var password = this.password
      if (username.length > 0 && password.length > 0) {
        axios({
          url: 'http://127.0.0.1:9000/login/',
          data: Qs.stringify({
            username,
            password
          }),
          method: 'post',
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          },
        }).then((res) => {
          console.log(res)
          switch (res.data) {
            case '用户不存在':
              alert('用户名不存在')
              break;
            case 'passwd error':
              alert('密码错误')
              break;
            default:
              console.log(res.data.token)
              //  将token存储在本地缓存
              window.localStorage.setItem('token', res.data.token)
              alert('登录成功')
              window.location.reload()
          }


        });
      } else {
        alert("用户名或密码不能为空")
      }
    },
    // 注册
    toRegister() {
      var username = this.username
      var password = this.password
      var password2 = this.password2
      console.log(username, password, password2)
      if (username.length > 0 && password.length > 0 && password2.length > 0) {
        if (password != password2) {
          alert("两次密码输入不相同!!!");
        } else {
          axios({
            url: 'http://127.0.0.1:9000/register/',
            data: Qs.stringify({
              username,
              password,
              password2,
            }),
            method: 'post',
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
          }).then((res) => {
            console.log(res)
            switch (res.data) {
              case 'same':
                alert("用户已经存在!!!")
                break


            }
          })
        }
      } else {
        alert("缺少必填项")
      }
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
  background: #00000020;
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