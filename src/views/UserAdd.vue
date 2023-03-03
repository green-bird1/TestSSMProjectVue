<template>
  <div class="app-container">
    <el-form ref="form" :model="user" label-width="120px">
      <el-form-item label="用户名">
        <el-input v-model="user.username" />
      </el-form-item>
      <el-form-item label="密码">
        <el-input type="password" v-model="user.password" />
      </el-form-item>
      <el-form-item label="确认密码">
        <el-input type="password" v-model="password2" @change="checkPass"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary"  @click="onSubmit">新增用户</el-button>
        <el-button @click="onCancel">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import request from '@/utils/request'
export default {
  data() {
    return {
      user: {
        username: '',
        password: ''
      },
      password2: ''
    }
  },
  methods: {
    onSubmit() {
      // 请求接口 实现用户新增接口的调用
      request({
        url:"http://127.0.0.1:8080/api/user/save.do",
        method: "POST",
        data:this.user
      }).then(res=>{
        if(res.code==0){
          //跳转到列表页面
          this.$router.push("/userlist");
        }
      })
    },
    checkPass() {},
    onCancel() {
      this.$message({
        message: '取消用户的新增',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
</style>

