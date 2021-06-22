<template>

  <el-row :gutter="20">
    <el-col :span="5">
      <el-table
          :data="fat"
          height="250"
          border
          style="width: 100%">
        <el-table-column
            prop="block"
            label="物理块号">
        </el-table-column>
        <el-table-column
            prop="next"
            label="下一块">
        </el-table-column>
      </el-table>
    </el-col>
    <el-col :span="7">
      <el-card shadow="hover" class="box-card">
        <template #header>
          <div class="card-header">
            <span>物理块0</span>
          </div>
        </template>
        <div class="text item">
          <el-descriptions class="margin-top" :column="4" :size="mini" border title="bitmap" style="width: 200px">
            <el-descriptions-item v-for="o in 12" :key="o">
              <template #label>
                {{ o - 1 }}
              </template>
              <div style="color:#409EFF">{{ bitmap[o - 1] }}</div>
            </el-descriptions-item>
          </el-descriptions>
        </div>
      </el-card>
    </el-col>
    <el-col :span="12">
      <el-card shadow="hover" class="box-card">
        <template #header>
          <div class="card-header">
            <span>物理块1</span>
          </div>
        </template>
        <el-table
            :data="inodes"
            height="250"
            border
            style="width: 100%">
          <el-table-column
              prop="id"
              label="序号">
          </el-table-column>
          <el-table-column
              prop="type"
              label="类型">
          </el-table-column>
          <el-table-column
              prop="block"
              label="起始块号">
          </el-table-column>
          <el-table-column
              prop="size"
              label="大小">
          </el-table-column>
          <el-table-column
              prop="date"
              label="创建日期">
          </el-table-column>
        </el-table>
      </el-card>
    </el-col>
  </el-row>

  <el-space wrap ref="wrap">
    <el-card shadow="hover" class="box-card" style="width: 180px" v-for="(b,idx) in bs" :key="idx">
      <template #header>
        <div class="card-header">
          <span>{{ '物理块' + (idx + 2) }}</span>
        </div>
      </template>
      <div class="text item">
        <el-collapse-transition>
          <el-table
              :data="b"
              height="150"
              border
              style="width: 100%"  v-show="b.length!==0">
            <el-table-column
                prop="file_name"
                label="名">
            </el-table-column>
            <el-table-column
                prop="i_node"
                label="结点">
            </el-table-column>
          </el-table>
        </el-collapse-transition>
      </div>
    </el-card>
  </el-space>


  <el-drawer
      title="输入指令"
      v-model="drawer"
      :direction="btt"
      :before-close="handleClose" destroy-on-close>
    <el-input placeholder="请输入内容" v-model="instruction" @keyup.enter="run">
      <template #prepend>{{ path + '\\' }}</template>
    </el-input>
    <span style="color: #909399" v-html="tips"></span>
  </el-drawer>
  <el-affix position="bottom" :offset="20">
    <el-button @click="drawer = true" type="primary" icon="el-icon-video-play" circle></el-button>
  </el-affix>
</template>

<script setup>
import {ref, reactive, onMounted} from 'vue'
import {ElMessageBox} from "element-plus";

let tips = ref(`<span>Tips:</span>
      <br>
      <span>1. md + name: Create a new catalogue</span>
      <br>
      <span>2. cd + name: Shift catalogues</span>
      <br>
      <span>3. mk + name + size: Create a new file</span>
      <br>
      <span>4. del + name: Delete a existed file</span>
      <br>
      <span>5. rd + name: Delete a empty catalogue</span>`)
let instruction = ref('')
const activeName = ref('1')
let bitmap = reactive([1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0])
const drawer = ref(false);
const handleClose = (done) => {
  instruction.value = ''
  done();
};
let path = ref('root')
let bs = reactive([[{
  file_name: '',
  i_node: ''
}], [], [], [], [], [], [], [], [], []])
let inodes = reactive([{
  id: 0,
  type: "catalogue",
  block: 2,
  size: 0,
  date: new Date().toISOString()
}])
let fat = reactive([{
  block: 2,
  next: -1
}])
let current = 0
const md = (name) => {
  // 找到空闲盘块, 填充fat表
  let i = 0
  for (i; i < bitmap.length; i++) {
    if (bitmap[i] === 0) {
      bitmap[i] = 1
      fat.push({
        block: i,
        next: -1
      })
      break
    }
  }

  // 添加i_node结点
  inodes.push({
    id: inodes.length,
    type: 'catalogue',
    block: i,
    size: 0,
    date: new Date().toISOString()
  })

  // 在创建文件的目录下新增目录项
  bs[current].push({
    file_name: name,
    i_node: inodes.length - 1
  })
}

const cd = (name) => {
  path.value += '\\' + name
  for (const b of bs[current]) {
    if (b.file_name === name) {
      for (const inode of inodes) {
        if (inode.id == b.i_node) {
          current = inode.block - 2
        }
      }
    }
  }
}

const mk = (name, size) => {
  // 找到空闲盘块, 填充fat表
  let block_number = Math.ceil(size / 50)
  let selected_blocks = []
  for (let i = 0; i < bitmap.length, selected_blocks.length < block_number; i++) {
    if (bitmap[i] === 0) {
      bitmap[i] = 1
      selected_blocks.push(i);
    }
  }
  for (let i = 0; i < selected_blocks.length; i++) {
    let next = selected_blocks[i + 1] > 0 ? selected_blocks[i + 1] : -1
    fat.push({
      block: selected_blocks[i],
      next: next
    })
  }

  // 添加i_node结点
  inodes.push({
    id: inodes.length,
    type: 'file',
    block: selected_blocks[0],
    size: size,
    date: new Date().toISOString()
  })

  // 在创建文件的目录下新增目录项
  bs[current].push({
    file_name: name,
    i_node: inodes.length - 1
  })
  for (const inode of inodes) {
    if (current + 2 === inode.id) {
      inode.size += size
    }
  }

  // 写磁盘
  for (const selectedBlock of selected_blocks) {
    bs[selectedBlock - 2].push({
      file_name: name,
      i_node: 'data'
    })
  }
  for (const inode of inodes) {
    if (inode.id === bs[current].i_node) inode.size += size
  }
}

const del = (name) => {
  let item = -1
  let b = bs[current]
  for (let i = 0; i < b.length; i++) {
    if (b[i].file_name === name) {
      for (const inode of inodes) {
        if (b[i].i_node == inode.id) {
          if (inode.size === 0) {
            alert("must empty!")
            return
          }
          bs[inode.block - 2] = []
          bitmap[inode.block] = 0

          let next = -1
          for (const fatElement of fat) {
            if (fatElement.block === inode.block) {
              next = fatElement.next
            }
          }
          while (next != -1) {
            bs[next - 2] = []
            bitmap[next] = 0
            for (const fatElement of fat) {
              if (fatElement.block === next) {
                next = fatElement.next
              }
            }
          }
        }
      }
      item = i;
    }
  }
  inodes.splice(b[item].i_node, 1);
  b.splice(item, 1)
}

const rd = (name) => {
  let b = bs[current]
  let item = -1
  for (let i = 0; i < b.length; i++) {
    if (b[i].file_name === name) {
      for (const inode of inodes) {
        if (b[i].i_node == inode.id) {
          // 清空物理块数据
          bs[inode.block - 2] = []
          bitmap[inode.block] = 0
        }
      }
      // 记录目录中该条目录项位置
      item = i;
    }
  }
  // 移除i_nodes表中对应条目
  inodes.splice(b[item].i_node, 1);
  // 移除记录项
  b.splice(item, 1)
}
const run = () => {
  let split = instruction.value.split(' ')
  instruction.value = ''
  if (split[0] === 'cd') cd(split[1].trim())
  else if (split[0] === 'md') md(split[1].trim())
  else if (split[0] === 'mk') mk(split[1].trim(), split[2])
  else if (split[0] === "del") del(split[1].trim())
  else if (split[0] === 'rd') rd(split[1].trim())
}

</script>

<style scoped>

</style>