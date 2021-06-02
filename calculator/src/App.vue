<template>

  <el-container>
    <el-main>
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="计算器" name="first">
          <Calculator></Calculator>
        </el-tab-pane>
        <el-tab-pane label="购物车" name="second">
          <ShoppingCar></ShoppingCar>
        </el-tab-pane>
        <el-tab-pane label="提交表单" name="third">
          <Form></Form>
        </el-tab-pane>
        <el-tab-pane label="测试" name="fourth">
          <Test></Test>
        </el-tab-pane>
        <el-tab-pane label="test" name="fifth">
          <EXP></EXP>
        </el-tab-pane>
        <el-tab-pane label="todos" name="sixth">
          <Todos></Todos>
        </el-tab-pane>
      </el-tabs>
    </el-main>
    <el-footer height="40px" style="background-color:#252319; text-align: center">
      <el-link style="color: #FFFFFF" href="https://github.com/tomotsune/" icon="el-icon-user">github</el-link>
    </el-footer>
  </el-container>

  <!--<HelloWorld></HelloWorld>-->
  <!--<ShoppingCar></ShoppingCar>-->

</template>

<script setup>
import Calculator from './components/Calculator.vue'
import ShoppingCar from './components/ShoppingCar.vue'
import Form from './components/Form.vue'
import Test from './components/Test.vue'
import EXP from './components/EXP_3.vue'
import Todos from './components/Todos.vue'
import {ref} from "vue";

let activeName = ref('first')

const handleClick = (tab, event) => {
  console.log(tab, event);
}

</script>
<style>

</style>
