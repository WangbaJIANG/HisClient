<template>
  <div>
      <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
  
   
    <el-divider/>
      检查结果查看
         <el-table :data="tableData" style="width: 100%" @row-click="rowClick"
      highlight-current-row>
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="检查编号" width="180" property="techCode">
        </el-table-column>
        <el-table-column label="检查名称" width="180" property="techName">
        </el-table-column>
         <el-table-column label="规格" width="180" property="techFormat">
        </el-table-column>
         <el-table-column label="单价" width="180" property="techPrice">
        </el-table-column>
         <el-table-column label="状态" width="180" property="checkState">
        </el-table-column>
         <el-table-column label="费用分类" width="180" property="techType">
        </el-table-column>

  

   
  </el-table>
   <el-divider/>
   
  <el-descriptions
    title="Vertical list with border"
    direction="vertical"
    :column="1"
    :size="small"
  
    border
  >
    <el-descriptions-item label="开立时间" width="80px">{{checkTableData.creationTime}}</el-descriptions-item>
    <el-descriptions-item label="检查医生" width="80%">{{doctorName}}</el-descriptions-item>
    <el-descriptions-item label="检查部位"  width="80%">{{checkTableData.checkPosition}}</el-descriptions-item>
    <el-descriptions-item label="目的要求" width="80%">{{checkTableData.checkInfo}}</el-descriptions-item>
    <el-descriptions-item label="医嘱备注">{{checkTableData.checkRemark}} </el-descriptions-item>
    <el-descriptions-item label="检查结果"> {{checkTableData.checkResult}}</el-descriptions-item>
     <el-descriptions-item label="检查时间" width="80%">{{checkTableData.checkTime}}</el-descriptions-item>

  </el-descriptions>

  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { getSessionStorage } from '../../../common';
import axios from 'axios';


const kooriookami=ref('123')
const patient=ref({})
const tableData=ref([])
const checkTableData=ref([])
const doctorName=ref('')
onMounted(() => {

let obj=getSessionStorage('patient')
if(obj){
  patient.value=obj
}
findCheck()

})

function findCheck(){
 axios({
    method: 'get',
    params: {
     registerId:patient.value.id

    },
    url:'http://localhost:8090/checkApply/findCheckListAll'
 }).then(response => {
    
    tableData.value=response.data.data 
    console.log(tableData.value)
 })
  
}
function rowClick(row){

 axios({
    method: 'get',
    params: {
     registerId:patient.value.id,
     techId:row.id
    },
    url:'http://localhost:8090/checkApply/findCheckRequestByRegisterIdAll'
 }).then(response => {
    console.log(response.data.data)
    checkTableData.value=response.data.data
    findDoctor()
   
 })


}
function findDoctor(){
    alert(123)
 axios({
    method: 'get',
    params: {
    empId:Number(checkTableData.value.checkEmployeeId)
    }, 
    url:'http://localhost:8090/employee/findEmpByEmpId'
 })
 .then(response => {
    doctorName.value=response.data.data.realname
 }) 
}
</script>

<style scoped>

</style>