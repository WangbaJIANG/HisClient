<script setup>

import { ref } from 'vue'
let msg=ref('hello')
let count=ref(10)
let likes=ref([2,3,4,5,5])
let flag1=ref(true)
let user=ref([
  {id:'123',name:'ma'},
  {id:'123',name:'ma1'}
])
let title=ref("你好")
function test1(){
  likes.value.push("haha")
}
function test(){
  msg.value+=1
  console.log(count.value)
}
</script>

<template>
<div><button @click="test1">{{ count }}</button>

  <input type="text" name="" id="" v-model="msg">
  <ul>
    <li v-for="(item,index) of likes" :key="index">
        {{ item }}
    </li>
    <li v-for="item of user" :key="item.id"> 
      {{ item.name }}
    </li>
  </ul>
  <h3 v-if="flag1">{{ title }}</h3>
  <button @click="flag1=!flag1">隐藏</button>
  <br>
 
  <p>{{ msg }}</p>
  </div>
</template>

<style scoped>

</style>
