<template>
  <el-form
    ref="form"
    :rules="rules"
    :model="form"
    label-width="80px"
    class="form"
  >
    <h3 class="title">欢迎登陆</h3>
    <el-form-item label="用户名" prop="name">
      <el-input v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="pass">
      <el-input
        type="password"
        v-model="form.pass"
        autocomplete="off"
      ></el-input>
    </el-form-item>
    <el-form-item align="right">
      <el-button type="primary" @click="onSubmit('form')">登录</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
export default {
  data() {
    return {
      form: {
        name: "",
        password: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入名字", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        pass: [
          { required: true, message: "密码不能为空" },
          { type: "number", message: "密码必须为数字值" }
        ]
      }
    };
  },
  methods: {
    onSubmit(formName) {
      this.$refs[formName].resetFields();
      console.log("submit!");
    }
  }
};
</script>
<style scoped>
.title {
  margin-top: 30px;
  margin-left: 28px;
  margin-bottom: 30px;
}
.form {
  height: 200px;
  width: 300px;
  margin: 30px auto;
  /* border: 1px solid ; */
}
</style>
