<template>
  {{ users }}
  <el-input
      type="text"
      placeholder="请输入内容"
      v-model="form.name"
      maxlength="10"
      show-word-limit
  >
  </el-input>
  <el-button type="primary" @click="onsubmit">submit</el-button>
</template>

<script setup>
import {ref, reactive, customRef} from 'vue'

const myRef = (value) => {
  return customRef(((track, trigger) => {
    fetch(value)
        .then((ref) => {
          return ref.json()
        }).then((data) => {
      value = data
      trigger()
    }).catch((err) => {
      console.log(err);
    })
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        value = newValue
        trigger()
      }
    }
  }))
}

let users = myRef('../../public/data.js')
let form = reactive({id: '', name: ''});
const onsubmit = () => {
  for (const user of users.value) {
    if (user.name === form.name) {
      alert("The name you input have been occupied!")
    } else {
      form.name = ''
    }
  }
}

</script>

<style>

</style>
