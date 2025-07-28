<template>
  <div>
    <div>
        <el-divider/>
        药房发药
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;" @click="findPatient">搜索 </el-button>
       </div>
   <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="患者病历号" width="180" property="caseNumber">
        </el-table-column>
        <el-table-column label="患者姓名" width="180" property="realName">
        </el-table-column>
         <el-table-column label="年龄" width="180" property="age">
        </el-table-column>
         <el-table-column label="性别" width="180" property="gender">
        </el-table-column>
  
  
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="findDrug(scope.$index, scope.row)">
          显示药品
        </el-button>
       
       
        
    </template>
     
    </el-table-column>
    
    <el-divider/>
  </el-table>
       
  <el-divider/>
   <el-table :data="tableDrugData" style="width: 100%;">
        <el-table-column type="index" :index="indexMethod"  width="60px"/>
        <el-table-column label="药品编码" width="180" property="drugCode">
        </el-table-column>
        <el-table-column label="药品名称" width="180" property="drugName">
        </el-table-column>
         <el-table-column label="药品规格" width="180" property="drugFormat">
        </el-table-column>
         <el-table-column label="包装单位" width="180" property="drugUnit">
        </el-table-column>
         <el-table-column label="生产产家" width="180" property="manufacturer">
        </el-table-column>
         <el-table-column label="单价" width="180" property="drugPrice">
        </el-table-column>
         <el-table-column label="药品数量" width="180" property="drugNumber">
        </el-table-column>
  
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="sendDrug(scope.$index, scope.row)" type="primary">
          发药
        </el-button>
       
       
        
    </template>
     
    </el-table-column>
    
    <el-divider/>
  </el-table>

    </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { onMounted } from 'vue';
import { getLocalStorage, setSessionStorage } from '../../common';
import { computed, ref } from 'vue'
import { ElMessage } from 'element-plus';
const pageNum=ref(1)
const pageSize=ref(0)
const total=ref(0)
const tableData=ref([])
const caseNumber=ref('')
const realName=ref('')
const tableDrugData=ref([])


function findPatient(){
  axios({
    method: 'get',
    params: {
      caseNumber: caseNumber.value,
      realName: realName.value,
     

    } ,
    url:'http://localhost:8090/prescription/findPatientWithDrug'
  }).then(res=>{
    if(res.data.status==200){
      tableData.value=res.data.data
      console.log(res.data.data)
      
    }else{
      ElMessage.error(res.data.message)
    }
  })
}

function findDrug(index,row){
  axios({
    method: 'get',
    params:{
      registerId:row.id,

    } ,
    url:'http://localhost:8090/prescription/findDrugWithRegisterId'
    
  }).then(response=>{
    if(response.data.status==200){
      tableDrugData.value=response.data.data
      console.log(response.data.data)  
    }
    
  })
  
  
}

function sendDrug(index,row){
 
  axios({
    method: 'post',
    params:{
      registerId:row.registerId,
      drugState:'已发',
      drugId:row.id,
    }, 
    url:"http://localhost:8090/prescription/sendDrug"

  }).then(res=>{
    if(res.data.status==200){
      ElMessage.success('发药成功')
      findPatient()
      tableDrugData.value=[]
    }else{
      ElMessage.error(res.data.message)
    }
  })
}

</script>


<style scoped>

</style>