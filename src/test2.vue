<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref } from 'vue'

</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-header class="header">
        <div style="margin-left: 50px;">东软云医院HIS</div>
        <div style="margin-right: 50px;" class="header_right">Neuedu  
          <el-dropdown>
            
      <span class="el-dropdown-link">
      扁鹊
      <el-icon class="el-icon--right">
        <arrow-down />
      </el-icon>
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item>个人信息</el-dropdown-item>
        <el-dropdown-item>退出登录</el-dropdown-item>
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
  >
    <el-sub-menu index="1">
      
      <template #title> <el-icon :size="20">
    <Edit />
  </el-icon>挂号收费</template>
      <el-menu-item index="1-1"><router-link to="./test"></router-link> 窗口挂号</el-menu-item>
      <el-menu-item index="1-2">窗口退号</el-menu-item>
      <el-menu-item index="1-3">窗口收费</el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="2">
      <template #title><el-icon><User /></el-icon>门诊医生</template>
      <el-menu-item index="1-1">患者查看</el-menu-item>
      <el-menu-item index="1-2">病历首页</el-menu-item>
      <el-menu-item index="1-3">检查申请</el-menu-item>
      </el-sub-menu>
      <el-sub-menu index="3">
      <template #title><el-icon><Camera /></el-icon>检查医生</template>
      <el-menu-item index="1-1">检查申请</el-menu-item>
      <el-menu-item index="1-2">病历首页</el-menu-item>
      <el-menu-item index="1-3">检查申请</el-menu-item>
      </el-sub-menu>
     
     
  
   
   

  </el-menu></el-aside>
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
