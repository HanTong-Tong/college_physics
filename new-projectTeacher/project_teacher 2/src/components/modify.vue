<template>
<!-- 边框 -->
  <div class="box">
    <!-- 表单 -->
    <el-form 
      :model="ruleForm" 
      status-icon 
      :rules="rules" 
      ref="ruleForm" 
      label-width="100px" 
      class="demo-ruleForm">
      <el-form-item label="账户名" prop="username">
      <el-input v-model="ruleForm.username" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="pass">
      <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="确认密码" prop="password">
      <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button  style="display:block;margin:0 auto" type="primary" @click="submitForm('ruleForm')">提交</el-button>
    </el-form-item>
    </el-form>
  </div>
</template>

<script>
  export default {
    data() {
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码,密码由长度为8-16位的数字、字母（大写或小写）和特殊字符（~!@#$%^&*()_+`\-={}:<>,.\/）组成'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('请注意两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          username:'',
          pass: '',
          password: '',
        },
        rules: {
          username:[{required:true,message:"请注意账户名不能为空",trigger:"blur"}],
          pass: [
            { validator: validatePass, trigger: 'blur' },
            { 
            required: true, 
            pattern:/^(?=.*[a-zA-Z])(?=.*\d)(?=.*[~!@#$%^&*()_+`\-={}:;<>,.\/]).{8,16}/,
            message: "请注意密码由长度为8-16位的数字、字母（大写或小写）和特殊字符（~!@#$%^&*()_+`\-={}:<>,.\/）组成", 
            trigger:"blur"
            }
          ],
          password: [
            { validator: validatePass2, trigger: 'blur' },
            { required: true }
          ],
        }
      };
    },
    methods: {
      //提交按钮交互
      async submitForm(formName) {
        this.$refs[formName].validate((valid) => {
        if (valid) {
        const { data:res } = this.$http.post('/api/change',{
          username: this.ruleForm.username,
          password: this.ruleForm.password
        })
        console.log(res);
            alert('修改密码成功!');
            this.$router.push('/login');
        } else {
            console.log('error submit!!');
            alert('修改失败,请注意密码由长度为8-16位的数字、字母（大写或小写）和特殊字符（~!@#$%^&*()_+`\-={}:<>,.\/）组成!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>

<style scoped>
  .box {
    width:65%;
    height: 45%;
    border:2px solid white;
    border-radius:20px;
    padding: 100px;
    box-shadow:5px 5px 60px 10px #e3ecec;
    margin: 35px auto;
  }
</style>