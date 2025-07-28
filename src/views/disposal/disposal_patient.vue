<template>
  <div>

    患者信息：
    <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
  
   

  
  
   
    <el-divider/>
      检查项目

      <el-table :data="tableData" style="width: 100%">
      <el-table-column type="index" label="编号" />
    <el-table-column prop="techCode" label="检查编码" />
    <el-table-column prop="techName" label="检查名称"  />
    <el-table-column prop="techType" label="检查规格" />    
    <el-table-column prop="techPrice" label="单价" width="70px"/>
    <el-table-column prop="techFormat" label="检查分类"  />
    
  </el-table>
  <el-tag type="primary">已选择的检查项目：</el-tag>  
 
  <el-descriptions title="确认检查科室和检查医生">
    <el-descriptions-item label="科室选择">
      <el-select
      v-model="departmentSelect"
      placeholder="Select"
      size="large"
      style="width: 240px"
    >
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item label="医生选择">
      <el-select
      v-model="doctorSelect"
      placeholder="Select"
      size="large"
      style="width: 240px"
    >
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
    </el-descriptions-item>
  
  </el-descriptions>
  <el-button @click="findCheck">开始检查</el-button>
  <el-button @click="findCheck">重置医生</el-button>
  </div>
</template>

<script setup>
import {onMounted, ref} from "vue";
import { getSessionStorage } from "../../common";
import { ElMessage } from "element-plus";
import axios from "axios";
const patient = ref({}); // 患者信息
const doctorSelect = ref(""); // 选择的医生
const departmentSelect = ref(""); // 选择的科室
const tableData = ref([]); // 检查项目数据
onMounted(() => {
  // Initialize the patient data from session storage
  const obj =getSessionStorage("checkRegsiter")
  if (obj) {
    patient.value = obj;
  } else {
   ElMessage.error("没有患者信息，请先选择患者！");
  }
  findCheck();
});


function findCheck() {
   axios({
        method: 'get',
        url: 'http://localhost:8090/checkApply/findCheckList',
        params: {
         registerId:patient.value.id 
        }
    })
    .then(response => {
        console.log(response.data.data)
        tableData.value = response.data.data;
        console.log(tableData.value)
        console.log(patient.value.id)
        
        
     
    })

}


</script>

<style scoped>

</style>