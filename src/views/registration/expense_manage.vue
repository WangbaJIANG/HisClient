<template>
    <div>
        费用记录查看
          <div>
       
    <el-divider/>
    窗口收费
    <el-divider/>
    <h1>患者信息查询</h1>
    <el-descriptions>
        <el-descriptions-item label="病历号" width="40%" style=""> 
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"

                placeholder="Please input"
            />
            </el-descriptions-item>   
             <el-descriptions-item label="患者名" width="40%" >
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"

                placeholder="Please input"
            />
            </el-descriptions-item>  


    </el-descriptions>


    <el-divider/>
    患者信息确认

    <el-descriptions column="4">
        <el-descriptions-item label="患者名" > 
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"
                disabled
                placeholder="Please input"
            />
            </el-descriptions-item>   
            <el-descriptions-item label="身份证号" >
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"
                disabled

                placeholder="Please input"
            />
            </el-descriptions-item>   
              <el-descriptions-item label="年龄" >
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"
                disabled
                placeholder="Please input"
            />
            </el-descriptions-item>  
                <el-descriptions-item label="性别" >
            <el-input
                v-model="textarea"
                style="width: 200px"
                :rows="1"
                disabled
                placeholder="Please input"
            />
            </el-descriptions-item>  


    </el-descriptions>
      
         <el-button type="primary" disabled>项目金额:</el-button>

        <el-table :data="tableData" style="width: 100%"  @selection-change="dialogSelectionChange">
       <el-table-column type="selection" width="55" />
        <el-table-column label="项目名称" width="180" property="techName">
        </el-table-column>

         <el-table-column label="单价" width="180" property="techPrice">
        </el-table-column>
         <el-table-column label="类型" width="180" property="techType">
        </el-table-column>
                 <el-table-column label="规格" width="180" property="techFormat">
        </el-table-column>

         <el-table-column label="数量" width="180" property="drugNumber">
        </el-table-column>  
        <el-table-column label="开立时间" width="180" property="drugNumber">
        </el-table-column>
        <el-table-column label="状态" width="180" property="checkState">
        </el-table-column>


  
    <el-divider/>
  </el-table>



    </div>
    </div>
</template>

<script setup>

</script>

<style scoped>

</style>