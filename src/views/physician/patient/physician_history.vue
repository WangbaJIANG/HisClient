<template>
   <div>
        <el-divider/>
        看诊记录
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;" @click="findPatientByempId1">搜索 </el-button>
       </div>
 


        <el-divider/>
        <div class="table1">
             <el-table :data="tableData" style="width: 100%;">
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="患者病历号" width="180" property="caseNumber">
        </el-table-column>
        <el-table-column label="患者姓名" width="120" property="realName">
        </el-table-column>
         <el-table-column label="患者状态" width="100" property="visitState"  :formatter="formatVisitState">
        </el-table-column>
         <el-table-column label="身份证号" width="180" property="cardNumber">
        </el-table-column>
         <el-table-column label="挂号时间" width="180" property="visitDate" :formatter="dateformat">
        </el-table-column>
  

          <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="choosePatient(scope.$index, scope.row)">
          更新病历
        </el-button>
       
        
    </template>
     
    </el-table-column>
    <el-divider/>
  </el-table>
    <el-pagination 
          v-model:current-page="pageNum" 
           v-model:page-size="pageSize"
          :page-sizes="[2,3,4]" 
          background 
          layout="total, sizes, prev, pager, next, jumper" 
          :total="total" 
          @current-change="currentChange" 
          @size-change="sizeChange"
          style="margin-top: 20px;"
        />

        </div>
    </div>
</template>

<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import { getLocalStorage, setSessionStorage } from '../../../common'
import { ElMessage } from 'element-plus'
import moment from 'moment'

import { useRouter } from 'vue-router'
const router=useRouter()
const pageNum=ref(1)
const pageSize=ref(1)
const total=ref(0)
const caseNumber=ref('')
const realName=ref('')
const employeeId=ref('')
const tableData=ref([])
const checkPatient=ref({})
var user=ref('')

onMounted(()=>{
 let obj=getLocalStorage('user') 
if(!obj){
  ElMessage.error('请先登录')
 return;
}
user.value=obj
findPatientByempId1()
}

)
function formatVisitState(row, column, cellValue) {
  if (cellValue === 1) return '未接诊'
  if (cellValue === 2) return '医生接诊'
  return '未知状态'
}
function dateformat(row, column, cellValue) {
    return moment(row.creationTime).format('YYYY-MM-DD HH:mm:ss')
  
}
function findPatientByempId1() {

  axios({
    method: 'get',
    url: 'http://localhost:8090/register/findPatientByempIdAndState',
    params: {
      caseNumber: caseNumber.value,
      realName: realName.value,
      pageNum: pageNum.value,
      pageSize: pageSize.value,
      employeeId:user.value.id
    }
  }).then(response=>{
    console.log()
    tableData.value=response.data.data.list
    console.log(response.data.data)
    total.value=response.data.data.total

  })

}

function choosePatient(index,row){

setSessionStorage('checkPatient',checkPatient.row)
 router.push('/home_medical_record')
setSessionStorage('patient',row)
}
</script>

<style scoped>

</style>