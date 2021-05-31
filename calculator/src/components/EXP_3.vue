<template>
  <label>
    <input type="text" :value="msg" v-on:input="msg=$event.target.value">
  </label>
  {{ msg }}
  <br>
  <label>
    <input type="radio" name="gender" value="male" v-model="userInfo.gender">
  </label>男
  <label>
    <input type="radio" name="gender" value="female" v-model="userInfo.gender">
  </label>女
  {{ userInfo.gender }}
  <br>
  <label for="single">
    <input type="checkbox" v-model="checked" id="single">single</label>
  {{ checked }}
  <br>
  <label>
    <input type="checkbox" value="1" v-model="userInfo.hobbies">
  </label>1
  <label>
    <input type="checkbox" value="2" v-model="userInfo.hobbies">
  </label>2
  <label>
    <input type="checkbox" value="3" v-model="userInfo.hobbies">
  </label>3
  <label>
    <input type="checkbox" value="4" v-model="userInfo.hobbies">
  </label>4
  <p>{{ userInfo.hobbies }}</p>
  <br>
  <label>
    <select v-model="userInfo.district">
      <option value="hlj">
        黑龙江
      </option>
      <option value="sh">
        上海
      </option>
      <option value="bj">
        北京
      </option>
    </select>
  </label>
  {{ userInfo.district }}
  <input type="number" v-model.number.lazy="userInfo.age"> <!--延迟加载-->
  {{userInfo.age}}
</template>

<script setup>
import {ref, reactive} from 'vue'

let msg = ref("test vue")
let userInfo = reactive({gender: 'male', hobbies: [], district: 'hlj', age: 0})
let checked = ref(false)
</script>


<style scoped>

</style>