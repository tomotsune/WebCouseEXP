<template>
  <div style="align-content: center">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <el-input v-model="input"></el-input>
          <el-button class="button" type="text" @click="onsubmit">add</el-button>
        </div>
      </template>
      <div v-for="(todo,idx) in todos" class="text item">
        <el-button type="text" icon="el-icon-circle-close" @click="remove(idx)"></el-button>
        {{ todo }}
      </div>
    </el-card>
  </div>

</template>

<script setup>
import {ref, reactive} from 'vue'

let todos = reactive([]);
let input = ref('');
const onsubmit = () => {
  todos.push(input.value)
  input.value = ''
}
const remove = (idx) => {
  todos.splice(idx, 1)
}
</script>

<style>
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.box-card {
  width: 480px;
}
</style>
