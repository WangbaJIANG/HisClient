<template>
  <div>
      
      <div>
    患者信息：
    <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
  
   
    <el-divider/>
    开立处方
    <el-divider/>

    <el-tag size="large">处方金额：{{totalprice}}</el-tag>
    <el-table :data="tableData" style="width: 100%" @selection-change="SelectionChange" >
                <el-table-column type="selection" width="55" />
    <el-table-column prop="drugName" label="药品名称"   />
    <el-table-column prop="drugFormat" label="药品规格" width="100px"   />
    <el-table-column prop="drugUnit" label="用法用量" >
      <template #default="scope"><el-input placeholder="输入用法用量，频次等信息" v-model="scope.row.drugUsage"></el-input>
      </template>
      
           </el-table-column> 
        <el-table-column prop="drugUnit" label="数量" >
          <template #default="scope">
          <el-input-number  v-model="scope.row.drugNumber" :min="1" :max="10" @change="handleChange" /> 
         </template>
          </el-table-column> 
    <el-table-column prop="drugPrice" label="单价" width="70px"/>

      <el-table-column align="right">
      <template #header>
        <el-button type="primary" @click="centerDialogVisible = true" > 添加 </el-button>
        <el-button type="primary" @click="deleteTech">
          删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
 
  

<div  class="btnbox" > <el-button type="primary" @click="saveDrug"> 保存 </el-button>
        <el-button type="primary" @click="resetTableData"> 
          清空
        </el-button></div>



<el-dialog v-model="centerDialogVisible" title="Warning" width="900" center>
    <el-tag>药品名称：</el-tag>
    <el-input placeholder="请输入药品名称：" v-model="drugName"> </el-input>  
    <el-tag>拼音助记码：</el-tag>
    <el-input placeholder="请输入拼音助记码：" v-model="mnemonicCode"> </el-input>
  
    <el-table :data="drugList" style="width: 100%" @selection-change="dialogSelectionChange">
    <el-table-column type="selection" width="55"/>
  
    <el-table-column label="药品编码" prop="drugCode" />
      <el-table-column  label="药品名称"  prop="drugName"/>
    <el-table-column  label="药品规格"  width="150px" prop="drugFormat"/>
    <el-table-column  label="药品单价" prop="drugPrice" />
    <el-table-column  label="包装单位" prop="drugUnit" width="100px"/>
      <el-table-column  label="生产厂家" prop="manufacturer" width="100px"/>

      <el-table-column align="right">
       <template #header>
        <el-button @click="findDrug ">查询</el-button>
      </template>
    </el-table-column>
  </el-table>
  <template #footer>
        
        <div class="dialog-footer">
          <el-button @click="closeDialog">取消</el-button>
          <el-button type="primary" @click="addDrug">
            添加
          </el-button>
        </div>
      </template>
</el-dialog>    
   </div>
  </div>
</template>

<script setup>

import { onMounted,computed } from 'vue'
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import { getSessionStorage } from '../../../common'
import { ElMessage } from 'element-plus'
import axios from 'axios'
const tableData = ref([])
const centerDialogVisible = ref(false)
const drugList = ref([])
const patient=ref({})

const mnemonicCode=ref('')
const drugName=ref('')

const drugRequest=ref({
    registerId: "",
    drugId: "",
    drugNumber: 1,
    drugUsage: "",
    creationTime:new Date().toLocaleString()

})

const temptableData=ref([])
const tableTempData=ref([])
const doctor=ref("")
const totalprice=computed(()=>
{
    let total=0
    tableTempData.value.forEach((item)=>{
        total+=item.drugPrice
    })
    return total.toFixed(2)
}

)

onMounted(() => {
  let obj=getSessionStorage('patient') 
   if(obj){
    patient.value=obj
   }
})


function dialogSelectionChange(val){
  console.log(val)
  temptableData.value=val
}

function findDrug(){
  axios({
    method: 'get',
    params: {
     drugName:drugName.value,
     mnemonicCode:mnemonicCode.value, 
    } ,
    url:'http://localhost:8090/prescription/findDrugByNameAndCode'
  })
  .then(response => {
    console.log(response.data.data)
    drugList.value=response.data.data
  })
}
function closeDialog() {
  centerDialogVisible.value = false
  console.log(tableData.value+"nima")
   drugList.value = []
  
}
function addDrug(){
  closeDialog()
  temptableData.value.forEach((item)=>{
    tableData.value.push(item)
    

  })

}

function deleteTech(){
  tableTempData.value.forEach((item)=>{
    let index=tableData.value.indexOf(item)
    tableData.value.splice(index,1)
  })
}

function resetTableData() {
  tableData.value = [] 
}

function saveDrug(){
   console.log(tableTempData.value)
  const drugRequests = tableTempData.value.map(item => ({
    registerId: patient.value.id,
    drugId: item.id, // 请根据实际字段名调整
    drugNumber: item.drugNumber,
    drugUsage: item.drugUsage,
    creationTime: new Date().toISOString()
  }));
 axios.post('http://localhost:8090/prescription/saveDrugPrescription', drugRequests)
    .then(res => {
      ElMessage.success('保存成功');
      // 可选：清空选择
      tableTempData.value = [];
    })
    .catch(err => {
      ElMessage.error('保存失败');
    });
    deleteTech()
}
function SelectionChange(val){
  console.log(val)
  tableTempData.value=val 
}


</script>

<style scoped>

</style>