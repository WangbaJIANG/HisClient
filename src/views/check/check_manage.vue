<template>
  <div>
    <div>
        <el-divider/>
      检查管理
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;" @click="findPatient">搜索 </el-button>
       </div>
   <el-table :data="tableData" style="width: 100%"  @row-click="rowClick"
      highlight-current-row>
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="患者病历号" width="180" property="caseNumber">
        </el-table-column>
        <el-table-column label="患者姓名" width="180" property="realName">
        </el-table-column>
         <el-table-column label="患者状态" width="180" property="gender">
        </el-table-column>
         <el-table-column label="身份证号" width="180" property="cardNumber">
        </el-table-column>
         <el-table-column label="挂号时间" width="180" property="creationTime" :formatter="dateformat">
        </el-table-column>
  
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="registRecord(scope.$index, scope.row)">
          检查项目
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
  <el-divider/>
   <el-table :data="tableCheckData" style="width: 100%;">
        <el-table-column type="index" :index="indexMethod"  width="60px"/>
       
        <el-table-column label="项目名称" width="180" property="techName">
        </el-table-column>
        
    
         
         <el-table-column label="项目编码" width="180" property="techCode">
        </el-table-column>
        <el-table-column label="类型" width="180" property="techType">
        </el-table-column>
         <el-table-column label="单价" width="180" property="techPrice">
        </el-table-column>
             <el-table-column label="状态" width="180" property="checkState">
        </el-table-column>
        
         <el-table-column label="开立时间" width="180" property="checkTime" :formatter="dateformat">
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
import moment from 'moment';
const pageNum=ref(1)
const pageSize=ref(0)
const total=ref(0)
const tableData = ref([]);
const caseNumber=ref('')
const realName=ref('')
const clickRow=ref({})
const clickRowId=ref('')
const tableCheckData=ref([])
onMounted(() => {
    findPatient() 
    
})

function findPatient(){
  //清空数据
    tableData.value = [];
    axios({
        method: 'get',
        url: 'http://localhost:8090/checkInput/findInputPatient',
        params: {
         
          
            caseNumber:caseNumber.value,
            realName:realName.value

        }
    })
    .then(response => {
        console.log(response.data.data)
        tableData.value = response.data.data;
       
        
        
     
    })

}
function dateformat(row, column, cellValue) {
    return moment(row.checkTime).format('YYYY-MM-DD HH:mm:ss')
  
}
function rowClick(row, event, column) {
    clickRow.value=row
    clickRowId.value=row.id
    findCheck()
}

function findCheck() {
    axios({
        method: 'get',
        params: {
            registerId:clickRowId.value
        } ,
        url:'http://localhost:8090/checkApply/findCheckListAll'
    }).then(response => {
        console.log(response.data.data)
        tableCheckData.value=response.data.data
    })

}
function registRecord(index, row) {
   rowClick()
}
</script>

<style scoped>

</style>