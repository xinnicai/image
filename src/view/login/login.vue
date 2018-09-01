<style lang="less">
    @import './login.less';
</style>
<!--登录-->
<template>
    <div class="login" @keydown.enter="handleSubmit">
        <!-- <div style="width: 100%;height: 40px;background: #2a303c;opacity: 0.7"></div> -->
        <!-- <div style="color: white;font-size: 20px;position: absolute;top: 6px;left: 50px;">医学影像平台</div> -->
        <div class="login-con">
            <Card :bordered="false">
                <p slot="title">
                    <Icon type="log-in"></Icon>
                    欢迎登录
                </p>
                <div class="form-con">
                    <Form ref="loginForm" :model="form" :rules="rules">
                        <FormItem prop="userName">
                            <Input v-model="form.userName" placeholder="请输入用户名">
                                <span slot="prepend">
                                   <Icon :size="16" type="ios-person"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <FormItem prop="password">
                            <Input type="password" v-model="form.password" placeholder="请输入密码">
                                <span slot="prepend">
                                    <Icon :size="14" type="md-lock"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <span style="color:red;" v-show="isOk">用户名密码错误！</span>
                        <FormItem>
                            <Button @click="handleSubmit" type="primary" long>登录</Button>
                        </FormItem>
                    </Form>
                    <p class="login-tip">输入用户名和密码登录</p>
                </div>
            </Card>
        </div>
    </div>
</template>

<script>
import Cookies from 'js-cookie'
import Axios from 'axios'
export default {
  data () {
    return {
      isOk: false,
      form: {
        userName: 'admin',
        password: ''
      },
      rules: {
        userName: [
          { required: true, message: '账号不能为空', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    handleSubmit () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$router.push({
            name: 'home'
          })
          // Axios({
          //   method: 'post',
          //   url: 'http://10.10.0.103:8083/medicalview/loginval',
          //   data: {
          //     username: this.form.userName,
          //     password: this.form.password
          //   },
          //   emulateJSON: true
          // }).then((res) => {
          //   let data = res.data
          //   if (data.success === true) {
          //     this.$router.push({
          //       name: 'home'
          //     })
          //     this.$store.commit('setAvator', 'https://file.iviewui.com/dist/a0e88e83800f138b94d2414621bd9704.png')
          //     sessionStorage.setItem('visualmodel', data.visualmodel)
          //     sessionStorage.setItem('imageViewUrlPrefix', data.imageViewUrlPrefix)
          //     // this.$store.commit('setVisualmodel', data.visualmodel)
          //     // this.$store.commit('setImageViewUrlPrefix', data.imageViewUrlPrefix)
          //   } else {
          //     this.$Notice.error(
          //       {
          //         desc: '用户名或密码错误！'
          //       }
          //     )
          //   }
          //   // this.$store.commit('changeLogin', '1')
          // }).catch((error) => {
          //   console.log(error)
          // })
        }
      })
    }
  }
}
</script>

<style>

</style>
