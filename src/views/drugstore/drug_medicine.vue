<template>
  <div>
    <div>
        <el-divider/>
       <h2>药房退药</h2> 
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
         <el-table-column label="患者状态" width="180" property="gender">
        </el-table-column>
         <el-table-column label="身份证号" width="180" property="cardNumber">
        </el-table-column>
         <el-table-column label="挂号时间" width="180" property="registMethod">
        </el-table-column>
  
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="registRecord(scope.$index, scope.row)">
          更新病历
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
   <el-table :data="tableData" style="width: 100%;">
        <el-table-column type="index" :index="indexMethod"  width="60px"/>
        <el-table-column label="药品编码" width="180" property="caseNumber">
        </el-table-column>
        <el-table-column label="药品名称" width="180" property="realName">
        </el-table-column>
         <el-table-column label="药品规格" width="180" property="gender">
        </el-table-column>
         <el-table-column label="包装单位" width="180" property="cardNumber">
        </el-table-column>
         <el-table-column label="生产产家" width="180" property="registMethod">
        </el-table-column>
         <el-table-column label="单价" width="180" property="registMethod">
        </el-table-column>
         <el-table-column label="药品数量" width="180" property="registMethod">
        </el-table-column>
  
  

    <el-table-column label="操作">
        <template #default="scope">
        <el-button size="small" @click="registRecord(scope.$index, scope.row)" type="primary">
          发药
        </el-button>
       
       
        
    </template>
     
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
const pageNum=ref(1)
const pageSize=ref(0)
const total=ref(0)

</script>

<style scoped>

</style>