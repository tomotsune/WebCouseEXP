<template>

  <el-input
      type="text"
      placeholder="请输入日期yyyy-MM-dd"
      v-model="dateStr"
      maxlength="10"
      show-word-limit
  >
  </el-input>
  <el-button type="primary" @click="getDate">submit</el-button>
  <el-input
      type="text"
      placeholder="请输入号码 地区码+前缀码+后缀码"
      v-model="phoneNum"
      maxlength="10"
      show-word-limit
  >
  </el-input>
  <el-button type="primary" @click="verifyPhoneNum">submit</el-button>
  <el-input
      type="text"
      placeholder="请输入金额"
      v-model="amount"
      maxlength="10"
      show-word-limit
  >
  </el-input>
  <el-button type="primary" @click="transform">submit</el-button>
</template>

<script setup>
import {ref} from "vue";

let dateStr = ref('')
const getDate = () => {
  //2004-04-30 | 2004-02-29
  dateStr.value.trim();
  let reg = /^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$/
  if (!reg.test(dateStr.value)) dateStr.value = ''
  else {
    let months = [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    let year = parseInt(dateStr.value.substr(0, 4), 10)
    console.log("year", year)
    let month = parseInt(dateStr.value.substr(5, 2), 10)
    console.log("month", month)
    let day = parseInt(dateStr.value.substr(8, 2), 10)
    console.log("day", day)
    if ((day === 1)) {
      --month;
      day = months[month - 1]
    } else {
      --day
    }
    dateStr.value = year + '-' + month + '-' + day
  }

}
let phoneNum = ref('')
const verifyPhoneNum = () => {
  let reg = /\d{0,3}[^0-1]{3}\d{4}/
  if (reg.test(phoneNum.value))
    phoneNum.value = "correct"
  else phoneNum.value = "error"
}
let amount = ref('')
const transform = () => {
  console.log(amount.value)
  let unit = ["", "拾", "佰", "仟", "万", "拾万", "百万", "仟万", "亿", "拾亿", "百亿", "仟亿"]
  let capital = ["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"]

  let reg = /^([1-9]\d{0,12}|0)([.]?|(\.\d{1,2})?)$/
  if (!reg.test(amount.value)) amount = ''
  else {
    console.log("enter else sentence")
    let result = ''
    if (amount.value.indexOf('.') !== -1) {
      var sub = amount.value.split('.');
      for (let i = 0; i < sub[0].length; i++) {
        result += unit[sub[0][i] - 1]
        result += capital[amount.value.length - 2 - i]
      }
      result += "元"
      result += unit[sub[1][0] - 1] + 角
      result += unit[sub[1][1] - 1] + 分
    } else {
      // input >=3 err
      for (let i = 0; i < amount.value.length; i++) {
        console.log("i=", i)
        if (i + 1 !== amount.value.length || amount.value[i] !== '0') {
          result += capital[(amount.value[i])]
          result += unit[amount.value.length - 1 - i]
        }
      }
    }
    console.log(result);
    result += "元整"
  }
  amount.value = result
}

}
</script>

<style scoped>

</style>