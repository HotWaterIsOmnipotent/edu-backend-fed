<template>
  <div class="login">
    <!--
      1. :model="ruleForm"
      2. :rules="rules"
      3. ref="ruleForm"
      4. el-form-item 绑定 prop 属性
     -->
    <div>
      <!-- <img class="logo" src="@/assets/logo.png" alt="EduBoss"> -->
    </div>
    <el-form
      class="login-form"
      label-position="top"
      ref="form"
      :rules="rules"
      :model="form"
      label-width="80px"
      size="medium"
    >
      <h2 class="form-title">系统登录</h2>
      <el-form-item label="手机号" prop="phone">
        <el-input v-model="form.phone"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          class="login-btn"
          type="primary"
          :loading="isLoading"
          @click="onSubmit"
        >登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { login } from '@/services/user'
import { Form } from 'element-ui'

export default Vue.extend({
  name: 'Login',
  data () {
    return {
      form: {
        phone: '18201288771',
        password: '111111'
      },
      rules: {
        phone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { pattern: /^1\d{10}$/, message: '请输入正确的手机号', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 18, message: '长度在 6 到 18 个字符', trigger: 'blur' }
        ]
      },
      isLoading: false
    }
  },

  methods: {
    async onSubmit () {
      try {
        // 1. 表单验证
        await (this.$refs.form as Form).validate()

        this.isLoading = true

        // 2. 验证通过 => 提交表单
        const { data } = await login(this.form)
        // 3. 处理请求结果
        //    失败：给出提示
        if (data.state !== 1) {
          this.$message.error(data.message)
        } else {
          // 成功：跳转首页
          // 存储用户数据
          this.$store.commit('setUser', data.content)
          // 跳转到上一次的路由或'/'
          this.$router.push(this.$route.query.redirect as string || '/')
          this.$message.success('登录成功')
        }
      } catch (error) {
        console.log('登录失败', error)
      }
      this.isLoading = false
    }
  }
})
</script>

<style lang='scss' scoped>

.login {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  .logo {
    width: 100px;
    margin: 20px;
    margin-top: -100px;
  }
  .login-form {
    width: 300px;
    background: #fff;
    padding: 30px;
    border-radius: 5px;
    .form-title {
      margin: 0 0 1rem;
      font-weight: 400;
      font-size: 1.5rem;
    }
  }
  .login-btn {
    width: 100%;
  }
}
</style>
