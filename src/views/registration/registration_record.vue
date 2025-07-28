<template>
    <div>
        <el-divider/>
        窗口退号
        <el-divider/>
       <div class="search">
          <el-input placeholder="请输入患者病历号" style="width: 150px; margin-left:10px;" v-model="caseNumber"></el-input>
         <el-input placeholder="请输入患者姓名" style="width: 150px; margin-left:10px;" v-model="realName"></el-input>
        <el-button style="margin-left: 20px;">搜索 </el-button>
       </div>
   <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" :index="indexMethod"  label="序号" width="60px"/>
        <el-table-column label="患者病历号" width="180" property="caseNumber">
        </el-table-column>
        <el-table-column label="患者姓名" width="180" property="realName">
        </el-table-column>
         <el-table-column label="性别" width="180" property="gender">
        </el-table-column>
         <el-table-column label="身份证号" width="180" property="cardNumber">
        </el-table-column>
         <el-table-column label="付款方式" width="180" property="registMethod">
        </el-table-column>
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="registRecord(scope.$index, scope.row)">
          退号
        </el-button>
       
        
    </template>
     
    </el-table-column>
    <el-divider/>
  </el-table>

    </div>
</template>

<script setup>

function registRecord(index, row) {
    console.log("退号", index, row);
    // 这里可以添加退号的逻辑
}
</script>

<style scoped>

.search {
    margin-right: 20px;
}

</style>