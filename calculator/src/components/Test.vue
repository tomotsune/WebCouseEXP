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
      if (month === 2) {
        months[1] = (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0) ? 29 : 28
      }
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
  let capital = ["零", "壹", "贰", "叁", "肆", "伍", "陆", "柒", "捌", "玖"]
  let unit = ["", "拾", "佰", "仟", "万", "拾万", "百万", "仟万", "亿", "拾亿", "百亿", "仟亿"]

  let reg = /^([1-9]\d{0,12}|0)([.]?|(\.\d{1,2})?)$/
  if (!reg.test(amount.value)) amount.value = ''
  else {
    let result = ''

    // Splitting the input amount into whole number and decimal parts
    let both = amount.value.split('.')
    // Translating integer part
    for (let i = 0; i < both[0].length; i++) {
      result += capital[(both[0][i])]
      result += unit[(both[0].length - 1 - i) % 4]
      if (both[0].length >= 4 && i + 5 === both[0].length) result += "万";
      if (both[0].length >= 8 && i + 9 === both[0].length) result += "亿";
      // 8 9 10 11 亿|  4 5 6 7 万| 0 1 2 3

      if (both[0][i + 1] === '0') {
        for (let j = i + 1; j < both[0].length; j++) {
          if (both[0][j] !== '0') {
            result += capital[0]
            i = j - 1;
            break
          } else i = j
        }
      }
    }
    result += "元"

    // Translating the decimal part
    if (both[1] !== undefined) {
      result += both[1][0] === '0' && both[1][1] !== undefined ? "零" : capital[both[1][0]] + "角"
      result += both[1][1] !== undefined ? capital[both[1][1]] + "分" : ''
    } else result += "整"

    amount.value = result
  }


}
</script>

<style scoped>

</style>