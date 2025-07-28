<template>
  <div>
     <div>
        <el-divider/>
          检查结果录入
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;" @click="findPatient">搜索 </el-button>
       </div>
   <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
         <el-table-column label="患者姓名" width="180" property="realName">
        </el-table-column>
        <el-table-column label="患者病历号" width="180" property="caseNumber">
        </el-table-column>
         <el-table-column label="性别" width="180" property="gender">
        </el-table-column>
         <el-table-column label="年龄" width="180" property="age">
        </el-table-column>
       
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="findCheck(scope.$index, scope.row)">
         已完成的检查
        </el-button>
       
        
    </template>
     
    </el-table-column>
    <el-divider/>
  </el-table>

  <el-table :data="tableCheckData" style="width: 100%" @row-click="rowClick"
      highlight-current-row>
   
         <el-table-column label="检查编码" width="180" property="techCode">
        </el-table-column>
        <el-table-column label="检查名称" width="180" property="techName">
        </el-table-column>
         <el-table-column label="检查规格" width="180" property="techType">
        </el-table-column>
         <el-table-column label="单价" width="180" property="techPrice">
        </el-table-column>
          <el-table-column label="费用分类" width="180" property="techFormat">
        </el-table-column>
       
       
  

    <el-divider/>
  </el-table>
  <el-tag type="primary">已选择的检查项目：{{ clickRowTechName }}</el-tag>  


   <el-descriptions title="确认录入检查科室和检查医生">
    <el-descriptions-item label="科室选择" >
      <el-select
      v-model="departmentSelect"
      placeholder="Select"
      size="large"
      style="width: 240px"
      @change="findDoctorByDeptId" 
    >
      <el-option
        v-for="item in departmentList"
        :key="item.id"
        :label="item.deptName"
        :value="item.deptName"
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
        v-for="item in doctorList"
        :key="item.value"
        :label="item.realname"
        :value="item.id"
      />
    </el-select>
    </el-descriptions-item>
  
  </el-descriptions>

 <el-descriptions title="输入检查结果" >
 <el-descriptions-item>
    <el-input type="textarea" placeholder="输入检查结果" v-model="checkResult" :rows="1" />
 </el-descriptions-item>
  
  </el-descriptions>

    <el-button @click="checkInput" type="primary">开始检查</el-button>
  <el-button @click="findCheck" type="primary">重置医生</el-button>

    </div>
  </div>

  
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';

const caseNumber = ref(''); // 患者病历号
const realName = ref(''); // 患者姓名
const tableData = ref([]); // 表格数据
const pageNum = ref(1); // 当前页码
const pageSize = ref(10); // 每页显示数量
const total = ref(0); // 总记录数
const tableCheckData=ref([])
const clickRowTechName=ref('')
let clickRow=ref({})
const departmentSelect = ref(''); // 选择的科室
const doctorSelect = ref(''); // 选择的医生
const departmentList = ref([]); // 科室列表
const doctorList = ref([]); // 医生列表
const checkResult = ref(''); // 检查结果
const registerId=ref(0)
function findPatient() {
     axios({
        method: 'get',
       url: 'http://localhost:8090/checkInput/findInputPatient',
       params: {
       
           checkState:'执行完成',
           caseNumber: caseNumber.value, // 传递患者病历号
           realName: realName.value, // 传递患者姓名 
       }

     }) 
     .then(response => {
        console.log(response.data.data)
        tableData.value = response.data.data; // 更新表格数据
        total.value = response.data.total; // 更新总记录数 
     })
}


function findCheck(index,row) {
console.log(row+'nihaos')
if(row){
  registerId.value=row.id
}

axios({
    method: 'get',
       url: 'http://localhost:8090/checkInput/findInputCheck',
       params: {
        registerId:registerId.value
       }

     }).then(response => {
        console.log(response.data.data)
        console.log(row)
        tableCheckData.value = response.data.data; // 更新表格数据
     })

  
}

function rowClick(row, event, column) {
 
  clickRowTechName.value=row.techName
  clickRow.value=row
  console.log(row.id+"zheshi")
  
  findDeptByTechId()
  }


function findDeptByTechId() {
  axios({
    method: 'get',
    url:'http://localhost:8090/checkPatient/findCheckDepartmentByTechType' ,
    params: {
      techType:clickRow.value.techType
    }
  }) .
  then(response => {
    console.log(response.data.data)
    departmentList.value=response.data.data
    console.log(departmentList.value)
  })
}
function findDoctorByDeptId() {

  axios({
    method: 'get',
    url:'http://localhost:8090/checkPatient/findDoctorByDeptId' ,
    params: {
      deptName:departmentSelect.value
     }
  }).then(response => {
    console.log(response.data.data)
    doctorList.value=response.data.data

  })
}
function checkInput(){
  console.log(registerId.value)
  axios({
    method: 'post',
    params: {
     id:registerId.value,
     techId:clickRow.value.id,
     inputcheckEmployeeId:doctorSelect.value,
     checkResult:checkResult.value, 
     checkState:'已结束'
  
    } ,
      url:'http://localhost:8090/checkPatient/checkPatient' ,

  }).then(response => {
    console.log(response.data.data)
    findPatient()
   findCheck()
     
  })
}
</script>

<style scoped>

</style>