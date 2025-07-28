<template>
    <div>
       
    <el-divider/>
    窗口收费
    <el-divider/>
    <h1>患者信息查询</h1>
    <el-descriptions>
        <el-descriptions-item label="病历号" width="40%" style=""> 
            <el-input
                v-model="caseNumber"
                style="width: 200px"
                :rows="1"

                placeholder="Please input"
            />
            </el-descriptions-item>   
             <el-descriptions-item label="患者名" width="40%" >
            <el-input
                v-model="realName"
                style="width: 200px"
                :rows="1"

                placeholder="Please input"
            />
            </el-descriptions-item>  
          <el-descriptions-item label="患者名" width="40%" >
              <el-button type="primary" @click="findPatientByCondition">查询</el-button>
            </el-descriptions-item>  
   
            

    </el-descriptions>


    <el-divider/>
    患者信息确认
    <div class="patient-info">
     <el-tag type="primary" class="taginfo" size="large">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary" class="taginfo" size="large">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary" class="taginfo" size="large">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary" class="taginfo" size="large">性别：{{ patient.gender }}</el-tag>  
  
    </div>




      
         <el-button type="primary" disabled>项目金额:{{ totalprice }}</el-button>
        <el-button type="primary" @click="expenseCharge">收费结算</el-button>
        <el-table :data="tableData" style="width: 100%"  @selection-change="SelectionChange">
       <el-table-column type="selection" width="55" />
        <el-table-column label="项目名称" width="180" property="techName">
        </el-table-column>

         <el-table-column label="单价" width="180" property="techPrice">
        </el-table-column>
         <el-table-column label="类型" width="180" property="techType">
        </el-table-column>
                 <el-table-column label="规格" width="180" property="">
        </el-table-column>

         <el-table-column label="数量" width="180" property="drugNumber">
        </el-table-column>  
        <el-table-column label="开立时间" width="180" property="creationTime" :formatter="dateFormat">
        </el-table-column>

  
    <el-divider/>
  </el-table>



    </div>
</template>

<script setup>
import axios from 'axios';
import { ElMessage } from 'element-plus';
import moment from 'moment';
import { computed, ref } from 'vue'   

const realName = ref('')
const caseNumber = ref('')
const patient = ref({
    realName: '',
    caseNumber: '',
    age: 0,
    gender: '',
    id: 0,
})
const requestData = ref({})
const ids = ref([])
const totalprice = computed(() => {
 
    let total = 0
    for (let i = 0; i < tempData.value.length; i++) {
        total += parseFloat(tempData.value[i].techPrice)
     
    }
    return total.toFixed(2)
})

let tempData= ref({})

function dateFormat(row,col){
    return moment(row.creationTime).format('YYYY-MM-DD HH:mm:ss')
    
}


function SelectionChange(selection) {
   tempData.value = selection
 
}


function expenseCharge() {
     requestData.value=tempData.value
    
    axios(
        {
            url: 'http://localhost:8090/expenseCharge/expenseCharge',
            method: 'post',
            data:  tempData.value,
               
            
        }
    ).then((response) => {
        if (response.data.status == 200) {
            ElMessage.success('收费成功')
             findPatientByCondition()
               requestData.value ={}
        } else {
            ElMessage.error('收费失败')
        }
    })
  findPatientByCondition()
   
}
const tableData = ref([])

function findPatientByCondition() {
axios(
    {
        url: 'http://localhost:8090/expenseCharge/findPatientByCondition',
        method: 'get',
        params: {
            realName: realName.value,
            caseNumber:caseNumber.value,
        }
    
    }
).then((response) => {
        if (response.data.status == 200) {
            patient.value = response.data.data
            tableData.value = response.data.data.voList
           
          
        } else {
            ElMessage.error('查询失败')
        }
    })
} 

</script>

<style lang="scss" scoped>

</style>