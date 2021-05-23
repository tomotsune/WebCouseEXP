<template>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="计算器" name="first"><Calculator></Calculator></el-tab-pane>
      <el-tab-pane label="购物车" name="second"><ShoppingCar></ShoppingCar></el-tab-pane>
      <el-tab-pane label="提交表单" name="third"><Form></Form></el-tab-pane>
    </el-tabs>
  <!--<HelloWorld></HelloWorld>-->
  <!--<ShoppingCar></ShoppingCar>-->

</template>

<script setup>
import Calculator from './components/Calculator.vue'
import ShoppingCar from './components/ShoppingCar.vue'
import Form from './components/Form.vue'
import {ref} from "vue";
let activeName = ref('first')

const handleClick = (tab, event) => {
  console.log(tab, event);
}

</script>
<style>

</style>
