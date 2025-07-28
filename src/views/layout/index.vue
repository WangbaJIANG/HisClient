<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router';
import { getLocalStorage, getSessionStorage, removeLocalStorage } from '../../common';
import { onMounted } from 'vue';
const user=ref({})
const router=useRouter();
let menulist=ref([])
let activeIndex=ref('')

onMounted(()=>{
  
  let emp=getLocalStorage('user')
  if(emp){
    user.value=emp;
    console.log(user.value)
    menulist.value=user.value.permitList;
    

  }

})
function logout(){
    removeLocalStorage('user')
  router.push('/login')
}
</script> 

<template>
  <div class="common-layout">
    <el-container>
      <el-header class="header">
        <div style="margin-left: 50px;">东软云医院HIS</div>
        <div style="margin-right: 50px;" class="header_right">Neuedu  
          <el-dropdown>
            
      <span class="el-dropdown-link">
      {{user.realname}}
      <el-icon class="el-icon--right">
        <arrow-down />
      </el-icon>
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item >个人信息</el-dropdown-item>
        <el-dropdown-item @click="logout">退出登录</el-dropdown-item>
    </el-dropdown-menu>
    </template>
  </el-dropdown></div>
        
      </el-header>
      <el-container>
        <el-aside class="aside"> 
          <el-menu
    :default-active="activeIndex"
    class="el-menu-demo"
    unique-opened="true"
    router="true"
  >
    <el-sub-menu :index="index+''"  v-for="(menu,index) of menulist" :key="index" >
      
      <template #title> 
        <el-icon :size="20">
        <component :is="menu.permitIcon"></component>
  </el-icon> {{ menu.permitName }}</template>
      <el-menu-item :index="child.permitPath" v-for="(child,index) of menu.children" :key="index">{{ child.permitName }}</el-menu-item>
          </el-sub-menu>
     </el-menu>
        </el-aside>
        <el-main>  <router-view></router-view></el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style scoped>
.header{
  width:"100%";
  background-color:rgb(67, 155, 255);
  height: 50px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: white;
  font-size: 20px;
 
 
}

.aside{

height: 600px;
background-color: aliceblue;}
.el-dropdown{

}
.el-dropdown-link{
  display: inline-flex;
  align-items: center;
color: white;
margin-left: 10px;
outline: none;
}
.header_right{
  display: flex;
  align-items: center;
  gap: 10px;
 
}
</style>
