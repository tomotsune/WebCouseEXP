<template>

  <el-row>
    <el-col :span="12" :offset="6">
      <el-input
          v-model="input"
          readonly>
      </el-input>
    </el-col>
  </el-row>

  <el-row>
    <el-col :span="3" :offset="6">
      <el-button type="primary" round @click="enterNum(7)">7</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(8)">8</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(9)">9</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="success" round @click="enterOpr('+')">+</el-button>
    </el-col>
  </el-row>

  <el-row>
    <el-col :span="3" :offset="6">
      <el-button type="primary" round @click="enterNum(4)">4</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(5)">5</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(6)">6</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="success" round @click="enterOpr('-')">-</el-button>
    </el-col>
  </el-row>

  <el-row>
    <el-col :span="3" :offset="6">
      <el-button type="primary" round @click="enterNum(1)">1</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(2)">2</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="primary" round @click="enterNum(3)">3</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="success" round @click="enterOpr('*')">*</el-button>
    </el-col>
  </el-row>

  <el-row>
    <el-col :span="3" :offset="6">
      <el-button type="primary" round @click="enterNum(0)">0</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="warning" round @click="equal()">=</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="danger" round @click="clear()">C</el-button>
    </el-col>
    <el-col :span="3">
      <el-button type="success" round @click="enterOpr('/')">/</el-button>
    </el-col>

  </el-row>
</template>

<script setup>
// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md
import {ref} from 'vue'

let input = ref('')
let operator = ''
let priorNum = ''
const enterNum = (value) => {
  input.value += value
  console.log("enterNum触发")
}
const enterOpr = (value) => {
  operator = value
  priorNum = input.value
  input.value = ''
  console.log("enterOpr触发", input)
}
const equal = () => {
  input.value = eval(priorNum.toString() + operator + input.value.toString())
  console.log("equal触发", input.value)
}
const clear = () => {
  input.value = ''
  priorNum = ''
  operator = ''
}
</script>


<style>

.el-row {
  margin-bottom: 20px;

&
:last-child {
  margin-bottom: 0;
}

}
.el-col {
  border-radius: 4px;
}

.el-button {
  margin: 0 auto;
}
</style>
