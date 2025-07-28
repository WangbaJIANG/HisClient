<template>
     <div><el-divider/>
        <h3>窗口挂号</h3>
        <el-divider/>
        <el-button type="primary" @click="saveRegister">挂号</el-button>
        <el-button type="primary">重置</el-button>
        <el-descriptions
    class="margin-top"
    title="基本信息"
    :column="3"
    :size="size"
    border
    
  >
   
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         病历号
        </div>
      </template>
     <el-input v-model="register.caseNumber" disabled></el-input>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         姓名
        </div>
      </template>
     <el-input v-model="register.realName" placeholder="请输入姓名"  style="width: 180px"></el-input>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         性别
        </div>
      </template>
      <el-select v-model="register.gender" class="m-2" placeholder="Select" style="width: 100px">
          <el-option
            v-for="(item,index) of sexList"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item" >
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         年龄
        </div>
      </template>
     <el-input v-model="register.age" style="width: 50px"></el-input>
     <el-select v-model="register.ageType" class="m-2" placeholder="Select" style="width: 80px">
          <el-option
            v-for="(item,index) of ageList"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         出生日期
        </div>
      </template>
      <el-date-picker
        v-model="register.birthday"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="MMM DD, YYYY"
        time-format="HH:mm"
      />
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          身份证
        </div>
      </template>
     <el-input v-model="register.cardNumber" placeholder="请输入身份证号码"></el-input>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          地址
        </div>
      </template>
     <el-input v-model="register.homeAddress" placeholder="请输入地址"></el-input>
    </el-descriptions-item>
    
  </el-descriptions>
  <el-descriptions
    class="margin-top"
    title="基本信息"
    :column="3"
    :size="size"
    border
    
  >
   
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         结算类型
        </div>
      </template>
      <el-select v-model="categrory" class="m-2" placeholder="Select" style="width: 100px">
          <el-option
            v-for="(item,index) of categroryList"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         看诊日期
        </div>
      </template>
      <el-date-picker 
      disabled=""
        v-model="register.visitDate"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="MMM DD, YYYY"
        time-format="HH:mm"
      />
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         午别
        </div>
      </template>
      <el-select disabled v-model="register.noon" class="m-2" placeholder="Select" style="width: 100px">
          <el-option
           v-for="(noon,index) of noonList"
           :key="index"
           :label="noon.label"
           :value="noon.value"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item" >
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         挂号科室
        </div>
      </template>

     <el-select v-model="register.deptmentId" class="m-2" placeholder="Select" style="width:180px">
          <el-option
            v-for="dept of departmentList"
            :key="dept.id"
            :label="dept.deptName"
            :value="dept.id"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
         号别
        </div>
      </template>
      <el-select v-model="registLevelId" class="m-2" placeholder="Select" style="width: 100px" @change="levelChange">
          <el-option
            v-for="registLevel of registLevelList"
            :key="registLevel.id"
            :label="registLevel.registName"
            :value="registLevel.id"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          看诊医生
        </div>
      </template>
      <el-select v-model="register.employeeId" class="m-2" placeholder="Select" style="width: 100px" >
          <el-option
            v-for="(employee,index) of employeeList"
            :key="index"
            :label="employee.realname"
            :value="employee.id"
          />
        
    </el-select>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          初始号额
        </div>
      </template>
     <el-input  disabled v-model="registLevel.registQuota"></el-input>
    </el-descriptions-item> 
     <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          已用号额
        </div>
      </template>
     <el-input disabled v-model="registLevel.registQuota"></el-input>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          病历本
        </div>
      </template>
   <el-switch  v-model="register.isBook"></el-switch>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          应收金额
        </div>
      </template>
     <el-input  v-model="register.registMoney" disabled ></el-input>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          收费方式
        </div>
      </template>
      <el-select v-model="register.registMethod" class="m-2" placeholder="Select" style="width: 100px">
          <el-option
            v-for="(item,index) of payList"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        
    </el-select>
    </el-descriptions-item>
  </el-descriptions>

     </div>
</template>

<script setup>
import axios from "axios";
import { ElMessage } from "element-plus";
import { onMounted, ref, watch } from "vue";
import { useRouter } from 'vue-router';
const router=useRouter();
let maxCaseNumber=ref('')
const sex=ref('男');

let noon=ref('上午');
let ageType=ref('年');
const registLevel=ref({});
let registLevelId=ref('');
let employeeId=ref('');
const registLevelList=ref([]);

const departmentList=ref([]);
let employeeList=ref([]);
let payWay=ref('');
let birthDate=ref('');
let age=ref('');
let realname=ref('');
let address=ref('');
let IDcard=ref('');

let payList = ref([
  {
    label: "微信",
    value: "Wechat"
  },
  {
    label: "支付宝",
    value: "Alipay"
  }
]);
let categrory=ref('1');
let isbook=ref('true');

const register=ref({
  caseNumber:'',
  realName:'',
  gender:'男',
  birthday:'',
  cardNumber:'',
  address:'',
  visitDate:'',
  noon:'上午',
  deptmentId:'',
  employeeId:'',
  age:'',
  ageType:'年',
  isBook: false,
  registMoney:'0',
  registLevelId:'',
  registMethod:'',

})
let categroryList = ref([
  {
    label: "自费",
    value: "1"
  },
  {
    label: "市医保",
    value: "2"
  }
]);

let sexList = ref([
  {
    label: "男",
    value: "男"
  },
  {
    label: "女",
    value: "女"
  }
]);
let ageList = ref([
  {
    label: "天",
    value: "天"
  },
  {
    label: "年",
    value: "年"
  }
]);let noonList = ref([
  {
    label: "上午",
    value: "上午"
  },
  {
    label: "下午",
    value: "下午"
  }
]);


function findDeptAll(){
  axios.get("http://localhost:8090/department/findDeptAll").then((response)=>{

  departmentList.value=response.data.data
})
}
function findRegistLevelAll(){
  axios.get("http://localhost:8090/registLevel/findRegistLevelAll").then((response)=>{
  registLevelList.value=response.data.data
})
}

function findLevelById(){
  axios.get("http://localhost:8090/registLevel/findLevelById?id="+registLevelId.value).then((response)=>{
  registLevel.value=response.data.data
  register.value.registMoney=registLevel.value.registFee
  if(register.isBook){
    register.value.registMoney+=1
  }
  console.log(response.data)})
} 


//自动生成最大
function findMaxCaseNumber(){
  axios.get("http://localhost:8090/register/findMaxCaseNumber").then((response)=>{
    register.value.caseNumber=Number(response.data.data)+1
  })
}

function levelChange(){
  findLevelById();
  if(register.value.deptmentId){
    findEmpByDeptIdAndLevelId();
  }
}

function findEmpByDeptIdAndLevelId(){
 
  axios.get(`http://localhost:8090/employee/findEmpByDeptIdAndLevelId?deptmentId=${register.value.deptmentId}&registLevelId=${registLevelId.value}`).then((response)=>{
    employeeList.value=response.data.data

 
  }
  
  )
 
}
function saveRegister(){
  axios({url:"http://localhost:8090/register/addRegister",
              data:register.value
            , method:'post',}
            ).then((response)=>{
              console.log(response.data)
    if(response.data.status==200){
      ElMessage.success('挂号成功')
    }else{
      ElMessage.error('挂号失败') 
    }
  }).then((response)=>{
   console.log(register.value.employeeId)
  })
}
onMounted(()=>{
  let date=new Date();
  let hours=date.getHours();
  register.value.visitDate=date  
  if(hours<12){
    register.value.noon='上午'
  }else{
    register.value.noon='下午'
  }
  findMaxCaseNumber();
findDeptAll();
findRegistLevelAll();




})

watch(
  () => register.value.isBook,
  (newValue) => {
    if (newValue) {
     register.value.registMoney+=1

    } else {
      register.value.registMoney-=1
    }
  }
);
</script>

<style scoped>
.margin-top {
    margin-top: 30px;
}
</style>