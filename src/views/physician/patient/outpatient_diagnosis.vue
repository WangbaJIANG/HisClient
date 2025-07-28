<template>
  <div>

    <div>
        <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
  
   
    </div>



      门诊确诊



      <div>
        <el-descriptions
  title="医嘱" :border="true" style="width: 100%;margin-top :30px"  column="1">
    <el-descriptions-item label="诊断结果" class="lable">
 <el-input
    v-model="diagnosis"
    style="width: 100%"
    :rows="2"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="处理意见">
       <el-input
    v-model="cure"
    style="width: 100%"
    :rows="2"
    type="textarea"
    placeholder="Please input"
  /></el-descriptions-item>
 
 
   
    
</el-descriptions>
<div  class="btnbox" > <el-button type="primary" @click="saveDiagnosis"> 保存 </el-button>
        <el-button type="primary" @click="resetTableData"> 
          清空
        </el-button></div>
      </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ElMessage } from 'element-plus';
import { el } from 'element-plus/es/locales.mjs';
import moment from 'moment';
import { computed, onMounted, ref } from 'vue'   

onMounted(() => {
  const obj = JSON.parse(sessionStorage.getItem('patient'));  
  if (obj) {
    patient.value = obj;
    console.log(patient.value); 
  } 
})
const patient = ref({
})
const cure = ref(''); 
const diagnosis = ref(''); 


function resetTableData() {
  cure.value = '';
  diagnosis.value = '';
}

function saveDiagnosis() {

  axios({
    method: 'post',
    params: {
      diagnosis: diagnosis.value,
      cure: cure.value,
      registerId: patient.value.id,
     
      
    } ,
    url:'http://localhost:8090/physician/saveDiagnosis'
  })
  .then(response => {
    if (response.data.status === 200) {
      ElMessage.success('保存成功');
    }
    else {
      ElMessage.error(response.data.message);
    }
  })
}

</script>

<style scoped>
.btnbox{
    margin-top: 30px;
 
}
</style>