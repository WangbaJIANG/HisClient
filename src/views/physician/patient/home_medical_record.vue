<template>
   <div>
    患者信息：
    <el-tag type="primary">姓名：{{ patient.realName }}</el-tag>
    <el-tag type="primary">病历号：{{ patient.caseNumber }}</el-tag>
    <el-tag type="primary">年龄：{{ patient.age }}</el-tag>
    <el-tag type="primary">性别：{{ patient.gender }}</el-tag>  
   <el-divider/>
   
    <h3>病例首页</h3>
    <el-descriptions
    
    :column="1"
    :size="size"
    border

  >
    <el-descriptions-item label="主诉" class="lable">
 <el-input
    v-model="medical.readme"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="现病史">
       <el-input
    v-model="medical.present"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  /></el-descriptions-item>
 
    <el-descriptions-item label="现病治疗情况">
        <el-input
    v-model="medical.presentTreat"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="既往史">
        <el-input
    v-model="medical.history"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="过敏史">
        <el-input
    v-model="medical.allergy"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item> 
     <el-descriptions-item label="身体检查">
        <el-input
    v-model="medical.physique"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item>
    <el-descriptions-item label="诊断">
        <el-table :data="diseaseSelection" style="width: 100%" @selection-change="SelectionChange">
                <el-table-column type="selection" width="55" />
    <el-table-column prop="diseaseName" label="疾病名称" width="80px"/>
    <el-table-column prop="diseaseCode" label="疾病编码"  />
    <el-table-column prop="diseaseICD" label="疾病ICD编码" />
    <el-table-column prop="diseaseCategory" label="疾病类型"  width="80px"/>
      <el-table-column align="right">
      <template #header>
        <el-button type="primary" @click="centerDialogVisible = true"> 编辑 </el-button>
        <el-button type="primary" @click="deleteDisease">
          删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
    </el-descriptions-item>


    <el-descriptions-item label="检查检验建议">
        <el-input
    v-model="medical.proposal"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item> 
    <el-descriptions-item label="注意事项">
        <el-input
    v-model="medical.careful"
    style="width: 100%"
    :rows="1"
    type="textarea"
    placeholder="Please input"
  />
    </el-descriptions-item> 
  </el-descriptions>
  <div  class="btnbox" > <el-button type="primary" @click="saveMedicalRecord"> 保存 </el-button>
        <el-button type="primary" @click="resetMedicalRecord"> 
          清空
        </el-button></div>

      




  <el-dialog v-model="centerDialogVisible" title="Warning" width="700" center>
    <el-tag>疾病编码：</el-tag>
    <el-input placeholder="请输入疾病编码" v-model="disease.diseaseCode"> </el-input>  
    <el-tag>疾病名称：</el-tag>
    <el-input placeholder="请输入疾病名称" v-model="disease.diseaseName"> </el-input>
    <el-table :data="diseaseList" style="width: 100%" @selection-change="dialogSelectionChange">
    <el-table-column type="selection" width="55"/>
    <el-table-column prop="diseaseName" label="疾病名称" />
    <el-table-column prop="diseaseCode" label="疾病编码"  />
    <el-table-column prop="diseaseICD" label="国际ICD编码" width="150px"/>
      <el-table-column align="right">
       <template #header>
        <el-button @click="findDisease">查询</el-button>
      </template>
    </el-table-column>
  </el-table>




    <template #footer>
        
      <div class="dialog-footer">
        <el-button @click="closeDialog">取消</el-button>
        <el-button type="primary" @click="addDisease">
          添加
        </el-button>
      </div>
    </template>
</el-dialog>

        
   </div>
    


</template>

<script setup>
import axios from "axios";
import { onMounted, ref, watch } from "vue";
import { getSessionStorage } from "../../../common";
import { ElMessage } from "element-plus";
const centerDialogVisible = ref(false)
const patient=ref({})
const disease=ref({
    diseaseCode: "",
    diseaseName: "",
    
})
let tableTempdata = ref([])
const diseaseList=ref([])
const diseaseSelection=ref([])
const deleteSelection=ref([])
const medical=ref({
    registerId: 0,
    readme: "",
    present: "",
    presentTreat: "",
    history: "",
    allergy: "",
    physique: "",
    proposal: "",
    careful: "",
    diseaseIds: [],
})

onMounted(() => {
  // 获取患者信息
  let obj = getSessionStorage("patient");
  if (obj) {
    patient.value = obj;
  } else {
    ElMessage({
      message: "请先选择患者",
      type: "warning",
    });
  }
});


function closeDialog() {
    centerDialogVisible.value = false;
    diseaseList.value =[];
  }


function addDisease(){
  
    for (let i = 0; i < tableTempdata.value.length; i++) {
        let index=diseaseSelection.value.findIndex(item => item.diseaseCode == tableTempdata.value[i].diseaseCode)
        if (index == -1) {
            diseaseSelection.value.push(tableTempdata.value[i])
            medical.value.diseaseIds.push(tableTempdata.value[i].id)
        }
    }

    closeDialog()
}
//对话框表格变动
function dialogSelectionChange(val) {
tableTempdata.value = val;

   
}
//页面表格变动
function SelectionChange(val) {
    tableTempdata.value = val;

}
//删除选中数据
function deleteDisease(){
 tableTempdata.value.forEach((disease)=>{
    let index=diseaseSelection.value.findIndex(item => item.diseaseCode == disease.diseaseCode)
    if (index != -1) {

        diseaseSelection.value.splice(index, 1)
        
        medical.value.diseaseIds.splice(index, 1)
    }
 })
}
//查询疾病
function findDisease(){
    axios({
        method: "post",
        url: "http://localhost:8090/phycisian/findDiseaseByCondition",
        data:disease.value,

    }).then((response) => {
        console.log(disease.value)
        console.log(response.data.data)
        if (response.data.status == 200) {
            diseaseList.value = response.data.data;
        } else {
            ElMessage({
                message: response.data.msg,
                type: "error",
              });
        }
    }).catch((error) => {
        console.error(error);
        ElMessage({
            message: "查询失败",
            type: "error",
          });
    });
}

function saveMedicalRecord(){
console.log(medical.value)
medical.value.registerId=patient.value.id

axios({method: "post",
    url:"http://localhost:8090/physician/saveMedicalRecord",
    data:medical.value,
}).then((response)=>{
  
    if (response.data.status == 200) {
        ElMessage({
            message: "保存成功",
            type: "success",
          });
    } else {
        ElMessage({
            message: response.data.msg,
            type: "error",
          });
    }
})
//清空数据
resetMedicalRecord()

}

function resetMedicalRecord(){
    medical.value={
        registerId: 0,
        readme: "",
        present: "",
        presentTreat: "",
        history: "",
        allergy: "",
        physique: "",
        proposal: "",
        careful: "",
        diseaseIds: [],
    }
    diseaseSelection.value=[]
    tableTempdata.value=[]
    diseaseList.value=[]
}
</script>

<style scoped>
.lable{
 display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.btnbox{
text-align: center;
display: flex;
justify-content: center;    
    margin-top: 20px;
  margin-bottom: 10px;
}
</style>