<template>
  <div>
    <el-divider/>
      <h2>药库管理</h2>
<el-divider/>
            <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 250px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 250px; margin-left:10px;" v-model="realName"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 250px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;">搜索 </el-button>
       </div>
        <el-button style="margin-left: 20px;" type="primary">增加药品 </el-button>

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
         检查项目
        </el-button>
       
        
    </template>
     
    </el-table-column>
    <el-divider/>
  </el-table>
  </div>
</template>

<script setup>

</script>

<style scoped>

</style>