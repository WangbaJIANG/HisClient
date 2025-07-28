<template>
   <div>
    患者信息：
    <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
  
   
    <el-divider/>
    处置申请
    <el-divider/>

    <el-tag size="large">项目金额：{{totalprice}}</el-tag>
    <el-table :data="tableData" style="width: 100%" @selection-change="SelectionChange" >
                <el-table-column type="selection" width="55" />
    <el-table-column prop="techCode" label="检查编码" />
    <el-table-column prop="techName" label="检查名称"  />
    <el-table-column prop="techType" label="检查规格" />    
    <el-table-column prop="techPrice" label="单价" width="70px"/>
    <el-table-column prop="techFormat" label="检查分类"  />
      <el-table-column align="right">
      <template #header>
        <el-button type="primary" @click="centerDialogVisible = true"> 添加 </el-button>
        <el-button type="primary" @click="deleteTech">
          删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
 
  
<el-descriptions
  title="医嘱" :border="true" style="width: 100%;margin-top :30px"  column="1">
    <el-descriptions-item label="目的要求" class="lable">
 <el-input
    v-model="checkRequest.checkInfo"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="检验部位">
       <el-input
    v-model="checkRequest.checkPosition"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  /></el-descriptions-item>
 
   
    <el-descriptions-item label="备注">
        <el-input
    v-model="checkRequest.checkRemark"
    style="width: 100%"
    :rows="1    "
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    
</el-descriptions>
<div  class="btnbox" > <el-button type="primary" @click="saveCheckRequest"> 保存 </el-button>
        <el-button type="primary" @click="resetTableData"> 
          清空
        </el-button></div>



<el-dialog v-model="centerDialogVisible" title="Warning" width="700" center>
    <el-tag>检查编码：</el-tag>
    <el-input placeholder="请输入检查编码" v-model="check.techCode"> </el-input>  
    <el-tag>检查名称：</el-tag>
    <el-input placeholder="请输入检查名称" v-model="check.techName"> </el-input>
    <el-tag>检查规格：</el-tag>
    <el-input placeholder="请输入检查规格" v-model="check.techType"> </el-input>
    <el-table :data="checkList" style="width: 100%" @selection-change="dialogSelectionChange">
    <el-table-column type="selection" width="55"/>
    <el-table-column  label="检查名称"  prop="techName"/>
    <el-table-column label="检查编码" prop="techCode" />
    <el-table-column  label="检查规格"  width="150px" prop="techType"/>
    <el-table-column  label="单价" prop="techPrice" />
    <el-table-column  label="门诊医生" prop="doctor" width="100px"/>
      <el-table-column align="right">
       <template #header>
        <el-button @click="findCheck">查询</el-button>
      </template>
    </el-table-column>
  </el-table>
  <template #footer>
        
        <div class="dialog-footer">
          <el-button @click="closeDialog">取消</el-button>
          <el-button type="primary" @click="addTech">
            添加
          </el-button>
        </div>
      </template>
</el-dialog>    
   </div>
    
</template>

<script setup>
import { onMounted,computed } from 'vue'
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import { getSessionStorage } from '../../common'
import { ElMessage } from 'element-plus'
import axios from 'axios'
const tableData = ref([])
const centerDialogVisible = ref(false)
const checkList = ref([])
const patient=ref({})
const totalMoney=ref(0)
const check=ref({
    techCode: "",
    techName: "",
    techType: "",
    
})
const tableTempData=ref([])
const checkRequest=ref({
    registerId: "",
    checkInfo: "",
    checkPosition: "",
    checkRemark: "",
    techIds: [],
    checkState:'已开立'

})

const temptableData=ref([])
const doctor=ref("")

onMounted(() => {
  let obj=getSessionStorage('patient')
  if (!obj) {
    ElMessage.error('请先选择患者')
  } else {
  patient.value=obj
 checkRequest.value.registerId=patient.value.id
  }
})

const totalprice=computed(()=>
{
    let total=0
    tableTempData.value.forEach((item)=>{
        total+=item.techPrice
    })
    return total
}

)
function findCheck() {
   axios({
    method: 'post',
    url: "http://localhost:8090/checkRequest/findMedicalTechnologyByIdAndName",
    data: check.value,})
    .then((response) => {
      if (response.data.status == 200) {
        checkList.value = response.data.data
      } else {
        ElMessage.error(response.data.msg)
      }
    })

}
function closeDialog() {
  centerDialogVisible.value = false
   checkList.value = []
  
}

function dialogSelectionChange(val) {
    temptableData.value = val

}

function addTech() {
 
    closeDialog()
    for (let i = 0; i < temptableData.value.length; i++) {
        let index=tableData.value.findIndex(item => item.techCode == temptableData.value[i].techCode)
        if (index == -1) {
            tableData.value.push(temptableData.value[i])
            
            totalMoney.value+=tableData.value[i].techPrice
        }
    }
}

function SelectionChange(val) {
    tableTempData.value = val
    
   
}
function deleteTech(){
  for (let i=0;i<tableTempData.value.length;i++){
    let index=tableData.value.findIndex(item=>item.techCode==tableTempData.value[i].techCode)
    if (index!=-1) {
     tableData.value.splice(index,1)
     totalMoney.value-=tableTempData.value[i].techPrice
    }

  }
}
function resetTableData(){
    tableData.value=[]
    checkRequest.value={
    registerId: "",
    checkInfo: "",
    checkPosition: "",
    checkRemark: "",
    techIds: [],
    checkState:'已开立'
    }
}
function saveCheckRequest(){
    console.log(temptableData.value)
    tableTempData.value.forEach((tech)=>{
        let index=checkRequest.value.techIds.findIndex(item => item == tech.id)
        if (index == -1) {
            checkRequest.value.techIds.push(tech.id)
        }
    })
  

    console.log(checkRequest.value+'nihao')
    axios({
    url: "http://localhost:8090/checkRequest/addCheckRequest",
    method: "post",
    data: checkRequest.value,

    }).then((response) => {

        if (response.data.status == 200) {
            ElMessage.success(response.data.msg)
           console.log(checkRequest.value);
        } else {
            ElMessage.error(response.data.msg)
        }
    })

    deleteTech()
    checkRequest.value.checkInfo=''
    checkRequest.value.checkPosition=''
    checkRequest.value.checkRemark=''
    checkRequest.value.techIds=[]
    tableTempData.value=[]
    
}

</script>

<style scoped>

</style>