<template>
  <div>
    


         <div>
        <el-divider/>
          检验项目
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;">搜索 </el-button>
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
        <el-button size="small" @click="registRecord(scope.$index, scope.row)">
         已完成检查
        </el-button>
       
        
    </template>
     
    </el-table-column>
    <el-divider/>
  </el-table>

  <el-table :data="tableData" style="width: 100%">
   
         <el-table-column label="检查编码" width="180" property="realName">
        </el-table-column>
        <el-table-column label="检查名称" width="180" property="caseNumber">
        </el-table-column>
         <el-table-column label="检查时间" width="180" property="gender">
        </el-table-column>
         <el-table-column label="单价" width="180" property="age">
        </el-table-column>
          <el-table-column label="费用分类" width="180" property="age">
        </el-table-column>
       
       
  

    <el-divider/>
  </el-table>


   <el-descriptions title="确认检验科室和检查医生">
    <el-descriptions-item label="科室选择">
      <el-select
      v-model="departmentSelect"
      placeholder="Select"
      size="large"
      style="width: 240px"
    >
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
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
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
    </el-descriptions-item>
  
  </el-descriptions>

 <el-descriptions title="输入检查结果">
 <el-descriptions-item>
    <el-input type="textarea" placeholder="输入检查结果" v-model="textarea" :rows="1" />
 </el-descriptions-item>
  
  </el-descriptions>

    <el-button @click="findCheck" type="primary">开始检查</el-button>
  <el-button @click="findCheck" type="primary">重置医生</el-button>

    </div>
  </div>

  
</template>

<script setup>
</script>

<style scoped>

</style>