<template>
  <table>
    <tr>
      <th></th>
      <th>NAME</th>
      <th>PUBLISH_DATE</th>
      <th>PRICE</th>
      <th>PURCHASE_QUANTITY</th>
      <th>OPERATION</th>
    </tr>
    <tr v-for="(book,idx) in books">
      <td>{{ idx + 1 }}</td>
      <td>{{ book.name }}</td>
      <td>{{ book.date }}</td>
      <td>{{ book.price }}</td>
      <td>
        <button @click="decrement(idx)" :disabled="book.count===1">-</button>
        {{ book.count }}
        <button @click="increment(idx)">+</button>
      </td>
      <td>
        <button @click="handleRemove(idx)">remove</button>
      </td>
    </tr>
  </table>
  <p> total price:{{ total }}</p>

  <el-table
      :data="books"
      height="150"
      border
      style="width: 100%">
    <el-table-column
        prop="name"
        label="NAME"
        width="180">
    </el-table-column>
    <el-table-column
        prop="date"
        label="PUBLISH_DATE"
        width="180">
    </el-table-column>
    <el-table-column
        prop="price"
        label="PRICE">
    </el-table-column>
    <el-table-column
        prop="count"
        label="COUNT">
    </el-table-column>
  </el-table>
</template>
<script setup>
import {ref, computed} from 'vue'

let books = ref([
  {id: 1, name: "Art of software testing", date: "2009-6", price: 85, count: 1},
  {id: 2, name: "Art of software testing", date: "2009-5", price: 85, count: 1},
  {id: 3, name: "Art of software testing", date: "2009-7", price: 85, count: 1}
])

const increment = (idx) => {
  books.value[idx].count++;
}
const decrement = (idx) => {
  books.value[idx].count--;
}

const handleRemove = (idx) => {
  books.value.splice(idx,1)// 下标向后移除1条
}
const total = computed(() => {
  /* return books.reduce((t, book) => {
     return t + book.price * book.count
   }, 0)*/
  let total = 0;
  for (let i = 0; i < books.value.length; i++) {
    let item = books.value[i];
    total += item.price * item.count
  }
  return total
})

</script>

<style scoped>
table {
  border: 1px solid #e9e9e9;
  border-collapse: collapse;
}

th, td {
  padding: 8px 16px; /*内容到元素的距离*/
  border: 1px solid #e9e9e9;
  text-align: left;
}

th {
  background-color: #f7f7f7;
  color: #5c6b77;
  font-weight: 600;
}
</style>