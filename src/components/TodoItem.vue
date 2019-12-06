<template>
  <el-card class="box-card mb-regular" shadow="hover">
    <div slot="header">
      <span class="title">{{title}}</span>
    </div>
    <div>{{content}}</div>
  </el-card>
</template>

<script>
export default {
    name:"todo-item",
    props:{
        title:String,
        content:String
    }
}
</script>

<style>
    .mb-regular{
      margin-bottom: 10px;
    }
</style>