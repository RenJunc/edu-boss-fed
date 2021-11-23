<template>
  <div class='login'>
    <el-form
      :model='ruleForm'
      :rules='rules'
      ref='ruleForm'
      label-width='100px'
      label-position="top"
      class='login-ruleForm'
    >
      <el-form-item label='手机号' prop='phone'>
        <el-input v-model='ruleForm.phone'></el-input>
      </el-form-item>
      <el-form-item label='密码' prop='password'>
        <el-input v-model='ruleForm.password' type="password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          type='primary'
          class="login-btn"
          @click="onSubmit"
          :loading="isLoginLoading"
          >登录</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang='ts'>
import request from '@/utils/request'
import Vue from 'vue'
import qs from 'qs'
import { Form } from 'element-ui'

export default Vue.extend({
  name: 'LoginIndex',
  data () {
    return {
      ruleForm: {
        phone: '18201288771',
        password: '111111'
      },
      rules: {
        phone: [
          { required: true, message: '请输入手机号', trigger: 'blur' }
        ],
        password: []
      },
      isLoginLoading: false
    }
  },
  methods: {
    async onSubmit () {
      try {
        await (this.$refs.ruleForm as Form).validate()
        this.isLoginLoading = true
        const { data } = await request({
          method: 'POST',
          url: '/front/user/login',
          headers: { 'content-type': 'application/x-www-form-urlencoded' },
          data: qs.stringify(this.ruleForm) // axios 默认发送的是 application/json 格式的数据
        })
        if (data.state !== 1) {
          return this.$message.error(data.message)
        }
        this.$router.push({
          name: 'home'
        })
        this.$message.success('登录成功')
      } catch (err) {
        console.log(err)
      }

      this.isLoginLoading = false
    }
  }
})
</script>

<style lang='scss' scoped>
.login{
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  .login-ruleForm{
    background: #fff;
    padding: 20px;
    border-radius: 5px;
    width: 300px;
    .login-btn{
      width: 100%;
    }
  }
}
</style>
