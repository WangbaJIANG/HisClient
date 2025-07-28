
<script setup>
import axios from 'axios';
import { ElMessage } from 'element-plus';
import { onMounted, ref } from 'vue';
import { getLocalStorage, setSessionStorage,setLocalStorage } from '../../common';
import { useRouter } from 'vue-router';


const router=useRouter();
let realname = ref('');
let password = ref('');
let user = ref('');



function login() {
    if (realname.value === '' || password.value === '') {
        ElMessage.error('用户名和密码不能为空');
        return;
    }

  // Perform login logic here
 axios({
    method: 'post',
    url: 'http://localhost:8090/login/login',
    params: {
      realname: realname.value,
      password: password.value
    }
  })
  .then(response => {
   if (response.data.status == 200) {
    
      setLocalStorage('user', response.data.data);
      user.value = response.data.data;
      ElMessage.success('登录成功');
      router.push('/'); // Redirect to home page after successful login
    } else {
      ElMessage.error('用户名或密码错误');
    }
    // Handle successful login here (e.g., redirect to another page)
  })
  .catch(error => {
   ElMessage.error('登录失败，请检查用户名和密码');
    // Handle login failure here (e.g., show an error message)
  });
}

</script>

<template>
  <div class="form">
    <el-input v-model="realname" placeholder="请输入账号"></el-input>
    <el-input v-model="password"  placeholder="请输入密码"></el-input>
   <div class="dtn"> <el-button type="primary" @click="login">登录</el-button>
    <el-button type="primary">重置</el-button></div>
  </div>

</template>


<style>

.form {
  width: 300px;
  margin: 100px auto;
  display: flex;
  flex-direction: column;
    justify-items: center;

  gap: 20px;
}
.dtn{
  display: flex;
  justify-content: space-around; 
}

</style>