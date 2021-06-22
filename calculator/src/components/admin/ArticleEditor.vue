<template>
  <div>
    <el-row>
      <el-input
          v-model="article.articleTitle"
          style="margin: 10px 0px;font-size: 18px;"
          placeholder="请输入标题"></el-input>
    </el-row>
    <el-row style="height: calc(100vh - 140px);">
      <v-md-editor v-model="article.articleContentMd" @save="saveArticles" height="400px"></v-md-editor>
    </el-row>
  </div>

  <!--  <v-md-editor v-model="markdown" mode="preview"></v-md-editor>-->
  <!--  <v-md-preview-html :html="html" preview-class="vuepress-markdown-body"></v-md-preview-html>-->
</template>

<script setup>
import {ref, reactive} from 'vue'

let article = reactive({})
/*let markdown = ref('### yes')*/


const saveArticles = (value, render) => {
  // value 是 md，render 是 html
  confirm('是否保存并发布文章?', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
        this.$axios
            .post('/admin/content/article', {
              id: article.id,
              articleTitle: article.articleTitle,
              articleContentMd: value,
            }).then(resp => {
          if (resp && resp.status === 200) {
            message({
              type: 'info',
              message: '已保存成功'
            })
          }
        })
      }
  ).catch(() => {
    message({
      type: 'info',
      message: '已取消发布'
    })
  })
}

</script>

<style scoped>

</style>