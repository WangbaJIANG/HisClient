<template>
   <div>
    <el-tag type="success">今日已检查{{finishPatientCount}}项</el-tag>
    <el-tag type="warning">当前有{{waitPatientCount}}人在排队</el-tag>
    <el-divider/>
    <h3>检查申请</h3>
    <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
    <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>

    <el-button type="default" @click="search">搜索</el-button>
    <el-divider/>
    <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="患者姓名" width="180" property="realName">
        </el-table-column>
    <el-table-column label="患者病历号" width="180" property="caseNumber">
    </el-table-column>
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="createCheck(scope.$index, scope.row)">
          进行检查
        </el-button>
        <el-button size="small" @click="skip(scope.$index, scope.row)">
          跳过
        </el-button>
        <el-button size="small" @click="handleEdit(scope.$index, scope.row)">
          叫号
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
    
  
</template>

<script setup>
import axios from 'axios';
import { onMounted } from 'vue';
import { getLocalStorage, setSessionStorage } from '../../common';
import { computed, ref } from 'vue'
import { ElMessage } from 'element-plus';
import { useRouter } from 'vue-router';

let emp=getLocalStorage('user')
const waitPatientCount=ref(0)
const finishPatientCount=ref(0)
const caseNumber=ref('')
const realName=ref('')
const pageNum=ref(1)
const pageSize=ref(3)
const total=ref(0)
const router=useRouter()
const tableData = ref([]);
onMounted(() => {
axios({
    method: 'get',
    url: 'http://localhost:8090/checkApply/findfinishPatientCount',
    params: {
     state:'执行完成'
    }
  })
  .then(response => {
    console.log(response.data.data)
    finishPatientCount.value = response.data.data;
   
  })

  axios({
    method: 'get',
    url: 'http://localhost:8090/checkApply/findfinishPatientCount',
    params: {
       state:'已缴费'
    }
  })
  .then(response => {
    console.log(response.data.data)
    waitPatientCount.value = response.data.data;
   
  })

  findPatient()

});

function findPatient(row){
  //清空数据
    tableData.value = [];
    axios({
        method: 'get',
        url: 'http://localhost:8090/checkApply/findPatient',
        params: {
            pageNum: pageNum.value,
            pageSize: pageSize.value,
            state:'已缴费',
            caseNumber:caseNumber.value,
            realName:realName.value

        }
    })
    .then(response => {
        console.log(response.data.data)
        tableData.value = response.data.data.list;
        total.value = response.data.data.total;
        console.log()
        
     
    })

}
function search(){
    findPatient();
    caseNumber.value=''
    realName.value=''   
}


function createCheck(index,row){
    setSessionStorage('patient',row)
    router.push('/check_patient')

}
function skip(index,row){
    console.log(row)
}
function sizeChange() {
 
    findPatient()
  }
  function currentChange() {
    findPatient() 
  }
</script>

<style scoped>

</style>